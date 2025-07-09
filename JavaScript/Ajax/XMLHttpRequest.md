
На сегодняшний день не обязательно использовать `XMLHttpRequest`, так как существует другой, более современный метод `fetch`.

В современной веб-разработке `XMLHttpRequest` используется по трём причинам:

1. По историческим причинам: существует много кода, использующего `XMLHttpRequest`, который нужно поддерживать.
2. Необходимость поддерживать старые браузеры и нежелание использовать полифилы (например, чтобы уменьшить количество кода).
3. Потребность в функциональности, которую `fetch` пока что не может предоставить, к примеру, отслеживание прогресса отправки на сервер.

Чтобы сделать запрос, нам нужно выполнить три шага:
### Создать `XMLHttpRequest`

```js
const xhr = new XMLHttpRequest();
```

### Инициализировать его

```js
xhr.open("GET", 'https://jsonplaceholder.typicode.com/users');
```

В  него передаются основные параметры запроса: 
- `method` – HTTP-метод. Обычно это `"GET"` или `"POST"`
- `URL` –  куда отправляется запрос: строка или объект URL.

Заметим, что вызов `open`, вопреки своему названию, не открывает соединение. Он лишь конфигурирует запрос, но непосредственно отсылается запрос только лишь после вызова `send`.

### Послать запрос

```js
xhr.send([body])
```

Этот метод устанавливает соединение и отсылает запрос к серверу. Необязательный параметр `body` содержит тело запроса. Некоторые типы запросов, такие как `GET`, не имеют тела. А некоторые, как, например, `POST`, используют `body`, чтобы отправлять данные на сервер

### Пролушивание события на `xhr`, чтобы получить ответ

Три наиболее используемых события:
- `load` – происходит, когда получен какой-либо ответ, включая ответы с HTTP-ошибкой, например 404.
- `error` – когда запрос не может быть выполнен, например, нет соединения или невалидный URL.
- `progress` – происходит периодически во время загрузки ответа, сообщает о прогрессе.

```js
xhr.addEventListener('load', () => {
	console.log(`Загружено: ${xhr.status} ${xhr.response}`);
});
```

```js
xhr.addEventListener('error', () => {
	console.log('Ошибка соединения');
});
```

```js
xhr.addEventListener('progress', () => { // запускается периодически
	// event.loaded - количество загруженных байт
	// event.lengthComputable = равно true, если сервер присылает заголовок Content-Length
	// event.total - количество байт всего (только если lengthComputable равно true)
	console.log(`Загружено ${event.loaded} из ${event.total}`);
};
```

Можно отслеживать состояние свойства `readyState` объекта `XMLHttpRequest`. 

```js
xhr.addEventListener('readystatechange', () => {
	if (xhr.readyState === 4 && xhr.status === 200) {
		// ...
	}
});
```

Объект XHR может иметь следующие состояния:
- `UNSENT = 0` - исходное состояние
- `OPENED = 1`- вызван метод open
- `HEADERS_RECEIVED = 2`- получены заголовки ответа
- `LOADING = 3`- ответ в процессе передачи (данные частично получены)
- `DONE = 4`- запрос завершён
- 
Состояния объекта `XMLHttpRequest` меняются в таком порядке: `0` → `1` → `2` → `3` → … → `3` → `4`. Состояние `3` повторяется каждый раз, когда получена часть данных.

Вы можете наткнуться на обработчики события `readystatechange` в очень старом коде, так уж сложилось исторически, когда-то не было событий `load` и других. Сегодня из-за существования событий `load/error/progress` можно сказать, что событие `readystatechange` «морально устарело».

---

### Пример GET-запроса

```js
const xhr = new XMLHttpRequest();
xhr.open("GET", 'https://jsonplaceholder.typicode.com/users');
xhr.send();

xhr.addEventListener('load', () => {
	if (xhr.status === 200) {
		const data = JSON.parse(xhr.response);
		console.log(data);
	} else {
		console.error(`Ошибка: ${xhr.status} ${xhr.statusText}`);
	}
});

xhr.addEventListener('error', () => {
	console.log('Ошибка соединения');
});
```


После ответа сервера мы можем получить результат запроса в следующих свойствах `xhr`:

- `status`
	Код состояния HTTP (число): `200`, `404`, `403` и так далее, может быть `0` в случае, если ошибка не связана с HTTP.

- `statusText`
	Сообщение о состоянии ответа HTTP (строка): обычно `OK` для `200`, `Not Found` для `404`, `Forbidden` для `403`, и так далее.

- `response`
	Тело ответа сервера.

___

### Тип ответа

Мы можем использовать свойство `xhr.responseType`, чтобы указать ожидаемый тип ответа:
- `""` (по умолчанию) – строка,
- `"text"` – строка,
- `"arraybuffer",
- `"blob"`,
- `"document"` – XML-документ (может использовать XPath и другие XML-методы),
- `"json"` – JSON (парсится автоматически - не нужно использовать `JSON.parse(xhr.response)`).

К примеру, давайте получим ответ в формате JSON:

```js
const xhr = new XMLHttpRequest();
xhr.open("GET", 'https://jsonplaceholder.typicode.com/users');
xhr.responseType = 'json';
xhr.send();

xhr.addEventListener('load', () => {
	console.log(xhr.response); // xhr.response не нужно парсить JSON.parse(xhr.response)
});
```
---

### Таймаут запроса

Мы можем указать таймаут – промежуток времени, который мы готовы ждать ответ.
Если запрос не успевает выполниться в установленное время, то он прерывается, и происходит событие `timeout`.

```js
xhr.timeout = 10000;
```
---

### Отмена запроса

Можно отменить запрос.  
При этом генерируется событие `abort`, а `xhr.status` устанавливается в `0`.

```js
xhr.abort()
```
---

### HTTP-заголовки

`XMLHttpRequest` умеет как указывать свои заголовки в запросе, так и читать присланные в ответ.

`setRequestHeader(name, value)` - устанавливает заголовок запроса с именем `name` и значением `value`.

```js
xhr.setRequestHeader('Content-type', 'application/json; charset=utf-8');
```

Заголовок `Content-type` нужно указывать при отправке данных на сервер.
___

### Пример POST-запроса

```js
const newPost = { "userId": 101, "id": 101, "title": "post title", "body": "post body" };

const xhr = new XMLHttpRequest();
xhr.open("POST", 'https://jsonplaceholder.typicode.com/posts');
xhr.setRequestHeader('Content-type', 'application/json; charset=utf-8');
xhr.send(JSON.stringify(newPost));

xhr.addEventListener('load', () => {
	console.log(xhr.status);
	console.log(JSON.parse(xhr.response));
});
```