___
Статья написана на основе `man ssh-keygen.1`. и [redhat_ssh_certificates](https://access.redhat.com/documentation/ru-ru/red_hat_enterprise_linux/6/html/deployment_guide/sec-signing_ssh_certificates)
В данной статье рассмотрен процесс настройки системы аутентификации на основе ssh-сертификатов.
Теоретические принципы изложены здесь: [[_digital certificates]]
Особенности работы с ssh-keygen: [[ssh - ssh-keygen]]

В системе есть три действующих лица:
- юзер `cuser (CA)`, который расположен на `chost`
- юзер `auser (Alice)`, которая работает на `ahost`
- юзер `buser (Bob)`, который работает на `bhost`
В результате настройки системы `Alice` с машины `ahost` сможет подклються к машине `bhost` под именем `Bob`.

___
### 1. Создаем ключи CA

Хотя можно использовать одну пару ключей, принято использовать две: одну для пользователей и другую для хостов.
```bash
ssh-keygen -f ca_user_sign_key
ssh-keygen -f ca_host_sign_key
```

___
### 2. Создаем сертификаты для CA

```bash
ssh-keygen -s ca_user_sign_key -I cuser_cert_id    cuser_key.pub
ssh-keygen -s ca_host_sign_key -I chost_cert_id -h chost_key.pub
```
Пара ключей `cuser_key` обычно генерируется вручную пользователем, а в качестве пары `chost_key` лучше использовать уже сгенерированную пару ключей: `/etc/ssh/ssh_host_rsa_key`.

В пункте 7 в примере сертификаты CA не используются. Но, более правильно подписывать сертификаты юзера и хоста сертификатом CA.

___
### 3. Создание сертификата для хоста

- Хост должен сгенерировать свою пару ключей: `bhost_key`. Или использовать предварительно сгенерированные.
- Затем публичный ключ должен быть передан на машину CA, например с помощью scp.
- Создать сертификат для хоста
```bash
ssh-keygen -s ca_host_sign_key -I bhost_cert_id \
-z serial_number -n host_name.example.com -V -1d:+54w -h bhost_key.pub
```
- Сертификат будет назван `bhost_key-cert.pub` и его нужно передать обратно на `bhost`.
- На `bhost` нужно добавить в `/etc/ssh/sshd_config` следующую строку: `HostCertificate /etc/ssh/bhost_key-cert.pub`.
- `systemctl reload ssh`
Теперь `bhost` будет предоставлять сертификат всем, кто попробует подключиться к нему по ssh.

___
### 4. Создание сертификата для юзера

- Создать сертификат для юзера
```bash
ssh-keygen -s ca_user_sign_key -I auser_cert_id \
-z serial_number -V -1d:+54w -h auser_key.pub
```
По умолчанию юзеру разрешен удаленный вход, если один из юзеров, указанных в сертификате совпадает с именем юзера в системе. 

>[!note]
>Конечно, это подразумевает, что имена юзерам назначаются централизовано и никаких коллизий быть не может.

___
### 5. Настройка хоста

Хост должен доверять `ca_user_sign_key`. Для этого нужно переместить ключ на этот хост и добавить в `/etc/ssh/sshd_config` следующую строку:
```
TrustedUserCAKeys /etc/ssh/ca_user_sign_key.pub
```
Перезагрузить сервис: `systemctl reload ssh`

Или можно сделать такую настройку для определенного пользователя на этом хосте. Для этого нужно добавить в файл `~/.ssh/authorized_keys` следующую строчку:
```bash
cert-authority,principals="name1,name2" <content_of_ca_user_sign_key.pub>
```
где
- `principals` - список паттернов для пользователей

Это означает, что юзер доверяет сертификатам CA, выпущенным для хостов `*.example.com` для пользователей `name1`, `name2`

Можно создать файл `AuthorizedPrincipalsFile`, добавить туда имена всех принципалов, которым будет разрешен вход. При это не забыть 
в файле `/etc/ssh/sshd_config`  добавить строку: `AuthorizedPrincipalsFile <filepath>`

По умолчанию юзеру разрешен удаленный вход, если один из юзеров, указанных в сертификате совпадает с именем юзера в системе. 

При попытке аутентификации хост должен отправлять свой сертификат. Для этого  на `bhost` добавляем в `/etc/ssh/sshd_config` следующую строку: `HostCertificate /etc/ssh/ssh_host_rsa_key-cert.pub`.
Перезагружаем sshd: `systemctl reload ssh`

___
### 6. Настройка юзера

Юзер должен доверять `ca_host_sign_key`.
Для этого нужно добавить в файл `~/.ssh/known_hosts` следующую строчку:
```bash
# A CA key, accepted for any host in *.example.com
@cert-authority *.example.com <content_of_ca_host_sign_key.pub>
```
где 
- `*.example.com` - список паттернов для хостов

На `ahost` добавляем в `~/.ssh/config` следующие строки:
`IdentityFile /home/auser/.ssh/auser_rsa_key`
`CertificateFile /home/auser/.ssh/auser_rsa_key-cert.pub
`AddKeysToAgent Ask`

___
### 7. Пример

Пробуем настроить подключение от юзера `gene@apire` к хосту `nwtpc`.

Создаем ключи для CA и юзера:
`ssh-keygen -f ca_user_sign_rsa_key -C ca_user_sign`
`ssh-keygen -f ca_host_sign_rsa_key -C ca_host_sign`
`ssh-keygen -f gene_user_rsa_key -C gene@aspire`
Для хоста у нас уже есть дефолтные ключи `ssh_host_rsa_key`.

Создаем сертификат для юзера:
`ssh-keygen -s ca_user_sign_rsa_key -I gene@aspire_rsa_1 -z 201 -n gene@aspire -V -1d:+54w gene_user_rsa_key.pub`
Создаем сертификат для хоста:
`ssh-keygen -s ca_host_sign_rsa_key -I nwtpc_rsa_1 -z 202 -n nwtpc -V -1d:+54w -h ssh_host_rsa_key.pub`
Получаю два сертификата:
`gene_user_rsa_key-cert.pub`
`ssh_host_rsa_key-cert.pub`

Сертификат `gene_user_rsa_key-cert.pub` кладу в `~/.ssh` на хосте `aspire`.
Сертификат `ssh_host_rsa_key-cert.pub` кладу в `/etc/ssh` на хосте `nwtpc`.

На `nwtpc` добавляем в файл `~/.ssh/authorized_keys` следующее:
`cert-authority,principals="gene@aspire" <content_of_ca_user_sign_key.pub>`
На `nwtpc` добавляем в `/etc/ssh/sshd_config` следующую строку: `HostCertificate /etc/ssh/ssh_host_rsa_key-cert.pub`.
Перезагружаем sshd: `systemctl reload ssh`

На `aspire` добавляем в файл `/home/gene/.ssh/known_hosts` следующее:
`@cert-authority nwtpc <content_of_ca_host_sign_key.pub>`
На `aspire` добавляем в `/home/gene/.ssh/config` следующие строки:
`IdentityFile /home/gene/.ssh/gene_user_rsa_key`
`CertificateFile /home/gene/.ssh/gene_user_rsa_key-cert.pub
`AddKeysToAgent Ask`

>[!tip]
>Про формат `authorized_keys` и `known_hosts` можно почитать в `man sshd`.

