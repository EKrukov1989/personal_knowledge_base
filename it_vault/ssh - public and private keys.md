>[!note]
>В данной статье рассматриваются только практические аспекты: как создавать ключи, что они содержат и так далее.

___
### 1. Генерация пары ключей

Для подключения по SSH требуется пара ключей - публичный и приватный. Сгенерировать эти ключи можно с помощью команд:

```bash
cd ~/.ssh
ssh-keygen -f filename -t key_type -C comment
```

`key_type` может принимать следующие значения:
`dsa | ecdsa | ecdsa-sk | ed25519 | ed25519-sk | rsa`
По умолчанию - `rsa`.

Данная команда запрашивает `passphrase` для шифрования приватного ключа и создает в текущей директории два RSA-ключа: `filename` и `filename.pub`. Ключи принято хранить в директории `~/.ssh` (для юзера) или `/etc/ssh` (для хоста).

Сгенерировання пара ключей подходит как для подписи, так и для аутентификации.

___
###  2. Приватный ключ

Внешний вид приватного ключа:
```text
-----BEGIN OPENSSH PRIVATE KEY-----
<long_string_base64encoded>
-----END OPENSSH PRIVATE KEY-----
```
Приватный ключ хранится в зашифрованном виде и ничего понять в нем нельзя.

___
### 3. Публичный ключ

Внешний вид публичного ключа:
```text
ssh-rsa <long_string_base64encoded> comment
```

Здесь мы видим, что в публичном ключе указаны:
- алгоритм ассиметричного шифрования
- комментарий

___
### 4. Комментарии

Если при генерации ключа не указать коментарий, то по умолчанию комментарий будет следующий: `<username>@<hostname>`, то есть принципал.

Комментарий хранится не только в публичном ключе, но и в приватном ключе. Комментарии нужны для идентификации ключа, но не несут какой-то дополнительной нагрузки.

Можно сменить комментарий в обоих ключах с помощью опции `-c`:
```bash
ssh-keygen -c [-C comment] -f keyfile
```

___
#### 4. Fingerprint

Fingerprint это всего лишь результат применения определенной хэш-функции к публичному ключу. Fingerprint короче, поэтому его проще передавать и проще проверить визуально. Выглядит fingerprint следующим образом:
`SHA256:kcKfPoFPuqCZM/WvpOZ4KVBs4/fJTQutQNHlBD4l8no gene@aspire`

Конечно, есть команда, с помощью которой можно вычислить значение fingerprint для определенного публичного ключа:
```bash
ssh-keygen -lf filename.pub
3072 SHA256:kcKfPoFPuqCZM/WvpOZ4KVBs4/fJTQutQNHlBD4l8no gene@aspire (RSA)
```
