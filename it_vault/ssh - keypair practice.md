___
В статье [[ssh - password practice]] я довольно подробно разобрал все подготовительные этапы. Здесь же я буду рассматривать только специфические аспекты, касающиеся аутентификации с помощью пары ключей.

___
### 1. Authorized keys

В системе linux в файле `/home/username/.ssh/authorized_keys` приведен список публичных ключей, для которых разрешен удаленный доступ в систему от имени пользователя `username`.

Т.е. если пользователь обладает парой ключей и публичный ключ из этой пары внесен в `authorized_keys` на удаленной машине, то этот пользователь сможет подключиться удаленно.

Есть три способа добавления ключей.

##### 1.1. С помощью pipe

Если у нас есть доступ к удаленному компьютеру через ssh с авторизацией по паролю, то мы можем добавить файл используя команду:
```bash
cat ~/.ssh/id_rsa.pub | \
ssh username@remote_host \
"mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

##### 1.2. С помощью ssh-copy-id

Если у нас есть доступ к удаленному компьютеру через ssh с авторизацией по паролю, то мы можем добавить файл используя команду ssh-copy-id. 

Команда подключается к удаленному компьютеру по ssh и добавляет публичные ключи клиента в список авторизованных ключей сервера.

```bash
ssh-copy-id username@remote_host
ssh-copy-id -i identity_file username@remote_host
```

`identity_file` - публичный ключ. Если расширение `.pub` не будет указано, то команда добавит его автоматически.

##### 1.3. Другим способом

Публичный ключ можно передать любым другим способом и вручную скопировать его в файл `authorized_keys`.

___
### 2. Подключение

Подключиться теперь можно без пароля от аккаунта `username`, с помощью команды:
```bash
ssh username@remote_host
```
Однако теперь требуется passphrase от приватного ключа. Чтобы избежать этого, следует воспользоваться ssh-agent ([[ssh - ssh-agent and ssh-add]]).
