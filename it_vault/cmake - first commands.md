___
```cmake
cmake_minimum_required( VERSION X )
```
- запрещает использовать файл для cmake версии ниже указанной X
- заставляет cmake обрабатывать файл ровно таким образом, как это бы сделал cmake версии X
- данная команда должна быть на первой строчке файла

```cmake
project( projectName [VERSION X] [LANGUAGES l1 l2 ...] )
```
- данная команда должна идти после `cmake_minimum_required`
- проверяет что есть необходимые компиляторы и линкеры
- по умолчанию `LANGUAGES == C CXX`