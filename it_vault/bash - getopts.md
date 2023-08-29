___
В bash есть встроенная команда getopts, которая специально предназначена для парсинга опций.

Аргументы команды, которые мы собираемся парсить с помощью getopts, 
должны удовлетворять следующим требованиям:

- cинтаксис: `cmdname -a -b opt_arg -c arg1 arg2 arg3`
- опция может быть только короткой: дефис + одна буква
- у некоторых опций могут быть свои аргументы
- все опции должны быть перечислены до аргументов команды
- несколько опций без аргументов могут быть соединены, но если опция принимает аргумент, то она должна идти последней:
   `-a -b b_arg` -> `-ab b_arg` 
- аргумент опции может следовать с или без разделяющего уайтспейса

Пример работы команды:

```bash
#!/bin/bash

## this script expects:
## option -a without arguments
## option -b with one argument
## and zero or more usual arguments

## Default values
a=0
b=

## List of options the program will accept;
## those options that take arguments are followed by a colon
optstring=ab:

## The loop calls getopts until there are no more options on the command line. Each option is stored in $opt, any option arguments are stored in OPTARG
while getopts $optstring opt
do
	case $opt in
		a) a=1 ;;
		b) b=$OPTARG ;; ## $OPTARG contains the argument to the option
		*) exit 1 ;;
	esac
done

## Remove options from the command line
## $OPTIND points to the next, unparsed argument
shift "$(( $OPTIND - 1 ))"

printf "options:\n"
printf "a=%s\n" $a
printf "b=%s\n" $b
printf "positional params:\n"
printf "%s\n" $@
printf "finish.\n"
```
