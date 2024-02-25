___
### 1.  Установка в Ubuntu

```bash
sudo sh -c "wget -O - https://dl.openfoam.org/gpg.key > /etc/apt/trusted.gpg.d/openfoam.asc"
sudo add-apt-repository http://dl.openfoam.org/ubuntu
sudo apt-get update
sudo apt-get -y install openfoam11
```

Теперь OpenFOAM и ParaView должны быть установлены в директорию `/opt`.

Так же нужно добавить в `~/.bashrc`:
```bash
. /opt/openfoam11/etc/bashrc
```

___
### 2. Пример работы с OpenFOAM

`$FOAM_RUN` - рабочая директория для всех вычислений
`$FOAM_TUTORIALS` - директория в которой хранятся все примеры

>[!note]
>Если рабочая директория не создана, то нужно создать ее: `mkdir -p $FOAM_RUN`

Копируем нужный пример в рабочую директорию:
```bash
cd $FOAM_RUN
cp -r $FOAM_TUTORIALS/incompressibleFluid/pitzDailySteady .
```

Теперь в рабочей директории содержатся все исходные данные, необхимые для расчета.
___
##### 2.1. Генерация меши 

Файл `system/blockMeshDict` содержит всю необходимую информацию для построения меши и еще немного дополнительной информации (например имена для разных частей меши)

Можно создать меш с помощью команды:
```bash
cd $FOAM_RUN/pitzDailySteady
blockMesh
```
В результате будет создано несколько файлов в директории `constant/polymesh`.
___
##### 2.2. Просмотр меши

Оценить результат можно с помощью специальной программы - Paraview, которая поставляется вместе с OpenFOAM. Для этого следует выполнить команду:
```bash
cd $FOAM_RUN/pitzDailySteady
paraFoam &
```

>[!tip]
> Если возникает проблема с отображением в ParaView, которая сопровождается сообщением: `Mesa warning: Window 48234505 has no colormap!`, то нужно добавить в `~/.bashrc`::
> ```bash
> export ParaView_GL=system
> ```

___
#### 2.3. Расчет

Запустить расчет можно с помощью команды:
```bash
cd $FOAM_RUN/pitzDailySteady
foamRun
```

В результате выполнения этой команды должно появиться сообщение:
```
SIMPLE solution converged in 287 iterations
```
В директории `$FOAM_RUN/pitzDailySteady` появится несколько директорий с результатами вычислений в разные моменты времени.

>[!tip]
>Все эти директории можно удалить с помощью команды `foamListTimes -rm`

В этих директориях можно найти например: поле скоростей, и самое главное - поле статических давлений!

___
#### 2.4. Посмотр результатов расчета

Результаты расчета можно посмотреть в той же программе, что и меш:
```bash
paraFoam &
```
