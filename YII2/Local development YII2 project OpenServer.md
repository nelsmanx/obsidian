1.  Клонировать проект `git clone`
2.  Cоздать и импортировать базу данных через PhpMyAdmin
3.  Внести данные для подключения бд в `.env` или в `config/db.php`

```
DB_HOST="localhost"
DB_NAME="raspil_lux"
DB_USER="root"
DB_PASS="root"
```

4. В консоли локаного сервера перейти в корневую папку проекта (`:d`)  и запустить команду  для установки зависимостей `composer update`. 
5. Добавить проект в домены OpenServer.   `OpenServer - Settings - Domains:` `Domain managment - Manual Control, Domain name - Project name, Domain Folder - Project folder` 

