`async/await` — это синтаксический сахар над промисами, делающий асинхронный код более читаемым и позволяет писать асинхронный код так, как если бы он был синхронным.

`async`-функция : 
-  объявляется с ключевым словом `async`; 
-  всегда возвращает промис (даже если возвращается не промис, он будет обёрнут в промис).

```js
async function foo() {
  return 42; // Автоматически обёртывается в Promise.resolve(42)
}

// или 

const foo = async () => 42; 
```

`await`: 
-  используется только внутри `async`-функций или на верхнем уровне модуля вне `async`-функций;
-  приостанавливает выполнение функции до разрешения промиса;
-  возвращает результат выполненного промиса (или выбрасывает ошибку, если промис `rejected`).

```js
async function bar() {
  const result = await somePromise; // ждёт разрешения промиса
  console.log(result); // выполняется только после await
}
```

> Если забыть `await`, промис не будет разрезолвлен (может привести к неочевидным багам)
___ 

### Пример. Замена цепочки промисов на `async/await`

```js
// Цепочка с промисами:
fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));

// То же самое с async/await:
async function getData() {
  try {
    const response = await fetch(url);
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}
```

Для обработки ошибок используется `try/catch` (аналог `.catch()` у промисов). 
Любая ошибка из блока `try` будет обрабатываться в блоке `catch` (аналог `.catch()` под нексколькими `then()` у промисов).
Если ошибку не обработать, то последующий код не отработает из-за появления этой ошибки.

```js
async function fetchData() {
  try {
    const response = await fetch('sfsdfsdfsdf');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Ошибка:', error);
  }
  console.log('hi there') // отработает, потому что ошибка обработана
}

async function fetchData() {
	const data = await fetch('sfsdfsdfsdf');
	console.log('hi there'); // не отработает, потому что fetch завершился с ошибкой
}
```
___

### Альтернативный способ обарбоки ошибкок без `try/catch`

При такой конструкции у нас не будет  доступа к переменной `data`, если ее нужно будет обработать за пределами конструкции `try/catch`:

```js
async function errorTest() {
	try {
		const response = await fetch('https://exapmple-api/');
		const data = await response.json();
	} catch (err) {
		console.error(err);
	}
	
	console.log(data); // ошибка (data находится в блоке try)
}
```

Чтобы обработать переменную `data`, нужно продолжить работу с ней в блоке `try`. 
Для доступа к переменной `data` за пределами `try/catch`, нам нужно вынести ее за пределы конструкции `try/catch` и перезаписать в блоке `catch`: 

```js
async function errorTest() {
  let data = null;
  
  try {
    const response = await fetch('https://exapmple-api/');
    data = await response.json();

  } catch (err) {
    console.error(err);
  }

  console.log(data);
}
```

Можно использовать `.catch()` для обработки каждого отдельного промиса, не заворачивая все в конструкцию `try/catch`, и не создавая дополнительные переменные за предалами этого блока.

```js
async function errorTest() {
	const response = await fetch('https://exapmple-api/');
		.catch((e) => {
		  console.log(e);
		  return e;
		});
	// ...
}
```

Реальный пример: 

```js
async function fetchGitHubUser(username) {
    // 1. Делаем запрос к GitHub API
    const response = await fetch(`https://api.github.com/users/${username}`)
      .catch(e => {
        console.error('Ошибка сети или URL:', e);
        throw new Error('Не удалось выполнить запрос'); // Перебрасываем ошибку дальше
      });

    // 2. Проверяем статус ответа (на случай 404 или 500 ошибок)
    if (!response.ok) {
      throw new Error(`Пользователь ${username} не найден (HTTP ${response.status})`);
    }

    // 3. Парсим JSON с обработкой возможных ошибок формата
    const userData = await response.json()
      .catch(e => {
        console.error('Ошибка парсинга JSON:', e);
        return { error: 'Invalid JSON', details: e.message };
      });

    // 4. Возвращаем результат
    return userData;
}

// Использование
(async () => {
  const user = await fetchGitHubUser('octocat'); // Существующий пользователь
  console.log('Успех:', user);

  const invalidUser = await fetchGitHubUser('nonexistent-user-123'); // Несуществующий
  console.log('Ошибка:', invalidUser);
})();
```


>[!tip] Нужно избегать избыточного `await` там, где можно вернуть промис напрямую. 

```js
async function getStarWarsMovie(id) {
	const response = await fetch(`https://swapi.dev/api/films/${id}/`); 
	return response.json() 
}
```

Возвращаем промис, не обязательно писать `await`, потому что из асинхронной фукнции
всегда возвращается промис.


>[!warning] Не смешивайте синтаксис `async/await` и `Promise.then`
 Старайтесь применять один подход на проекте: так код легче читать и поддерживать.
  Но если нужно выполнить асинхронную операцию в глобальной области видимости, все придётся воспользоваться `then()`.
 
###  Параллельное выполнение асинхронных операций

 Используйте `await Promise.all([...])` для параллельного выполнения нескольких независимых асинхронных функций.

Используя `async/await`, мы делаем наш код последовательным: ожидаем выполнения одной асинхронной функции и лишь после запускаем другую. В примере ниже новости будут запрошены только после получения пользователя:

```js
async function getUser(){
  // Возвращает информацию о пользователе
}

async function getNews(){
  // Возвращает список новостей
}

const user = await getUser()
const news = await getNews()
```

Но, запустив `getNews` параллельно c `getUser`, мы в большинстве случаев получим результат быстрее. `Promise.all()` позволяет запустить запросы параллельно, при этом дожидаться результата мы можем как и раньше при помощи `await`:

```js
const [user, news] = await Promise.all([
  getUser(),
  getNews()
])
```
___

### Top-Level Await

Top-level await добавляет поддержку `await` на верхнем уровне модуля, то есть вне асинхронных функций, которые декларируются с помощью `async`. Это позволяет превратить модуль в некое подобие большой асинхронной функции. При импорте такого модуля асинхронный код, помеченный с помощью ключевого слова `await`, будет останавливать выполнение импортирующего модуля до того момента, пока все зависимости не будут зарезолвлены. 

Определение данных в дочернем модуле с использованием `await` позволяет родительскому модулю ожидать окончания загрузки асинхронных данных, при этом загрузка других дочерних модулей не блокируется.

Допустим, у нас есть модуль `Parent.mjs`, импортирующий данные из модулей `Child.mjs`:

```js
// Parent.mjs
import { data } from './Child.mjs'
console.log('Parent:', data)
```

Модуль `Child.mjs` экспортирует данные, полученные асинхронно (не промис). 
При запуске `Parent.mjs` будет ожидать завершения асинхронной операции.

```js
// Child.mjs

const promise =
  fetch('https://dummyjson.com/products/1')
  .then(res => res.json())

export const data = await promis


/* Выше был приведен теоретический пример для демонстрации верхнеуровнего await. 
   На практике лучше возвращать функцию с параметром url: */

const getData = async (url) => {
  const response = await fetch(url);
  return response.json();
};

export { getData }
```
___

Здесь приведен пример преобразования "асинхронного модуля" с помощью` top-level await`:

```js
// было
import { process } from "./some-module.mjs";
export default (async () => {  
	const dynamic = await import(computedModuleSpecifier);  
	const data = await fetch(url);  
	const output = process(dynamic.default, data); 
	return { output };
})();

// стало
import { process } from "./some-module.mjs";
const dynamic = import(computedModuleSpecifier);
const data = fetch(url);
export const output = process((await dynamic).default, await data);
```

Добавление в стандарт top-level await упростит работу с динамическими импортами и модулями, предоставляющими асинхронные ресурсы, например, соединение с базой данных.