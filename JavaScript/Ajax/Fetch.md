
```js 
const response = await fetch(url, [opitons])
```
- **`url`** – URL для отправки запроса.
- **`options`** – дополнительные параметры: метод, заголовки и так далее.

Без `options` это простой GET-запрос, скачивающий содержимое по адресу `url`.

В качестве аргумента `fetch` можно передать объект класса `Request`

```js
const request = new Request('https://api.github.com/users/username', {
	headers: {
		'Authorization': 'BEARER API-TOKEN'
	}
});

const response = await fetch(request)
```

В качестве результата возвращается `Promise` c объектом класса `Response`.

Промис завершается с ошибкой, если `fetch` не смог выполнить HTTP-запрос, например при ошибке сети или если нет такого сайта. HTTP-статусы 404 и 500 не являются ошибкой.

Мы можем увидеть HTTP-статус в свойствах ответа:
- **`status`** – код статуса HTTP-запроса, например 200.
- **`ok`** – логическое значение: будет `true`, если код HTTP-статуса в диапазоне 200-299.

`Response` предоставляет несколько методов, основанных на промисах, для доступа к телу ответа в различных форматах. Основные методы: 
- **`response.json()`** – декодирует ответ в формате JSON,
- **`response.text()`** – читает ответ и возвращает как обычный текст 

### Пример GET-запроса

```js
async function fetchRequest() {
	try {
		const response = await fetch('https://jsonplaceholder.typicode.com/users');
		if (response.ok) {
			const data = await response.json();
			console.log(data);
		} else {
			console.error(`HTTP error: status ${response.status}`); // статусы 401, 404, 500 
		}
	} catch (error) {
		console.error(`Fetch error: ${error.message}`); // ошибки сети, неверный url
	}
}

// или с помощью промисов

fetch('https://sonplaceholder.typicode.com/users')
	.then(response => {
		if (!response.ok) {
			throw new Error(response.status);
		}
		return response.json();
	})
	.then(data => console.log('Success:', data))
	.catch(error => console.error('Error:', error));

```
___

### POST-запрос

Для отправки `POST`-запроса или запроса с другим методом, нам необходимо использовать `fetch` параметры:

- **`method`** – HTTP метод, например `POST`,
- **`body`** – тело запроса, одно из списка:
    - строка (например, в формате JSON),
    - объект `FormData` для отправки данных как `form/multipart`,
    - `Blob`/`BufferSource` для отправки бинарных данных,

```js
const newPost = { "userId": 1, "title": "new title", "body": "new body" };

async function fetchRequest() {
	try {
		const response = await fetch('https://jsonplaceholder.typicode.com/users', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json; charset=UTF-8',
			},
			body: JSON.stringify(newPost)
		});

		if (!response.ok) {
			throw new Error(response.status);
		}

		const data = await response.json();
		console.log(data);
	} catch (error) {
		console.error('Error:', error.message);
	}
}
```
___

### Отмена запроса 

```js
const controller = new AbortController();

fetch('http://jsonplaceholder.typicode.com/posts', {
    signal: controller.signal,
})

controller.abort()

```
___

### Отправка формы

```js
const form = document.querySelector('form');
const formData = new FormData(form);
form.addEventListener('submit', (e) => {
	e.preventDefault();
	fetchRequest();
});


async function fetchRequest() {
	try {
		const response = await fetch('api/upload', {
			method: 'POST',
			headers: {
				'Content-Type': 'multipart/form-data' // возможно, не нужен
			},
			body: formData,
		});

		if (!response.ok) {
			throw new Error(response.status);
		}

		const data = await response.json();
		console.log('Success: ', data);

	} catch (error) {
		console.error('Error: ', error.message);
	}
}
```
___

### Отправка файла

В объект `FormData` также можно добавить файл из `<input type="file">`

```html
<form>
	<input type="file" name="file" accept="image/*" />
	<button>Загрузить</button>
</form>
```

```js
const formData = new FormData();
const fileInput = document.querySelector('input[type="file"]');

const file = fileInput.files[0];
formData.append('image', file, file.name)
```
