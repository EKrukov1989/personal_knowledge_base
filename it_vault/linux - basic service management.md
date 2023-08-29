>[!warning]
>Тема слишком обширная, поэтому в этой статье я рассмотрю только основы управления сервисами в linux - как мониторить, запускать, останавливать и т.д. Статья написана на основе:
>[systemd-essentials-working-with-services-units-and-the-journal](https://www.digitalocean.com/community/tutorials/systemd-essentials-working-with-services-units-and-the-journal)

### 1. Коротко о systemd

systemd - подсистема инициализации и управления службами в Linux. systemd запускается во время early boot и затем запусает все остальные процессы.

systemd управляет самыми разными объектами, которые обобщенно называются юнитами. Есть 11 типов юнитов, но в этой статье будут рассматриваться только объекты типа service.

### 2. Основые возможности systemctl

systemctl (systemd and service manager control) - утилита для мониторинга и управления состоянием systemd и менеджера сервисов

##### 2.1. Синтаксис

`systemctl [OPTIONS...] COMMAND [UNIT...]`

##### 2.2. Основные команды

`start <unitname>`, `stop <unitname>`
- запустить/остановить (activate) указанный юнит
`enable <unitname>`, `disable <unitname>`
- включить/отключить запуск сервиса на старте системы
`list-units`
- вывести список юнитов, которые в настоящее время активны
`list-units --all`
- вывести список юнитов, которые systemd загрузило или пыталось загрузить, даже те, которые в настоящий момент неактивны
`list-unit-files`
- вывести список всех юнитов, которые были установлены в системе
- выводит информацию о `STATE` и `PRESET`. `STATE` может быть `enabled` или `disabled`. `PRESET` показывает значение по умолчанию.
`status <servicename>.service`
- вывести описание текущего состояние юнита
`cat <servicename>.service`
- вывести содержимое юнит-файла
`list-depenedcies <servicename>.service`
- вывести дерево зависимостей для сервиса, т.е. юниты от которых сервис зависит
`list-depenedcies --all <servicename>.service`
- вывести все зависимости сервиса: ..юниты от которых зависит сервис, а также юниты, которые зависят от сервиса
`show <servicename>.service`
- вывести низкоуровненные настройки юнита

### 3. Коротко о systemd-journald

systemd запускает в том числе и сервис systemd-journald.service, который собирает и хранит логи. Он создает и поддерживает структурированные, индексированные журналы. Основные команды journalctl:

`journalctl`
- вывести все логи
`journalctl -b`
- вывести все логи с момент загрузки системы
`journalctl -k`
- только сообщений ядра (kernel)
`journalctl -u <servicename>.service`
- вывести сообщения для определенного юнита


