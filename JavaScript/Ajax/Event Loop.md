Связано с темой [[Архитектура браузера]]

**Event Loop** - это механизм, который позволяет однопоточному JavaScript выполнять асинхронные операции, обрабатывать события и обновлять интерфейс.

## Компоненты Event Loop

1. **Call Stack (Стек вызовов)**:
    - LIFO-структура (Last In, First Out)
    - Хранит выполняемые функции
    - При переполнении - ошибка "Maximum call stack size exceeded"

2. **Web APIs**:
    - API, предоставляемые браузером (не часть ядра JS)
    - Примеры: `setTimeout`, : `setInterval`,  AJAX (`fetch, XHR), DOM-события (event Listener)
    - Когда асинхронная операция завершается, callback помещается в очередь

3. **Callback Queues (Очереди callback'ов)**:
    - **Task Queue (Macrotask Queue)**: `setTimeout`, `setInterval`, события `DOM`, `fetch` (FIFO-структура)
    - **Microtask Queue**: `Promise.then/catch/finally`, `queueMicrotask` (функция для явного добавления микрозадач.) , `MutationObserver` (FIFO)
    - **Animation Frames**: `requestAnimationFrame` (призван оптимизировать рендеринг)

4. **Event Loop**:
    - Постоянно проверяет, пуст ли call stack
    - Если стек пуст, берет задачи из очередей


### Call Stack

Стек вызовов — это структура данных, которая, говоря упрощённо, записывает сведения о месте в программе, где мы находимся. Если мы переходим в функцию, мы помещаем запись о ней в верхнюю часть стека. Когда мы из функции возвращаемся, мы вытаскиваем из стека самый верхний элемент и оказываемся там, откуда вызывали эту функцию. Рассмотрим пример. 

```js
function multiply(x, y) {
    return x * y;
}
function printSquare(x) {
    const s = multiply(x, x);
    console.log(s);
}
printSquare(5);
```

Когда движок только начинает выполнять этот код, стек вызовов пуст. После этого происходит следующее:

![[Pasted image 20250707140337.png]]

### Web API

**Web API** — это **встроенные браузером (или Node.js)** функции, которые **не являются частью самого JavaScript**, но **доступны из него**.

Они выполняются **вне основного потока JS**, и позволяют JavaScript быть **асинхронным**, несмотря на то, что он однопоточен. 

Чтобы реализовать **асинхронность**, браузер предоставляет **внешние "службы" (Web APIs)**. Они делают "тяжёлую" работу (таймеры, HTTP-запросы, DOM-события и т.д.), а потом ставят результат обратно в **очередь задач**, которую обрабатывает **Event Loop**.

Самые частоиспользуемые Web API: 

### Таблица популярных Web API с примерами

| Web API                           | Назначение                         | Пример использования                                              |
| --------------------------------- | ---------------------------------- | ----------------------------------------------------------------- |
| `setTimeout`, `setInterval`       | Таймеры и периодическое выполнение | `setTimeout(() => console.log("Hi!"), 1000);`                     |
| `fetch`                           | HTTP-запросы                       | `fetch('/api').then(res => res.json())`                           |
| `DOM API`                         | Работа с HTML-элементами           | `document.querySelector('#btn').textContent = "Click!"`           |
| `addEventListener`                | Обработка событий                  | `button.addEventListener('click', () => alert("Clicked"));`       |
| `localStorage` / `sessionStorage` | Хранение данных в браузере         | `localStorage.setItem('key', 'value');`                           |
| `Promise` + Web APIs              | Асинхронная обработка              | `navigator.clipboard.readText().then(text => console.log(text));` |
| `console`                         | Логирование, отладка               | `console.table([{name: "Alice"}, {name: "Bob"}])`                 |
| `History API`                     | Управление историей браузера       | `history.pushState({}, '', '/new-url');`                          |
| `Navigator API`                   | Инфо об устройстве / буфер обмена  | `navigator.clipboard.writeText('Hello!');`                        |
| `Clipboard API`                   | Чтение/запись в буфер обмена       | `navigator.clipboard.readText().then(console.log);`               |
### Дополнительно:

| Web API                 | Назначение                                        | Пример                                                                                                              |
| ----------------------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `XMLHttpRequest`        | Старый способ делать HTTP-запросы                 | `const xhr = new XMLHttpRequest(); xhr.open("GET", "/"); xhr.onload = () => console.log(xhr.response); xhr.send();` |
| `IntersectionObserver`  | Отслеживание появления элемента в видимой области | `new IntersectionObserver(cb).observe(document.querySelector('#target'))`                                           |
| `MutationObserver`      | Отслеживание изменений DOM                        | `new MutationObserver(muts => console.log(muts)).observe(document.body, {childList: true})`                         |
| `File API`              | Работа с файлами пользователя                     | `input.addEventListener('change', e => console.log(e.target.files[0]))`                                             |
| `Canvas API`            | Рисование графики                                 | `ctx.fillRect(10, 10, 100, 100)`                                                                                    |
| `WebSocket`             | Двусторонняя связь с сервером                     | `new WebSocket("wss://example.com")`                                                                                |
| `Notification API`      | Показ уведомлений                                 | `Notification.requestPermission().then(() => new Notification("Hi!"));`                                             |
| `Geolocation API`       | Получение координат пользователя                  | `navigator.geolocation.getCurrentPosition(pos => console.log(pos.coords));`                                         |
| `requestAnimationFrame` | Анимации с высокой частотой кадров                | `requestAnimationFrame(() => console.log("frame"))`                                                                 |
### Микротаски:

```js
then(() => { ... })

catch(() => { ... })

finally(() => { ... })

async () => { 
	const response = await ... // await является микротаском
}

queueMicrotask(() => { ... })

const mutationObserver = new MutationObserver(mutations => console.log(mutations))
```

### Макротаски:

```js
setTimeout(() => { ... })

setInterval(() => { ... })

setInterval(() => { ... }) // node.js

document.addEventListener('ev', () => { ... })
```

`fetch` сам по себе не является ни микротаской. ни макротаской: он регистрирует сетевой запрос в Web API, а последующий вызов `then()` обрабатывается через **микротаски**

Коллбек `xhr.onload = () => console.log('XHR done')` в свою очередь попадает в очередь макротасок



![[Pasted image 20250704204159.png]]
## Последовательность выполнения в Event Loop:

1. **Синхронный код** (Call Stack)  
    → Выполняется **первым** и полностью.

2. **Микрозадачи (Microtasks)**  
    → Обрабатываются **сразу после синхронного кода**, до рендеринга и макрозадач.  
    → Действительно, **очередь микрозадач опустошается полностью** (если новые микрозадачи добавляются в процессе, они тоже выполняются).

3. **Рендеринг (Render Phase)**  
    → Если браузеру нужно перерисовать кадр (60 FPS).

4. **Макрозадачи (Macrotasks)**  
    → Обрабатываются **по одной за цикл** (например, один `setTimeout` за проход).


 >[!example] Render Queue
 >**Рендеринг** — это не отдельная очередь в Event Loop, а часть браузерного рендеринг-пайплайна (style recalculation → layout → paint → composite). Он **связан с Event Loop**, но не является его формальной частью.
 >
 >**Запускается по условиям**:
 >- После выполнения всех микрозадач.
 >- Перед обработкой следующей макрозадачи.
 >- Только если есть изменения в DOM/CSS или визуальные обновления.
 >- Часто запускается в контексте `requestAnimationFrame`.
 >
 >**Частота**: До 60 раз в секунду (каждые ~16.6 мс), если основной поток не перегружен.


![[Event loop example.mp4]]
**Event loop на конкретном примере**


![[Event loop micortask vs macrotask.gif]]
**Приоритезация микротасков над макротасками**


![[Infinite microtask queue.gif]]
**Бесконечная очередь микротасков (очередь никогда не дойдет до макротасков)**


## Пример макрозадач, создающих микрозадачи

```js
console.log('Начало скрипта')

setTimeout(() => {
  console.log('Макрозадача: setTimeout')

  // Микрозадача, созданная внутри макрозадачи
  Promise.resolve().then(() => {
    console.log('Микрозадача: обработка промиса внутри setTimeout')
  })

  Promise.resolve().then(() => {
    console.log('Микрозадача: обработка промиса внутри setTimeout 2')
  })
}, 0)

setTimeout(() => {
  console.log('Макрозадача: setTimeout 2')
}, 0)

Promise.resolve().then(() => {
  console.log('Микрозадача: обработка первого промиса')
})

console.log('Конец скрипта')

// Начало скрипта
// Конец скрипта
// Микрозадача: обработка первого промиса
// Макрозадача: setTimeout
// Микрозадача: обработка промиса внутри setTimeout
// Микрозадача: обработка промиса внутри setTimeout 2
// Макрозадача: setTimeout 2
```

Обратите внимание на ситуацию, когда микрозадача создаётся в макрозадаче. Пока макрозадача не выполнит свой колбэк, браузер не узнает о том, что внутри есть микрозадача. Если в списке задач есть макрозадачи, которые ожидают обработки, а в процессе выполнения одной из макрозадач появились микрозадачи, выполнение оставшихся макрозадач отложится. Это продлится до тех пор, пока все микрозадачи не завершатся, даже если они появились после макрозадач.


![[Lydia Hallie. JavaScript Visualized - Event Loop, Web APIs, (Micro)task Queue.mp4]]

![[sentiero. JavaScript - Event Loop ｜ Асинхронность, Web API, Очереди (макро⧸микро.mp4]]