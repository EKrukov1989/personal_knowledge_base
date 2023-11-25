___
### 1. Поиск файлов

Команда `find_file()` выполняет поиск файлов из заданного списка имен в заданном списке локаций и имеет следующий синтаксис: 
```
find_file(outVar
	name | NAMES name1 [name2...]
	[HINTS path1 [path2...] [ENV var]...]
	[PATHS path1 [path2...] [ENV var]...]
	[PATH_SUFFIXES suffix1 [suffix2 ...]]
	[NO_DEFAULT_PATH]
	[NO_PACKAGE_ROOT_PATH]
	[NO_CMAKE_PATH]
	[NO_CMAKE_ENVIRONMENT_PATH]
	[NO_SYSTEM_ENVIRONMENT_PATH]
	[NO_CMAKE_SYSTEM_PATH]
	[CMAKE_FIND_ROOT_PATH_BOTH |
	ONLY_CMAKE_FIND_ROOT_PATH |
	NO_CMAKE_FIND_ROOT_PATH]
	[DOC "description"]
)

# short form
find_file(outVar name [path1 [path2...]])
```
- `name` -> имя файла
- `NAMES name1 [name2...]` -> список возможных имен файлов, наиболее предпочтительные должны быть слева
- `HINTS`, `PATHS` -> локации в которых будет осуществлен поиск файла. `PATH` обычно содержит заранее определенные пути, а `HINTS` - пути, которые конструируются на основе каких-то переменных
- Cmake ищет имя во всех локациях и только затем переходит к следующему имени.

Порядок поиска в локациях:

1. Package root variables
	- Используется только если `find_file()` вызвана внутри find-модуля.
	- Может быть отключена с помощью опции `NO_PACKAGE_ROOT_PATH`.
	- Команда `find_package()` может быть вызвана рекурсивно. При каждом вызове `find_package()` список префиксов сохраняется в переменную `<packageName>_ROOT`, которая добавляется в стэк. Таким образом этот стэк хранит все префиксы характерные для предыдущих вызовов.
	- Cmake выполняет поиск по всем этим префиксам так же, как это описано в следующем пункте.

2. Cache variables (CMake-specific)
	- Поиск в локациях, которые указаны с помощью `CMAKE_PREFIX_PATH`, (а еще `CMAKE_INCLUDE_PATH`, `CMAKE_FRAMEWORK_PATH`)
	- может быть отключена с помощью `NO_CMAKE_PATH`
	- Переменная `CMAKE_PREFIX_PATH` содержит список префиксов
	- Для `find_file()` будет произведен поиск в локациях `<prefix>/include` для каждого из префиксов, указанных в `CMAKE_PREFIX_PATH`
	- по умолчанию `CMAKE_PREFIX_PATH` пуста

3. Environment variables (CMake-specific)
	- Поиск в локациях, которые указаны с помощью переменных окружения `CMAKE_PREFIX_PATH`, (а еще`CMAKE_INCLUDE_PATH`, `CMAKE_FRAMEWORK_PATH`)
	- Может быть отключена с помощью опции `NO_CMAKE_ENVIRONMENT_PATH`.

4. Пути, указанные после ключевого слова `HINTS`

5. Environment variables (system-specific)
	- Поиск в локациях, которые указаны с помощью переменных окружения `INCLUDE` и `PATH`
	- Может быть отключена с помощью опции `NO_CMAKE_ENVIRONMENT_PATH`.

6. Cache variables (platform-specific)
	- Поиск в локациях, указанных в переменных `CMAKE_SYSTEM_PREFIX_PATH`, `CMAKE_SYSTEM_INCLUDE_PATH` и `CMAKE_SYSTEM_FRAMEWORK_PATH`
	- Предполагается, что cmake автоматически выставит эти переменные в процессе настроки platform toolchain
	- Может быть отключена с помощью опции `NO_CMAKE_SYSTEM_PATH`.

8. Пути, указанные после ключевого слова `PATHS`

___
### 2. Поиск директорий

Команда `find_path()` функционирует ровно таким же образом как и  `find_file()`, только возвращает не путь к файлу, а путь к директории в которую этот файл находится.

___
### 3. Поиск программ

В целом `find_program()` работает так же как и `find_file()`. Список отличий:
- Вместо `CMAKE_INCLUDE_PATH` используется `CMAKE_PROGRAM_PATH`
- Для каждого из префиксов, указанных в `CMAKE_PREFIX_PATH` производит поиск в локациях `<prefix>/sbin` (вместо `<prefix>/include`)
- Переменная окружения `INCLUDE` не используется при поиске программ
- При использовании `NAMES_PER_DIR` для локации проверяются все имена, а только потом происходит переход к следующей локации

___
### 4. Поиск библиотек

В целом `find_library()` работает так же как и `find_file()`. Список отличий:
- Для каждого из префиксов, указанных в `CMAKE_PREFIX_PATH` производит поиск в локациях `<prefix>/lib` (вместо `<prefix>/include`)
- `CMAKE_INCLUDE_PATH` вместо `CMAKE_INCLUDE_PATH`
- Опция `NAMES_PER_DIR` доступна так же как для `find_program()`
- Вместо переменной окружения `INCLUDE` используется переменная окружения `LIB`

____
### 5. Поиск пэккаджей

Существует два вида пэккаджей:
- модулей (подробнее тут -> [[cmake - module]])
- конфиг-пэккаджи (пэккаджи определенные с помощью конфиг-файла)

Важные различия между модулями и конфиг-пэккаджами:
- Конфиг обычно находится внутри пэккаджа и пишется его разработчиками, модуль обычно пишется сторонними разработчиками
- Модуль ищется в `CMAKE_MODULE_PATH` и внутренней директории с модулями, а конфиг ищется в куче директорий включая пэккадж регистры!

Команда `find_package` применяется как для поиска модулей так и конфиг пэккаджей.
Форма команды для поиска модулей и конфиг-пэккаджей:
```
find_package(packageName
	[version [EXACT]]
	[QUIET] [REQUIRED]
	[[COMPONENTS] component1 [component2...]]
	[OPTIONAL_COMPONENTS component3 [component4...]]
	[MODULE]
	[NO_POLICY_SCOPE]
)
```
- `EXACT` -> пэккадж должен быть строго указанной версии (по умолчанию - указанной версии или выше)
- `REQUIRED` -> данный пуккадж обязателен
- `COMPONENTS`, `OPTIONAL_COMPONENTS` -> список нужных компонентов пэккаджа

Форма команды только для поиска конфиг-пэккаджей:
```
find_package(packageName
	[version [EXACT]]
	[QUIET | REQUIRED]
	[[COMPONENTS] component1 [component2...]]
	[NO_MODULE | CONFIG]
	[NO_POLICY_SCOPE]
	[NAMES name1 [name2 ...]]
	[CONFIGS fileName1 [fileName2...]]
	[HINTS path1 [path2 ... ]]
	[PATHS path1 [path2 ... ]]
	[PATH_SUFFIXES suffix1 [suffix2 ...]]
	[CMAKE_FIND_ROOT_PATH_BOTH |
	ONLY_CMAKE_FIND_ROOT_PATH |
	NO_CMAKE_FIND_ROOT_PATH]
	[<skip-options>]
	# See further below
)
```
Если в команде указать какую-то опцию, которая не характерна для первой формы, то команда будет искать конфиг-пэккаджи. Лучше всего всегда указывать в таких случаях опции `NO_MODULE` или `CONFIG`.

В этом случае cmake ищет файл конфига:
- c именем `<packageName>Config.cmake`
- или с именем `<lowercasePackageName>-config.cmake` 
- или с именами, которые указаны после ключевого слова `CONFIGS` (не рекомендуется)

Когда файл конфига найден, то cmake ищет файл версии с именем:
- `<configfilebasename>Version.cmake` или `<configfilebasename>-version.cmake`

Порядок поиска в целом похож на `find_file()`, кроме того что дополнительно поиск производится в пэккадж-регистрах.

Наверное самое примечательное отличие `find_package` от других find-команд следующее:
- Cmake производит поиск не в какой-то одной директории (например `<prefix>/include` или `<prefix>/lib`), а в довольно большом наборе директорий, которые определены с помощью регулярных выражений и специфичны для своих платформ. Перечислять их здесь нет смысла.

Когда будет найден подходящий файл, то кэш-переменная `<packageName>_DIR` будет установлена соответствующим образом. Если `<packageName>_DIR` установлена и по указанному в ней пути находится конфиг-файл, то `find_package` возвращает значение переменной `<packageName>_DIR`.
___
### 6. FindPkgConfig

Еще один способ подключения пэккаджей.
Настолько запутанный, что даже не буду пытаться его описать.
