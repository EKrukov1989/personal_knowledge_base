___
В данной статье описаны только возможности утилиты ssh-keygen, без объяснения зачем эти возможности нужны.

___
### 1. Генерация пары ключей

Для подключения по SSH требуется пара ключей - публичный и приватный. Сгенерировать эти ключи можно с помощью команд:
```bash
cd ~/.ssh
ssh-keygen -t rsa -f filename
```

Ключи принято хранить в директории `~/.ssh` или `/etc/ssh`. Данная команда запрашивает `passphrase` для шифрования приватного ключа и создает в текущей директории два RSA-ключа: `filename` и `filename.pub`.

>[!note]
>Обычно используют одну пару ключей для аутентификации, а вторую для подписи. Хотя сгенерировання таким образом пара ключей подходит как для подписи, так и для аутентификации.

`-t dsa | ecdsa | ecdsa-sk | ed25519 | ed25519-sk | rsa(default)`
-   задать тип ключа

___
### 2. Фингерпринты

Фингерпринт это всего лишь результат применения определенной хэш-функции к публичному ключу. Fingerprint короче, поэтому его проще передавать и проще проверить визуально. Выглядит fingerprint следующим образом:
`SHA256:kcKfPoFPuqCZM/WvpOZ4KVBs4/fJTQutQNHlBD4l8no gene@aspire`

Конечно, есть команда, с помощью которой можно вычислить значение fingerprint для определенного публичного ключа:
```bash
ssh-keygen -lf filename.pub
3072 SHA256:kcKfPoFPuqCZM/WvpOZ4KVBs4/fJTQutQNHlBD4l8no gene@aspire (RSA)
```

___
### 3. Выпуск сертификатов

ssh-сертификат это цифровой сертификат, который используется для установления ssh-соединений ([[_digital certificates]]).

С помощью утилиты ssh-keygen можно выпустить сертификаты двух типов:
- сертификаты пользователя
- сертификаты хоста (опция `-h`)

Нужно выбрать пару ключей, которые теперь будут использоваться для подписи сертификатов и можно выпускать сертификаты:
```bash
ssh-keygen -s /path/to/ca_key -I key_id /path/to/user_key.pub
ssh-keygen -s /path/to/ca_key -I key_id -h /path/to/host_key.pub
```

В результате выполнения этих команд будут созданы сертификаты `/path/to/user_key-cert.pub` и `/path/to/host_key-cert.pub`, соответственно.

Опции:
`-n principals`
- ограничить список возможных владельцев
- по умолчанию такие сертификаты действительны для любого пользователя или хоста (конечно, если он владеет соответствующим приватным ключом)
`-z serial_number`
- указать серийный номер сертификата
`-V validity_interval`
- указать срок действия сертификата

___
### 4. Чтение сертификатов

Если открыть сертификат в текстовом редакторе, то он выглядит вот так:
```
ssh-rsa-cert-v01@openssh.com AAAAHHNzaC1yc2EtY2VydC12MDFAb3BlbnNzaC5jb20AAAAgnG/K7x+LpkVhhJrkXhkR8Bi
...
+cnLctXs8paWvfZhyMiT1vhgGf79VmcI1Ji15etcGNN gene@aspire
```

Чтобы прочитать информацию из сертификата нужно воспользоваться следующей командой:
```bash
ssh-keygen -L -f /path/to/user_key-cert.pub
```
Возможный вывод:
```
rsa_test_user-cert.pub:
        Type: ssh-rsa-cert-v01@openssh.com user certificate
        Public key: RSA-CERT SHA256:4kXz0U7q1SgHJ6y7ap8mf834YPFJxq30ncde55ttz4Y
        Signing CA: RSA SHA256:KXo2+OrC/UXEkcjQU1qhzc5jLGecBTQs/DT2J5KxJrA (using rsa-sha2-512)
        Key ID: "this_user"
        Serial: 0
        Valid: forever
        Principals: (none)
        Critical Options: (none)
        Extensions: 
                permit-X11-forwarding
                permit-agent-forwarding
                permit-port-forwarding
                permit-pty
                permit-user-rc
```

___
### 5. Key Revocation Lists

Key Revocation Lists - список ключей или сертификатов, подлежащих отзыву.
Если ключ был добавлен в KRL, то он уже не может быть использован в целях аутентификации.

OpenSSH не предлагает способов распространения KRL, поэтому KRL нужно скопировать на хост и добавить в `sshd_config` секцию RevokedKeys.
```
RevokedKeys /etc/ssh/sshd_revoked_keys"
```
(и не забыть перезагрузить сервис: `systemctl reload ssh`)

С помощью утилиты ssh-keygen можно выпустить KRL листы. Например, чтобы создать KRL, содержащий `key1`, `key2`, `cert1`, `cert2` нужно выполнить команду:
```bash
ssh-keygen -k -f revoked_keys key1 key2 cert1 cert2
```

`-Q`
- проверить, есть ли определенную ключ в KRL

___
### 6. Подпись файлов и верификация файлов

На эту тему есть отдельная статья: [[ssh - sign and verify practice]].


