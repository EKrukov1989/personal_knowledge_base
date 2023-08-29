___
Смещение позиционных параметров, с помощью команды shift
Например:
```bash
printf "len:%s, args:%s\n" $# "$@" # вывод: len:4, args:aaa bbb ccc ddd
shift 2
printf "len:%s, args:%s\n" $# "$@" # вывод: len:2, args:ccc ddd
```
