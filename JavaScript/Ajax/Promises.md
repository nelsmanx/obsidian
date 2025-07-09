Промис - специальный объект JavaScript, который используется для написания и обработки асинхронного кода. 

Асинхронные функции возвращают объект `Promise` в качестве значения. Внутри промиса хранится результат вычисления после успешного завершения асинхронной операции или ошибку, если она не завершается успешно.

---

Синтаксис создания `Promise`:

```js
const promise = new Promise((resolve, reject) => {
	// исполнитель (executor)
});
```

Функция, переданная в конструкцию `new Promise`, называется _исполнитель_ (executor). Когда `Promise` создаётся, она запускается автоматически после создания промимса (`Promise` выполняется как синхронный код, его не нужно отдельно вызывать как функцию). Задача этой функции — выполнить асинхронную операцию и перевести состояние промиса в `fulfilled` или `rejected`.

Изменить состояние промиса можно, вызвав колбэки, переданные аргументами в функцию:
- `resolve(value)` — если работа завершилась успешно, с результатом `value`.
- `reject(error)` — если произошла ошибка, `error` – объект ошибки.
---

У объекта `promise`, возвращаемого конструктором `new Promise`, есть внутренние свойства:  
`state` и `result`.

`result` - это свойство может иметь следующие значения:
- `undefined` -  первоначально, когда промис находится в состоянии `pending`;
- `value` - при вызове `resolve(value)`;
- `error` - при вызове `reject(error)`.

Промис может находиться в одном из трёх состояний:
- `pending` —  ожидание (стартовое состояние, операция стартовала);
- `fulfilled` — выполнено успешно ( при вызове `resolve`)
- `rejected` — выполнено с ошибкой (при вызове `reject`).
Состояние промиса, который находится либо в состоянии  `fulfilled`, либо в состоянии `rejected`, называют `settled` (завершенным).

![[Pasted image 20240801003427.png]]

Свойства `state` и `result` – это внутренние свойства объекта `Promise` и мы не имеем к ним прямого доступа.

>Поменять состояние можно только один раз: перейти из `pending` либо в `fulfilled`, либо в `rejected`.
>Также заметим, что функция `resolve`/`reject` ожидает только один аргумент (или ни одного). Все дополнительные аргументы будут проигнорированы.

![[Pasted image 20240722140309.png]]

```js
const promise = new Promise((resolve, reject) => {
	setTimeout(() => resolve('done'), 1000); 
	// перевод промиса в состояние fulfilled. Результатом выполнения будет строка'done'
});
```

![[Pasted image 20240722141136.png]]

```js
const promise = new Promise((resolve, reject) => {
	setTimeout(() => reject(new Error("Whoops!")), 1000);
	// перевод в состояние rejected. Результатом выполнения будет объект Error
});
```

![[Pasted image 20240722141256.png]]

>[!tip] `Promise.resolve()` и `Promise.reject()` всегда возвращают промисы.

___

### Методы: then, catch, finally

Объект `Promise` служит связующим звеном между исполнителем (`executor`) и функциями-потребителями (`consuming function`), которые получат либо результат, либо ошибку. Функции-потребители могут быть зарегистрированы с помощью методов `.then` и `.catch`.
___
#### then

Метод `then()` используют, чтобы выполнить код после успешного выполнения асинхронной операции.

Метод `then()` принимает в качестве аргумента две функции-колбэка. Если промис в состоянии `fulfilled` то выполнится первая функция. Если в состоянии `rejected` — вторая. 

```js
const promise = new Promise((resolve, reject) => {
	setTimeout(() => resolve("done!"), 1000);
});

// resolve запустит первую функцию, переданную в .then
promise.then(
	result => alert(result), // выведет "done!" через одну секунду
	error => alert(error) // не будет запущена
);
```

Хорошей практикой считается не использовать второй аргумент метода `then()` и обрабатывать ошибки при помощи метода `catch()`.

```js
const promise = new Promise((resolve, reject) => {
	setTimeout(() => reject(new Error("Whoops!")), 1000);
});

// reject запустит функцию, переданную в .catch
promise
	.then(result => alert(result))
	.catch(error => alert(error));
```

___
#### catch

Метод `catch()` используют, чтобы выполнить код в случае ошибки при выполнении асинхронной операции.

Метод `catch()` принимает в качестве аргумента функцию-колбэк, которая выполняется сразу после того, как промис поменял состояние на `rejected`.

 По сути,  `catch(errorHandlingFunction)` - это то же самое, что если использовать в методе `then` `null` в качестве первого аргумента: `then(null, errorHandlingFunction)`.
___
#### finally

Метод `finally()` используют для выполнения кода при завершении промиса. Код выполнится  в любом случае: как при переходе промиса  в состояние `fulfilled`, так и в `rejected`.

Колбэк у `finally()` не содержит параметров. Это следствие того, что колбэк будет вызван как при успехе, так и при ошибке.

`finally()` отлично работает в случаях, когда нужно убрать лоадер со страницы или кнопки. Он сработает вне зависимости от результата промиса, поэтому можно избежать дублирования кода.

```js
let isLoading = true;

// Вместо использования флага, чтобы показать что форма отправляется:
sendForm()
	.then((res) => {
		isLoading = false;
		alert('ok');
	})
	.catch((err) => {
		isLoading = false;
		alert(`Ошибка: ${err.message}`);
	});

// можно написать:
sendForm()
	.then((res) => {
	    alert('ok')
	})
	.catch((err) => {
	    alert(`Ошибка: ${err.message}`)
	})
	.finally(() => {
	    isLoading = false
	})
```

Метод `finally` «пропускает» результат или ошибку дальше, к последующим обработчикам.

 Здесь результат проходит через `finally` к `then`:

```js
new Promise((resolve, reject) => {
	setTimeout(() => resolve("value"), 2000);
})
	.finally(() => alert("Промис завершён")) // срабатывает первым
	.then(result => alert(result)); // <-- .then показывает "value"
```

А здесь ошибка из промиса проходит через `finally` к `catch`:

```js
new Promise((resolve, reject) => {
	throw new Error("error");
})
	.finally(() => alert("Промис завершён")) // срабатывает первым
	.catch(err => alert(err));  // <-- .catch показывает ошибку
```

> Обработчик `finally` также не должен ничего возвращать. Если это так, то возвращаемое значение молча игнорируется.
___

### Цепочка промисов

Цепочка промисов используется для выполнения последовательности асинхронных задач, которые должны быть выполнены одна за другой. 

```js
new Promise((resolve, reject) => {
	setTimeout(() => resolve(1), 1000);
})
	.then(result => {
		console.log(result); // 1
		return result * 2;
	})
	.then(result => {
		console.log(result); // 2
		return result * 2;
	})
	.then(result => console.log(result)) // 4
```

Идея состоит в том, что результат первого промиса передаётся по цепочке обработчиков `.then`. Всё это работает, потому что вызов `promise.then` возвращает промис, так что мы можем вызвать на нём следующий `.then`.

Но сама по себе цепочка промисов не обеспечивает выполнение асинхронного кода последовательно.

```js
new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(console.log('1 console.log'));
	}, 1000);
})
	.then(() => {
		setTimeout(() => {
			console.log('2 console.log');
		}, 2000);
	})
	.then(() => {
		console.log('3 console.log');
	});

// 1 console.log
// 3 console.log
// ... 2 seconds later
// 2 console.log
```

Для того, чтобы этот код был выполен последовательно нужно чтобы обработчик `handler`, переданный в `.then(handler)`, возвращал промис. В этом случае дальнейшие обработчики ожидают, пока он выполнится, и затем получают его результат.

```js
new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve(console.log('first console.log'));
	}, 1000);
}).then(() => {
	return new Promise(resolve => {
		setTimeout(() => {
			resolve(console.log('second console.log'));
		}, 2000);
	});
}).then(() => {
	console.log('third console.log');
});
```

Разберем измененный код пошагово:

- Создается новый промис с функцией-исполнителем, которая запускает `setTimeout`.
- Через 2 секунды выполняется колбэк `setTimeout`: 
	a. Вызывается `resolve(console.log('1 console.log'))`.
    - `console.log('1 console.log')` выполняется и выводит сообщение.
    - `console.log()` возвращает `undefined`, которое передается в `resolve()`.
    b. Сразу после `resolve()` выполняется следующая строка: 
	 `console.log('console.log after resolving promise')`
- После этого промис переходит в состояние `fulfilled`.
- Только после завершения текущего стека вызовов (который включает весь колбэк `setTimeout`) будет вызван обработчик `.then()`.

Третий `console.log` выполняется последним, потому что он находится в обработчике `.then()`, который будет вызван только после того, как текущий стек вызовов (включая весь колбэк `setTimeout`) завершится и управление вернется в цикл событий.


Еще один пример цепочки промисов с использованием `fetch`, который всегда возвращает `Promise`:

```js
fetch(`https://swapi.dev/api/films/${id}/`)
  .then(response => {
    // этот then сработает, когда разрешится промис с запросом данных о фильме
    return response.json() // нужно распарсить ответ сервера, это асинхронная операция
  })
  .then(movie => {
    // этот then сработает, когда данные о фильме распарсятся
    const characterUrl = movie.characters[0]
    return fetch(characterUrl) // вызов fetch вернет промис, возвращаем его из колбэка, чтобы продолжить цепочку
  })
  .then(response => {
    // этот then сработает, когда разрешится промис с результатами запроса персонажа
    return response.json()
  })
  .then(character => {
    renderCharacterProfile(character)
  })
  .catch(err => {
    // catch сработает, когда любая из операций выше завершится ошибкой
    renderErrorMessage(err)
  })
```


> `.then(handler)` всегда возвращает промис (любая возвращаемая сущность оборачивается в промис).  Если из функции обработчика `.then(handler)` не возвращается промис, то создается новый промис и немедленно переходит в состояние "fulfilled" (выполнено) с возвращаемым значением в качестве результата или `undefined`, если не было ничего возвращено.

```js
Promise.resolve(10)
    .then(x => x * 2) // возвращает 20 (т.к => в одну строку) → новый промис fulfilled(20)
    .then(y => console.log(y)); // 20

Promise.resolve(10)
    .then(x => {
        console.log(x); // 10, но нет return!
    }) // новый промис fulfilled(undefined)
    .then(y => console.log(y)); // undefined
```

![[Pasted image 20240801003138.png]]

> [!danger] Внимание!
> 
> В приведенном ниже примере, вызов `resolve()` не прерывает выполнение текущей функции. Он просто меняет состояние промиса на `fulfilled`, но код продолжает выполняться дальше. Только после выполнения кода выполняется `then`

```js
new Promise(resolve => {
    setTimeout(() => {
        resolve(console.log('1 console.log'));
        console.log('console.log after resolving promise'); // !!!
    }, 2000);
}).then(() => {
    console.log('2 console.log');
});

// 1 console.log
// console.log after resolving promise
// 2 console.log
```
___

### Обработка ошибок в цепочке промисов

Цепочки `then()` при обработке промисов могут быть очень большими. Для обработки ошибок могут использоваться несколько `catch()`. 

`catch()` обрабатывает ошибки от всех `then()` между ним и предыдущим `catch()`

![[2-216w.webp]]

В примере выше наш `catch()` — последний, а предыдущего нет, поэтому он будет обрабатывать все ошибки.

![[3-216w.webp]]

Если в цепочке несколько `catch()`, то каждый ловит ошибки от `then()`, находящихся выше.

![[4-216w.webp]]

Возможен вариант, когда финального `catch()` нет. Тогда ошибки от последних `then()` не будут обрабатываться.

⚠️ Такой код — плохой. Если в одном из последних `then()` произойдёт ошибка, то вся дальнейшая цепочка не отработает, при этом, из-за асинхронной природы промиса, прочий код вне промиса продолжит работать и приложение не упадёт.
___

### Методы промисов

- `Promise.all(iterable)`
- `Promise.allSettled(iterable)`
- `Promise.race(iterable)`
- `Promise.any(iterable)`
- `Promise.reject(reason)`
- `Promise.resolve(value)`


#### `Promise.all()`

Метод `Promise.all()` используют, когда нужно запустить несколько промисов параллельно и дождаться их выполнения.

`Promise.all()` принимает итерируемую коллекцию промисов (чаще всего — массив) и возвращает новый промис, который будет ==выполнен==, когда будут выполнены все промисы, переданные в виде перечисляемого аргумента, или ==отклонён==, если хотя бы один из переданных промисов завершится с ошибкой.

Метод `Promise.all()` возвращает массив значений всех переданных промисов, при этом сохраняя порядок оригинального (переданного) массива, но не порядок выполнения.

```js
const promise1 = new Promise(resolve => setTimeout(() => resolve(1), 5000)) 
const promise2 = new Promise(resolve => setTimeout(() => resolve(2), 2000)) 
const promise3 = new Promise(resolve => setTimeout(() => resolve(3), 1000))

Promise.all([promise1, promise2, promise3])
  .then((responses) => {
	console.log(responses); // [1, 2, 3]
  })

// c деструктуризацией массива
Promise.all([promise1, promise2, promise3])
  .then(([response1, response2, response3]) => { 
	console.log(response1); // 1 
	console.log(response2); // 2
	console.log(response3); // 3
  })
```

Если хотя бы один промис из переданного массива завершится с ошибкой, то `Promise.all()` тоже завершится с этой ошибкой. Метод уже не будет следить за выполнением оставшихся промисов, которые рано или поздно все-таки выполнятся, и их результаты будут просто проигнорированы.

```js
const promise1 = new Promise(resolve => setTimeout(() => resolve(1), 5000));
const promise2 = new Promise((resolve, reject) => setTimeout(() => reject('error'), 2000));
const promise3 = new Promise(resolve => setTimeout(() => resolve(3), 1000));

Promise.all([promise1, promise2, promise3])
  .then(responses => console.log(responses))
  .catch(error => console.error(error)); // error
```

В итоге обработчик `then()`будет проигнорирован, и будет выполняться код из обработчика ошибок `catch()`.

Довольно частое использование — преобразование массива с данными в массив с промисами с помощью `map()`. В `map()` для каждого элемента создаётся промис, а полученный массив передаётся в `Promise.all()`. Это позволит дождаться выполнения всех промисов, а затем обработать результат.

Например, можно использовать метод `Promise.all()` для получения данных нескольких пользователей через запрос к API:

```js
const userIds = [1, 2, 5];
const arrayFetchUsers = userIds.map(
  userId => fetch(`https://jsonplaceholder.typicode.com/users/${userId}`)
	.then(response => response.json()) // возвращается Promise
);


Promise.all(arrayFetchUsers)
  .then(responses => {
	responses.forEach(resp => {
	  console.log(resp.name);
	});
  })
  .catch(error => console.error(error));
```

Пример сначала сделает три запроса к API, с помощью `Promise.all()` дождётся их завершения и парсинга ответа в JSON, а затем выведет имя пользователя для каждого. В консоли появится:

```
Leanne Graham
Ervin Howell
Chelsey Dietrich
```


#### `Promise.allSettled()`

`Promise.allSettled()` используют, когда нужно запустить несколько промисов параллельно и дождаться их выполнения.

`Promise.allSettled()` очень похож на метод `Promise.all()`, но работает по-другому. В отличие от `Promise.all()`, `Promise.allSettled()` ждёт выполнения всех промисов, при этом неважно, завершились они успешно или с ошибкой.

`Promise.allSettled()` принимает итерируемую коллекцию промисов, чаще всего — массив. После метод возвращает новый промис, который выполнится, когда выполнятся все переданные промисы. Полученный промис содержит массив результатов выполнения всех переданных промисов, сохраняя порядок оригинального массива, но не порядок выполнения.

```js
const promises = [
  new Promise(resolve => setTimeout(() => resolve(1), 3000)),
  new Promise((resolve, reject) => setTimeout(() => reject('error'), 2000)),
  new Promise(resolve => setTimeout(() => resolve(3), 1000))
]

Promise.allSettled(promises)
	.then(responses => console.log(responses))


/* Вывод в консоль
[
	{status: 'fulfilled', value: 1},
	{status: 'rejected', reason: 'error'},
	{status: 'fulfilled', value: 3},
]
*/
```

Если промис выполнился успешно, на выходе получаем объект с двумя свойствами `status` и `value`. `status` содержит строку `'fulfilled'`, а `value` — значение, которое передали при вызове `resolve` у промиса:

```js
{ status: 'fulfilled', value: значение }
```

Если промис выполнился с отказом, на выходе получаем объект с двумя свойствами — `status` и `reason`. `status` содержит строку `'rejected'`, а `reason` — значение, которое передали при вызове `reject` у промиса:

```js
{ status: 'rejected', reason: значение }
```

Метод применяется для запросов к API. Он особенно удобен, когда запросы независимы и ошибка в одном не влияет на другие, так как `Promise.allSettled()` дождётся завершения всех запросов. Если же запросы зависимы, то лучше использовать метод `Promise.all()`.


#### `Promise.any()`

Метод `Promise.any()` используют, когда нужно запустить несколько промисов параллельно и дождаться ==первого успешного разрешённого==.

`Promise.any()` принимает итерируемую коллекцию промисов, чаще всего — массив. Метод возвращает новый промис, который выполнится, когда выполнится первый из промисов, переданных в виде перечисляемого аргумента. Промис может быть отклонён, если все из переданных промисов завершатся с ошибкой.

Возвращает значение первого успешно выполнившегося промиса. После того как один из промисов будет исполнен, метод не будет дожидаться исполнения остальных.

В отличие от `Promise.all()`, который содержит массив значений исполненных промисов, `Promise.any()` содержит только одно значение при условии, что хотя бы один из промисов исполнен успешно. Такой подход полезен, когда нужно, чтобы выполнился только один промис, неважно какой.

Также `Promise.any()` отличается от `Promise.race()`. Метод `Promise.race()` возвращает промис, содержащий значение первого завершённого (`resolved` или `rejected`). Метод `Promise.any()` возвращает промис, содержащий значение первого успешно выполненного (`resolved`) промиса. `Promise.any()` проигнорирует исполнение промисов с ошибкой (`rejection`) до первого исполненного успешного (`resolved`). 

#### Успешное выполнение нескольких промисов

Создадим два промиса, второй выполнится раньше:

```js
const promise1 = new Promise(resolve => setTimeout(() => resolve(1), 5000))
const promise2 = new Promise(resolve => setTimeout(() => resolve(2), 1000))

Promise.any([promise1, promise2])
  .then(result => console.log(result)) // 2

```

#### Один из промисов завершается ошибкой

Метод `Promise.any()` не завершится с ошибкой, если хотя бы один из промисов выполнится успешно.
В итоге обработчик `catch()` проигнорируется и выполнится код из обработчика `then()` со значением `1` в переменной `result`.

```js
const promise1 = new Promise(resolve => setTimeout(() => resolve(1), 5000))
const promise2 = new Promise(
	(resolve, reject) => setTimeout(() => reject('error'), 1000)
)

Promise.any([promise1, promise2])
  .then(result => console.log(result)) // 1
  .catch(error => console.error(error))
```

#### Все промисы завершаются ошибкой

Метод `Promise.any()` завершится с ошибкой, если все переданные промисы завершатся с ошибкой.
В итоге обработчик `then()` проигнорируется и выполнится код из обработчика ошибок `catch()`. В этом случае `error` это экземпляр класса `AggregateError`. Объект `AggregateError` содержит ошибки от обоих промисов в массиве `errors`.  
Порядок в массиве ошибок определяется очерёдностью промисов в исходной коллекции.

```js
const promise1 = new Promise(
	(resolve, reject) => setTimeout(() => reject('error1'), 1000)
)
const promise2 = new Promise(
	(resolve, reject) => setTimeout(() => reject('error2'), 2000)
)

Promise.any([promise1, promise2])
  .then(result => console.log(result))
  .catch(error => {
	  console.error(error); // AggregateError: All promises were rejected
	  console.log(error.errors) // ['error1', 'error2']
  })
```


#### `Promise.race()`

Метод `Promise.race()` используют, чтобы запустить несколько промисов и получить результат того, ==который выполнится быстрее==.

`Promise.race()` принимает итерируемую коллекцию промисов, чаще всего — массив. Метод возвращает новый промис, который завершится, когда будет получен ==первый результат или ошибка== от переданных промисов.  
Если все промисы завершатся без ошибок, то вернется тот, который будет выполнен первым.
Если все промисы завершатся с ошибками, то все равно вернется тот, который будет выполнен с ошибкой первым.
То же самое будет, если часть промисов завершится с ошибкой, а другая без - все равно вернется тот, который будет выполнен первым.

Результаты или ошибки остальных промисов будут проигнорированы.

```js
const promise1 = new Promise(resolve => setTimeout(() => resolve(1), 1000))
const promise2 = new Promise(
	(resolve, reject) => setTimeout(() => reject('error2'), 2000)
)

Promise.any([promise1, promise2])
  .then(result => console.log(result)) // 1
  .catch(error => console.error(error));
```

#### Отличие от `Promise.any()`

Как мы уже знаем, `Promise.race()` вернёт промис, который завершится, когда получен первый (самый быстрый) результат или _ошибка_ из всех переданных промисов.

`Promise.any()` вернёт промис, который завершится, когда получен первый (самый быстрый) результат (_без ошибки_) из всех переданных промисов.


#### `Promise.resolve(), Promise.reject()`

Методы `Promise.rеsolve()` и `Promise.reject()` чаще всего используют в разработке, для имитации  API- запросов, когда бэкенд ещё не готов, но нужно протестировать фронтенд.

```js
const mockApi = () => Promise.resolve({ id: 1, name: "Test User" });
const mockApi2 = () => Promise.reject(new Error("User not found"));

mockApi().then(console.log)
mockApi2().catch(console.log)
```

___

### Создание асинхронной функции с промисом

```js
function earnAllMoney() {
  return new Promise(function (resolve, reject) {
    const result = tryEarnAllMoney() // асинхронная операция
    if (result.ok) {
      resolve(result) // успех → переводим промис в fulfilled и передаём результат
    } else {
      reject(new Error(result)) // ошибка → переводим промис в rejected
    }
  })
}

earnAllMoney()
  .then((result) => {
    console.log("Успех! Деньги заработаны:", result);
  })
  .catch((error) => {
    console.error("Ошибка:", error.message);
  });

// или
async function handleMoney() {
  try {
    const result = await earnAllMoney();
    console.log("Успех! Деньги заработаны:", result);
  } catch (error) {
    console.error("Ошибка:", error.message);
  }
}

handleMoney();
```
___

### Схлопывание промисов

Промисы в JavaScript обладают свойством **"схлопывания" (flattening)** — если один промис возвращает другой промис, они автоматически объединяются в один. Это предотвращает создание "промисов внутри промисов" (вложенных промисов), упрощая обработку асинхронных операций.

#### Пример схлопывания промисов

```js
const promise = Promise.resolve(Promise.resolve(Promise.resolve("🐶")));
// Promise {<fulfilled>: '🐶'}

promise.then(console.log); // Выведет: 🐶
```

#### Что здесь происходит?

1. `Promise.resolve("🐶")` создаёт промис, который сразу выполнен (`fulfilled`) со значением `"🐶"`.
2. Внешний `Promise.resolve()` принимает этот промис и **автоматически "разворачивает" (схлопывает)** его, возвращая не `Promise<Promise<"🐶">>`, а просто `Promise<"🐶">`.
3. В итоге получается **один плоский промис**, который можно обработать через `.then()`.

#### Где это встречается?

- Цепочки промисов (`then`-цепочки)
	Если в `.then()` возвращается новый промис, он автоматически схлопывается:

```js
fetch("/api/data")
  .then((response) => response.json()) // response.json() возвращает промис
  .then((data) => console.log(data)); // data уже развернута
```

- `async/await`
	`await` автоматически разворачивает промисы:

```js
async function fetchData() {
  const data = await fetch("/api/data").then(res => res.json());
  // await "схлопывает" промис от res.json()
  console.log(data); // готовый объект, а не промис
}
```

- Ручное создание промисов
	Когда вручную оборачиваетcz значение в `Promise.resolve()`:

```js
const p = Promise.resolve(42); // Промис с числом
const p2 = Promise.resolve(p); // Схлопнется в промис с 42
p2.then(console.log); // 42
```

- `Promise.all`, `Promise.race`  
	Эти методы также автоматически разворачивают вложенные промисы:

```js
Promise.all([
  Promise.resolve(1),
  Promise.resolve(Promise.resolve(2)), // Схлопнется в 2
]).then(console.log); // [1, 2]
```

#### Зачем это нужно?

- **Упрощает код.** Не нужно вручную разворачивать вложенные промисы.
- **Избегает "адских" цепочек.** Без схлопывания пришлось бы писать `.then().then().then()` для каждого уровня вложенности.
- **Совместимость с `async/await`.** `await` работает только с "плоскими" промисами.
___


# Задачи

Напиши функцию `loadImage(url)`, которая загружает изображение по указанному URL и возвращает промис. Промис должен:
- `resolve` с элементом <img> (установи src в переданный URL), если изображение загружается успешно,
- `reject` с ошибкой new Error('Failed to load image'), если изображение не загружается.

```js
function loadImage(url) {
	return new Promise((resolve, reject) => {
	  const img = new Image();
	  img.src = url;
	  img.addEventListener('load', () => resolve(img))
	  img.addEventListener('error', err => reject) // Point-Free Style
	});
}

loadImage('https://placehold.co/800x400')
	.then(img => document.body.appendChild(img))
	.catch(err => console.error;
```


Напиши функцию `firstSuccessfulPromise(promises)`, которая принимает массив промисов и возвращает промис, который:
- **resolve** с первым успешно выполненным промисом (если хотя бы один успешен),
- **reject** с массивом ошибок, если все промисы завершились с ошибкой.

```js
function firstSuccessfulPromise(promises) {
  return Promise.any(promises)
    // .then(promise => promise) // избыточно, итак возвратится первый успешный
    .catch(err => err.errors);
}

const promises = [
  Promise.reject(new Error('Failure 1')),
  Promise.resolve('Success 1'),
  Promise.reject(new Error('Failure 2')),
];

firstSuccessfulPromise(promises)
  .then(result => console.log(result)) // 'Success 1'
  .catch(errors => console.error(errors));
```
