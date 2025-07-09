- После установки активировать `Modules - MySQL`. После этого будет доступен PhpMyAdmin. По умолчанию - `user: root, password: (empty)`
-  Чтобы настроить свою папку с доменами нужно в `Server - Root domain directory` указать адрес папки с проектами, затем в `Domains` выбрать `Manual control` и добавить доменное имя и папку c `index.php`. Например `raspil-lux - \raspil-lux-site\web`
### Пример развертывания YII2 проекта с Github

1. Скачать проект с помощью `git clone https://github.com/...`
2. Создать БД с помощью PhpMyAdmin и импортировать в нее базу данных проекта
3. В корне проекта в файле `.env` указать данные для подключения к БД (PhpMyAdmin), которые используются в `/config/db.php`
```php
DB_HOST="localhost"
DB_NAME="raspil_lux"
DB_USER="root"
DB_PASS="root"
```
4. В консоли Open Server (`Advanced - Console`) перейти в папку с проектом и выполнить команду  `composer update`
