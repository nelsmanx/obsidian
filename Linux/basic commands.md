### Выключение/перезагрузка

```bash
$ sudo shutdown now
$ sudo reboot || sudo shutdown -r now ||
```
___

### Очистка экрана

```bash
$ clear || ctrl + L
```
___

### Очистка в терминале

```bash
ctrl + U  # Очистка всей текущей строки (от курсора влево)
ctrl + W  # Удаление слова слева от курсора (ctrl + backspace не рабоает)
```
___

### Очистка в терминале

```bash
ctrl + Shift + Up|Down Arrow   # Scroll up|down one line at a time
```
___


### Выход из terminal, ssh, su

```bash
$ exit || ctrl + D
```
___

### Disk free and disk usage

Показать все файловые системы в `human-readable` единицах (`G, M`)

```bash
$ df -h
```

Использование места папкой/файлом

```bash
$ du -sh [FILE]
```
`-s, --summarize` - display only a total for each argument
___


ctrlZ fg bg $
alias для ssh
статичный ip, ip a
alias for ssh
users


`which` в Linux — это ==команда командной строки, которая находит и показывает полный путь к исполняемому файлу программы==


  alias ls='ls -l --color=auto'

command from history !534



In Linux, the `cd` command without any arguments will ==change the current working directory to the user's **home directory**==



### Основные команды для работы с файлами

pwd (Print Working Directory) - просмотр текущего местоположения

ls (List) - просмотр содержимого текущего каталога  

-a - отображать все файлы, включая скрытые, это те, перед именем которых стоит точка (a - all)

-l - выводить подробный список (l - list), аналогичная команда ll

h - выводить размеры папок в удобном для чтения формате (h - human-readable)

cd (Change Directory) - смена директории 

cd .. - подняться на уровень выше

cd ../.. -подняться на два уровня выше

cd - вернуть в домашнюю директорию

./myprog - запуск программы из текущей директории (используется без cd)

mkdir (Make Directory) - создание директории

mkdir new_folder - создание директории new_folder в текущей директории

mkdir /new_folder - создание директории new_folder в корне

mkdir -p won/der/ful - создать все отсутсвующие родительские директории (p - parent);

при вводе аналогичной команды без параметра -p возникнет ошибка: mkdir: cannot create directory `won/der/ful': No such file or directory

cp (Copy) - копирование объекта

cp app.log /home/user- копирование файла app.log из текущей директории в директорию /home/user.

cp app.log /home/user/app_new.log - копирование файла app.log из текущей директории в директорию /home/user c дальнейшим переименованием

!!! Если в директории, куда выполняется копирование, уже есть файл с аналогичным именем, то он будет перезаписан

cp -r /etc/sysconfig /home/user - копирование директории

rm (Remove) -удаление объекта

rm test.txt  — удалит файл test.txt в текущей директории

rm * - удаление всех файлы в текущей директории

rm -r test_folder - удаление папки (r -recursive)

rm -rf test_folder - удаление папки и ее содержимого без дополнительных подтверждений (f - force)

rm -rfv test_folder - удалит папку со всем содержимым, но выведет имена удаляемых файлов (v - verbose (подробное протоколирование))

touch - создание нового файла

touch /home/user/file - создание нового файла в домашней директории

Команда touch обновляет время последней модификации файла, если тот уже существует

Также можно создать пустой файл и сразу приступить к его редактированию с помощью текстового редактора: vi /home/user/readme

cat  (concatenate)- вывод содержимого файла в консоль

cat /home/user/readme - просмотр содержимого файла readme

mv (Move) - перемещение файла

mv app.log /home/user/ - перемещение файла app.log в директорию /home/user/

mv app.log /home/user/app_new.log - перемещение файла app.log в директорию /home/user/ c дальнейшим переименованием в app_new.log

mv app.log app_new.log - переименование файла app.log в app_new.log с сохранением содержимого файла

find - поиск файлов

find / -name messages - поиск в корневой паке файл с именем messages

man (Manual) - вывод справки по команде (man раздел название_команды)

man ls - вывод справке по команде ls

man -L ru man - вывод информации на русском языке о команде man

ls --help - также вывод справки

tail - просмотр последних строк файла

tail /var/log/messages - просмотр последних 10 строк файла (по умолчанию)

tail -50 /var/log/messages - просмотр последних 50 строк файла

head - просмотр первых строк файла

head /var/log/messages - просмотр первых 10 строк файла (по умолчанию)

head -50 /var/log/messages - просмотр первых 50 строк файла

grep - поиск/фильтрация строк, содержащих определенное значение (global regular expression print)

grep kdump.service /var/log/messages - поиск и вывод выражения kdump.service в файле /var/log/messages

tail -100 /var/log/messages | grep -n kdump.service - поиск выражения kdump.service в последних 100 строках файла  /var/log/messages.

Если выражение будет найдено, то перед строкой, содержащей выражение kdump.service, будет выведен номер этой сроки в файле.