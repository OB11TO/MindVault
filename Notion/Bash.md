---
modified: 2024-09-02T15:07:31+03:00
---
## Section 1: Shell basics

### Посмотреть версию релиза

```Java
cat /etc/*rel*
```

```Java
DISTRIB_ID=LinuxMint
DISTRIB_RELEASE=21.2
DISTRIB_CODENAME=victoria
DISTRIB_DESCRIPTION="Linux Mint 21.2 Victoria"
NAME="Linux Mint"
VERSION="21.2 (Victoria)"
ID=linuxmint
ID_LIKE="ubuntu debian"
PRETTY_NAME="Linux Mint 21.2"
VERSION_ID="21.2"
HOME_URL="https://www.linuxmint.com/"
SUPPORT_URL="https://forums.linuxmint.com/"
BUG_REPORT_URL="http://linuxmint-troubleshooting-guide.readthedocs.io/en/latest/"
PRIVACY_POLICY_URL="https://www.linuxmint.com/"
VERSION_CODENAME=victoria
UBUNTU_CODENAME=jammy
```

### Основные команды

![[images/Untitled 155.png|Untitled 155.png]]

![[images/Untitled 1 16.png|Untitled 1 16.png]]

  

### Переменные shell

_Понятие переменной очень распространено в любых языках программирования - это временное хранилище какой-то информации. Командные оболочки - не исключение, они точно так же могут хранить переменные, причем как локальные, так и глобальные, которые еще принято называть переменными окружения (Environment variables). В этом видео мы особое внимание уделим такой переменной окружения как PATH, благодаря которой в принципе операционные системы unix ищут и выполняют любые введенные нами команды._

- Локальные переменные

![[images/Untitled 2 15.png|Untitled 2 15.png]]

- `export` - установить глобальную переменную

![[images/Untitled 3 14.png|Untitled 3 14.png]]

  

### Файловая система

![[images/Untitled 4 14.png|Untitled 4 14.png]]

![[images/Untitled 5 14.png|Untitled 5 14.png]]

  

### Чтение текстовых файлов

![[images/Untitled 6 13.png|Untitled 6 13.png]]

  

  

###  Основы потоков ввода-вывода

- _Второй этап на пути к созданию своей_ `_environment variable_` _- это разобраться с основами потоков ввода-вывода в unix operating systems._
- Любые команды которые вводим ls и т.д. имеют входные и выходные параметры.
- Всё это массив байт

![[images/Untitled 7 12.png|Untitled 7 12.png]]

- Что можем использовать для input и какой будет output (часто)

![[images/Untitled 8 12.png|Untitled 8 12.png]]

- Перенаправление вывода output

![[images/Untitled 9 12.png|Untitled 9 12.png]]

- Дополнять файл (не перезаписывать)
    
    ![[images/Untitled 10 10.png|Untitled 10 10.png]]
    
- Объединить два потока вывода

![[images/Untitled 11 10.png|Untitled 11 10.png]]

  

###  Run Commands File

- Теперь можно _добавить свою собственную_ `_environment variable_` _на постоянной основе. И для этого существуют специальные зарезервированные имена файлов в зависимости от используемой командной оболочки:_ `_.bash_profile_` _и_ `_.bashrc_` _- для bash; .zprofile и .zshrc - для z shell._
- Как только мы логинимся в систему вызывается `bash_profile`
- И как только открываем `shell`, вызывается bashrc
- `evm` нужно добавить в эти файлы
- эти файлы относятся к пользователю конкретному.

![[images/Untitled 12 10.png|Untitled 12 10.png]]

![[images/Untitled 13 10.png|Untitled 13 10.png]]

- Нужно собрать source, чтобы сбилдился заново

![[images/Untitled 14 8.png|Untitled 14 8.png]]

  

### Pipes

![[images/Untitled 15 8.png|Untitled 15 8.png]]

![[images/Untitled 16 7.png|Untitled 16 7.png]]

  

![[images/Untitled 17 7.png|Untitled 17 7.png]]

  

![[images/Untitled 18 6.png|Untitled 18 6.png]]

###  Command history

- храниться история команд, которые вводил

![[images/Untitled 19 6.png|Untitled 19 6.png]]

  

## Section 2: File basics

###  Directory operations

![[images/Untitled 20 6.png|Untitled 20 6.png]]

  

###  File operations

![[images/Untitled 21 6.png|Untitled 21 6.png]]

### Inodes

![[images/Untitled 22 6.png|Untitled 22 6.png]]

```Bash
ls -i
```

- ==Метаинформация== хранится в `inode` таблице
- `-i` просто ==показывает id файла==, который ==уникальный== для каждого файла и ссылается на таблицу `iniode`

![[images/Untitled 23 6.png|Untitled 23 6.png]]

- ==Можно посмотреть вот так==

![[images/Untitled 24 5.png|Untitled 24 5.png]]

  

![[images/Untitled 25 5.png|Untitled 25 5.png]]

  

- `idonde table не бесконечная!!!!!`

![[images/Untitled 26 5.png|Untitled 26 5.png]]

### Hard & soft links

- `hard link` ==ссылается на тот же== ==inode==

![[images/Untitled 27 5.png|Untitled 27 5.png]]

- `soft link` ==указывает на путь к исходному файлу== (по типу ==ярлыка==)

![[images/Untitled 28 5.png|Untitled 28 5.png]]

  

- Создать `soft link`
    
    ```Bash
    ln -s файл название ссылки
    ```
    
    ![[images/Untitled 29 5.png|Untitled 29 5.png]]
    
- Создать `hard link`

![[images/Untitled 30 5.png|Untitled 30 5.png]]

###  File Text Manipulation

![[images/Untitled 31 5.png|Untitled 31 5.png]]

- `cut`

![[images/Untitled 32 5.png|Untitled 32 5.png]]

![[images/Untitled 33 5.png|Untitled 33 5.png]]

  

- ==Интересная комбинация==
- ==Позволяет записать в файл начиная со второй строчки==

![[images/Untitled 34 5.png|Untitled 34 5.png]]

  

- `sort`

![[images/Untitled 35 5.png|Untitled 35 5.png]]

- ==Сортирует как числа==
- `-r` - обратная
- `uniq - с` (количество дупликатов)

![[images/Untitled 36 5.png|Untitled 36 5.png]]

![[images/Untitled 37 5.png|Untitled 37 5.png]]

### grep & egrep

`_Регулярные выражения_` _- это невероятно мощное и часто встречаемое средство_ ==_для извлечения нужных данных из текстовой информации._==

Ищет `the` в файле

![[images/Untitled 38 5.png|Untitled 38 5.png]]

  

- в `grep` ==нельзя писать регулярки==
- ==можно только через== `grep - E` ==аналог== ниже

![[images/Untitled 39 5.png|Untitled 39 5.png]]

  

### File Location

![[images/Untitled 40 5.png|Untitled 40 5.png]]

- `which` - ==показывает расположение программ==

![[images/Untitled 41 5.png|Untitled 41 5.png]]

  

- `find` - ==ищет уже обычные файлы==
- `-name` - по ==имени== ищет
- `-type` - о **==типу==** ищет
- `2> /dev/null` - ==перенаправить== ==поток ошибок== ==в файл== (черная дыра для мусора)

![[images/Untitled 42 5.png|Untitled 42 5.png]]

![[images/Untitled 43 5.png|Untitled 43 5.png]]

![[images/Untitled 44 5.png|Untitled 44 5.png]]