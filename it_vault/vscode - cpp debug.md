___
Всегда можно отладить С++ программу с помощью gdb, но иногда удобнее использовать встроенный графический интерфейс. В этой статье рассмотрены базовые настройки для отладки в vs code.
___
### 1. Приготовления

Чтобы приступить к отладке нужно выполнить следующие приготовления:
1. Установить `C/C++ extension`
2. Нужно создать файл `.vscode/launch.json` со следующим содержимым:
```json
{
	"version": "0.2.0",
	"configurations": [
		{
			"name": "custom_debug",
			"type": "cppdbg",
			"program": "${workspaceRoot}/app",
			"request": "launch",
			"cwd":"${workspaceRoot}"
		}
	]
}
```
`type` -> `cppvsdbg` для Visual Studio Windows debugger или`cppdbg` для GDB или LLDB
`request` -> `launch` или `attach`
___
### 2. Запуск

1. Открыть какой-нибудь `cpp`-файл.
2. Нужно убедиться, что комбобокс в правом верхнем углу находится в положении debug:
![[vsdebug.png]]
3. Начать сеанс отладки можно с помощью клавиши `F5` Завершить сеанс отладки можно с помощью клавиши `Shift+F5`.