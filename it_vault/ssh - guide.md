>[!note] Предисловие
>SSH-система состоит из большого количества компонентов, которые тесно взаимосвязаны друг с другом. В данной статье собраны ссылки на статьи по SSH, приведен наулучший порядок из изучения, а также рекомендации.

___
Лучше всего начать изучение со статьи: [[ssh - password practice]]
Если Вы не собираетесь пользоваться ключами ля аутентификации, то на этом изучение SSH можно закончить.
___
Общая информация про публичные и приватные ключи, фингерпринты, а также их генерацию: [[ssh - public and private keys]]
___
Самая необходимая информация про инфраструктуру SSH - конфигурационные файлы, места хранения ключей, authorized_hosts и known_hosts: [[ssh - key infrastructure]]
___
Прежде чем переходить к практике нужно хотя бы немножко познакомиться с ssh-клиентом: [[ssh - ssh-client basics]]
___
На этом этапе можно уже пользоваться аутентификацией с помощью пары ключей. В статье [[ssh - keypair practice]] описывается самые тривиальные варианты подключения.
___
Чтобы не вводить passphrase для приватного ключа, нужно использовать ssh-agent: [[ssh - ssh-agent and ssh-add]].
___
Если есть подключение по паролю, то публичный ключ на сервер можно передать с помощью команды ssh-copy-id: [[ssh - ssh-copy-id]].
___
Есть еще много разных возможностей и настроек:
- [[ssh - ssh-config]], [[ssh - sshd-config]]
___
Наиболеее продвинутый способ аутентификации - с помощью сертификатов: [[ssh - ssh-keygen]], [[ssh - certificate practice]]
___

Ссылки:
[how-to-use-ssh-to-connect-to-a-remote-server-ru](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-to-connect-to-a-remote-server-ru#)
[ssh-essentials-working-with-ssh-servers-clients-and-keys](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)

Возможно в будущем будет полезно разобрать:
- ssh-keysign
- ssh-keyscan
- ssh-askpass
