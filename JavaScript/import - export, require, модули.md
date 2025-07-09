
В **ESM** можно экспортировать:  
✅ **Примитивы** (числа, строки, булевы значения, `null`, `undefined`, `Symbol`)  
✅ **Функции** (обычные, стрелочные)  
✅ **Объекты** (литералы, экземпляры классов)  
✅ **Массивы**  
✅ **Классы**  
✅ **Default export** (любую одну сущность)

Главное правило: **все экспорты должны быть объявлены** (кроме `default export`, который может быть анонимным).

❌ **Нельзя экспортировать необъявленные значения**

```js
export 123; // Ошибка! Нужно сначала объявить  
export "text"; // Ошибка!  
```

✅ **Правильно:**

```js
const value = 123;  
export { value };  
// или  
export const value = 123;  
```


#### Все варианты `export`:

- Перед объявлением класса/функции/…:
    - `export [default] class/function/variable ...`
- Отдельный экспорт:
    - `export {x [as y], ...}`.
- Реэкспорт:
    - `export {x [as y], ...} from "module"`
    - `export * from "module"` (не реэкспортирует `export default`).
    - `export {default [as y]} from "module"` (реэкспортирует только `export default`).


#### Импорт:

- Именованные экспорты из модуля:
    - `import {x [as y], ...} from "module"`
- Импорт по умолчанию:
    - `import x from "module"`
    - `import {default as x} from "module"`
- Всё сразу:
    - `import * as obj from "module"`
- Только подключить модуль (его код запустится), но не присваивать его переменной:
    - `import "module"`


>[!important] Фигурные скобки необходимы в случае именованных экспортов, для `export default` они не нужны. 

| Именованный экспорт       | Экспорт по умолчанию              |
| ------------------------- | --------------------------------- |
| `export class User {...}` | `export default class User {...}` |
| `import {User} from ...`  | `import User from ...`            |

> [!tip] Модуль выполняется полностью при первом импорте
>  Когда импортируется модуль **в первый раз**, JavaScript выполняет **весь его код сверху вниз**, включая выражения, такие как `console.log`, `if`, объявления функций и т. д.  
Экспорты (`export`) просто указывают, какие значения будут доступны другим модулям, но не останавливают выполнение остального кода.  
В приведенном ниже примере экспортируется только `user`, но `console.log` — это обычный JS-код, который выполняется при загрузке модуля.

```js
// module1.js
export const user = { name: 'Alex' }
console.log(user.name)

// module2.js
import { user } from './module1.js'
// Выведет 'Alex'
import { user } from './module1.js'
// Не выведет ничего
```

![[Модули, import_export — JavaScript — Дока.mhtml]]
![[Экспорт и импорт - Learn.JavaScript.ru.mhtml]]
![[Динамические импоры - Learn.JavaScript.ru.mhtml]]
## Модули в JavaScript

Изначально модулей в JavaScript не существовало. Считалось, что скрипты, подключаемые к страницам, очень простые, и модульная система не нужна. Время шло, приложения на JavaScript становились всё сложнее, и необходимость модулей становилась очевидной. Тогда появились первые попытки «принести» модули в JavaScript.

Сейчас модули уже появились и поддерживаются, но их «становление» проходило медленно. Существовало несколько версий модульных систем: `AMD, CommonJS, UMD и ES-модули`. Современной системой считаются ES-модули. Другие модульные системы считаются устаревшими.

### AMD

`AMD (asynchronous module definition)` — асинхронное определение модулей, одна из первых попыток создать систему модулей.

Её изначально реализовали в библиотеке Require.js. Вид и определение модулей были довольно многословными. Функция `define` из библиотеки require.js позволяла определять модули для дальнейшего использования.

Но, даже несмотря на такую многословность, сейчас в вебе достаточно много легаси, который использует AMD. Уж очень была заманчива идея наконец структурировать код.
___
### UMD

`UMD (Universal Module Definition`) предлагается как универсальная модульная система, совместима и с `AMD`, и с `CommonJS`.
___
### CommonJS

`CommonJS` — это стандарт модульной системы. Используется в серверном JavaScript (Node.js) и некоторых сборщиках

Любой `.js` файл может рассматриваться как модуль.

#### Экспорт значений

1. **Полный экспорт объекта**

```js
// module.js

const name = 'Alex';
const greet = () => 'Hello!';

module.exports = {
  name: name,
  greet: greet
};

// или просто name, greet (сокращённая запись ES6)
module.exports = { name, greet };
```

2. **Добавление свойств в экспорт (аналог именованных экспортов).**

```js
exports.name = 'Alex'; // добавляем в объект exports свойство 'name' со значением 'Alex'
exports.greet = () => 'Hello!';

// Или через предварительное объявление:
const name = 'Alex';
const greet = () => 'Hello!';
exports.name = name;
exports.greet = greet;
```

`exports` — это сокращение для `module.exports`. Изначально в Node.js:

```js
// Примерно так Node.js инициализирует модуль
var module = { exports: {} };
var exports = module.exports; // exports становится ссылкой на module.exports
```

`exports` нельзя использовать при попытке переопределить весь экспорт:

```js
// ❌ Не сработает (потеряется связь с module.exports)
exports = { a: 1, b: 2 };

// ✅ Работает правильно
module.exports = { a: 1, b: 2 };
```

3. **Экспорт одного значения (аналог default export)**

	Если модуль экспортирует только одну сущность, можно использовать экспорт одного значения (не требуется экспортировать объект). 
	В этом случае при импорте не требуется деструктуризация импортируемого объекта.

```js
const greet = () => 'Hello!';
module.exports = greet;
```

>[!danger] При такой записи перезаписываются все предыдующие экспорты
> Если бы ранее был экспорт `exports.name = 'Alex';` то он будет потеян

#### Импорт модулей

```js
const myModule = require('./module.js');
console.log(myModule.name); // 'Alex'
console.log(myModule.greet()); // 'Hello!'

// Или с деструктуризацией (похоже на ES Modules):
const { name, greet } = require('./module.js');
```

## Чем это отличается от ES Modules?

**В CommonJS**:
- Нет синтаксической разницы между "именованными" и "default" экспортами
- Всё экспортируется как свойства одного объекта (`module.exports`)
- При `require()` всегда получаем весь объект целиком

**В ES Modules**:
- Чёткое разделение: `export const name` vs `export default`
- Можно импортировать только нужные именованные экспорты
- Синтаксис более строгий и декларативный

___













## Пример исправления модуля

```js
const getData = async () => { 
	const response = await fetch('https://dummyjson.com/products/1'); 
	return response.json(); 
} 

export getData() // Ошбика SyntaxError
```

### Ошибки в коде:

Синтаксис `export`:
- `export` требует указания, что именно экспортируется (`export const`, `export function`, или `export default`).
- `export getData()` — невалидный синтаксис.

Экспорт промиса:
 - `getData()` возвращает промис, но вы пытаетесь экспортировать его результат напрямую без `await`.

### Варианты исправления

#### 1 .  Экспорт функции (рекомендуется)

```js
const getData = async () => {
  const response = await fetch('https://dummyjson.com/products/1');
  return response.json();
};

export { getData }; // Корректный экспорт функции
```

Импорт и использование:

```js
import { getData } from './module.mjs';

(async () => {
  const data = await getData(); // Ждём результат
  console.log(data);
})();
```


#### 2.  Экспорт результата вызова (с Top-Level Await)

```js
const getData = async () => {
  const response = await fetch('https://dummyjson.com/products/1');
  return response.json();
};

export const data = await getData(); // Ждём результат сразу
```

Импорт и использование:

```js
import { data } from './module.mjs'; // Уже готовые данные
console.log(data);
```


####  3. Экспорт по умолчанию

```js
export default async function getData() {
  const response = await fetch('https://dummyjson.com/products/1');
  return response.json();
}
```

Импорт и использование:

```js
import getData from './module.mjs';
const data = await getData();
```


####  4.  Улучшенный вариант

```js
const getData = async (url) => {
  const response = await fetch(url);
  return response.json();
};

export { getData }
```

Импорт и использование:

```js
import { getData } from './module.mjs';

(async () => {
  const data = await getData('https://api.example.com/users');
  console.log(data);
})();
```

Преимущества:
-  Гибкость. Принимает `url` как параметр → можно переиспользовать для разных API-эндпоинтов
-  Контроль над выполнением. 
	- Импортирующий код сам решает, когда вызывать функцию и обрабатывать `await`.
	- Нет блокировки загрузки модуля (в отличие от `export const data = await ...`)
- Обработка ошибок на стороне вызывающего кода.  Можно использовать `try/catch` там, где это логически уместно.

```js
try {
  const data = await getData(url);
} catch (err) {
  console.error('Ошибка загрузки:', err);
}
```

- Оптимизация производительности. Не загружает данные до фактического вызова функции