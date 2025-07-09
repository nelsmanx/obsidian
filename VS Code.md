#vscode

## Skip the current match

Once you've started the matching using `Ctrl+D`, you can press `Ctrl+K, Ctrl+D` to skip the current match. Alternatively, to undo the most recent selection and go back one step, use `Ctrl+U` or `Ctrl+Shift+Z`

### Regexp 

Для поиска текста между `<p>` и `-` нужно выражение `(?<=<p>).*?(?=–)`
```html
<p>Замер окна – процесс определения размеров оконной конструкции для изготовления нового окна или приобретения готового.</p>
```
___

### SFTP 

Дополнительный контекст

```
ecopro
	- vlg
	- rostov
	- local-dev
	- .vscode
```

```json
[
	{
		"name": "ecoproholding volgograd",
		"host": "ip",
		"protocol": "ftp",
		"port": 21,
		"context": "./volgograd/",
		"remotePath": "/www/ecoproholding.ru/",
		"username": "",
		"password": "",
		"uploadOnSave": false,
		"useTempFile": false,
		"openSsh": false
	},
	{
		"name": "ecoproholding rostov",
		"host": "ip",
		"protocol": "ftp",
		"port": 21,
		"context": "./rostov/",
		"remotePath": "/www/rostov.ecoproholding.ru/",
		"username": "",
		"password": "",
		"uploadOnSave": false,
		"useTempFile": false,
		"openSsh": false
	}
]
```
___
### SSH подключение 

Если название ssh-ключа отличается от стандартного, то нужно добавить в файл `config` в папке `%HOMEPATH%\.ssh`

```
Host balcon-msk
    HostName 185.22.233.221
    User root
    Port 20022
	IdentityFile ~/.ssh/work_rsa

Host balcon-vlg
    HostName 185.22.233.221
    User root
    Port 20022
	IdentityFile ~/.ssh/work_rsa
```
___
### Custom theme

1. `C:\Users\Maestro\.vscode\extensions\azemoh.one-monokai-0.5.0\themes`
2. `Ctrl+Shift+P >Inspect Editor Tokens and Scopes`
```json
[
  {
    "name":"String",
    "scope":[
      "string",
      "string.template"
    ],
    "settings":{
      "foreground":"#61AFEF"
    }
  },
  {
    "name":"Types",
    "scope":"storage.type",
    "settings":{
      "foreground":"#C678DD"
    }
  }
]
```

