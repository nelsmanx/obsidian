В JavaScript есть два типа свойств объекта:
- свойства-данные (Data Properties)
- свойства-аксессоры (Accessor Properties)

**Свойства-данные** — это обычные свойства объекта, которые хранят значение.

```js
const obj = {
  name: 'Alice',
  age: 25        
};
```

**Свойства-аксессоры** не хранят значение напрямую, а используют **геттеры** и **сеттеры** для управления доступом.

```js
const user = {
  _email: 'alice@example.com', // защищенное поле
  
  get email() {
    return this._email.toLowerCase();
  },
  
  set email(value) {
    if (value.includes('@')) { // упрощенная проверка
      this._email = value;
    }
  }
};
```
 
 У каждого из свойств-данных объекта, кроме значения, есть ещё три флага конфигурации, которые могут принимать значения `true` или `false`. Эти флаги называются _дескрипторами_:

- `writable` — доступно ли свойство для записи;
- `enumerable` — является ли свойство видимым при перечислениях (например, в цикле `for..in`);
- `configurable` — доступно ли свойство для переконфигурирования.

Когда мы создаём свойство объекта «обычным способом», эти три флага устанавливаются в значение `true`.

Для изменения значений дескрипторов применяется статический метод `Object.defineProperty()`, а для чтения значений — `Object.getOwnPropertyDescriptors()`.

Другими словами, дескрипторы — это пары ключ-значение, которые описывают поведение свойства объекта при выполнении операций над ним (например, чтения или записи).

![[Pasted image 20250714133521.png]]

Создадим объект и добавим в него свойство `os` для ноутбука.  

```js
const laptop = {}

Object.defineProperty(laptop, 'os', {
  value: 'MacOS',
  writable: false,
  enumerable: true,
  configurable: true
})

```

Свойство `os` будет недоступно для перезаписи, но будет видно при перечислении и доступно для переконфигурирования. Попробуем перезаписать свойство `os` и выведем полученный результат:

```js
laptop.os = 'Windows'
console.log(laptop) // { 'os': 'MacOS' }
```
___

## `Object.defineProperty`

```js
Object.defineProperty(объект, имяСвойства, дескрипторы)
```

Функция принимает следующие параметры:

- `объект` — объект, свойство которого изменяем или добавляем;
- `имяСвойства` — свойство, для которого нужно применить дескриптор;
- `дескриптор` — дескриптор, описывающий поведение свойства.

Если свойство уже существует, `Object.defineProperty()` обновит флаги.  
Если свойство не существует, метод создаёт новое свойство с указанным значением и флагами. Если какой-либо флаг не указан явно, ему присваивается значение `false`.
___

## Дескрипторы

В JS каждое свойство объекта имеет **дескриптор**, который определяет его поведение. 
Дескриптор может быть:

- **Дескриптор данных (`Data Descriptor`)** определяет значение свойства и возможность изменить это значение.
- **Дескриптор доступа (`Accessor Descriptor`)** определяет работу свойства через функции чтения и записи свойства (геттера и сеттера)

> [!important] В обоих типах можно использовать общие свойства `configurable` и `enumerable`.


### Дескриптор данных

Дескриптор данных (`Data Descriptor`) определяет значение свойства и возможность изменить это значение. 
- `value` — значение свойства, по умолчанию `undefined`.
- `writable` — можно ли изменить значение с помощью оператора присваивания. По умолчанию устанавливается в `false` для свойств, созданных через `Object.defineProperty()` и в `true`, если свойство добавлено через оператор `'.'`.
- `configurable` и `enumerable` (общие свойства).

```js
const laptop = {}

Object.defineProperty(laptop, 'displaySize', {
  value: '15'
})

const descriptor = Object.getOwnPropertyDescriptor(laptop, 'displaySize')
console.log(descriptor)

// Вывод консоли
{
  "value": "15",
  "writable": false,
  "enumerable": false,
  "configurable": false
}
```

Мы не указали остальные свойства явно, поэтому дескриптор имеет  значения `false` для остальных свойств.  

Если свойство добавить через оператор `'.'` , то все свойства дескриптора устанавливаются в `true`. 
Добавим свойство, а затем исзменим свойсво дескриптора `writable` на `false`

```js
 const laptop = {};
laptop.displaySize = 15;

Object.defineProperty(laptop, 'displaySize', {
  writable: false
});

laptop.displaySize = 17;
console.log(laptop.displaySize) // 15 (значение не изменилось). 
// В строгом режиме мы получим ошибку `TypeError`, которая говорит о том, что мы не можем изменить неперезаписываемое свойство.
```


### Дескриптор доступа

Дескриптор доступа (`Accessor Descriptor`) определяет работу свойства через функции чтения и записи свойства (геттера и сеттера)
- `get` — функция, используемая для получения значения свойства, возвращает значение или `undefined`.
- `set` — функция, используемая для установки значения свойства. Принимает единственным аргументом новое значение, присваиваемое свойству.
- `configurable` и `enumerable` (общие свойства).

Подробнее о геттарах и сеттерах можно прочитать здесь [[Getters and setters]].


### Общие свойства

Общие свойства можно указывать в обоих типах дескрипторов как в дескрипторе данных, так и в дескрипторе доступа.

#### `enumerable`

Это свойство определяет, является ли создаваемое свойство объекта видимым при перечислениях.
Создадим два свойства у объекта `laptop` — одно будет перечисляемым, а другое — нет:

```js
const laptop = {}

Object.defineProperty(laptop, 'processor',
    { enumerable: true, value: 'Intel Core' } // Сделаем processor перечисляемым, как обычно
)

Object.defineProperty(laptop, 'touchID',
    { enumerable: false, value: true } // Сделаем `touchID` НЕперечисляемым
)

console.log(laptop.touchID) // true
console.log(('touchID' in laptop)) // true
console.log(laptop.hasOwnProperty('touchID')) // true

for (let key in laptop) {
  console.log(key, laptop[key])
}
// 'processor': 'Intel Core'
```

Заметьте, что `laptop.touchID` существует и имеет значение, но не отображается в цикле `for..in` (при этом, оно существует, если воспользоваться оператором `in`). «Перечислимое» означает: "будет учтено, если пройти перебором по свойствам объекта"».


#### `configurable`

Свойство `configurable` определяет, доступно ли создаваемое свойство объекта для переконфигурирования.

Изменим значение `configurable`:

```js
const laptop = {}

Object.defineProperty(laptop, 'processor', {
    value: 'Intel Core',
    writable: true,
    configurable: false, // Запрещаем переконфигурирование!
    enumerable: true
})

console.log(laptop.processor) // Intel Core

laptop.processor = 'M1'
console.log(laptop.processor) // 'M1'


Object.defineProperty(laptop, 'processor', {
    value: 'M1 TOP',
    writable: true,
    configurable: true,
    enumerable: true
})
// TypeError: Cannot redefine property: processor
```

Попытка переписать дескриптор свойства `processor` приводит к ошибке `TypeError`, даже если вы находитесь не в строгом режиме.

Еще пример - свойсво `Math.PI` – только для чтения, неперечислимое и неконфигурируемое:

```js
let descriptor = Object.getOwnPropertyDescriptor(Math, 'PI');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": 3.141592653589793,
  "writable": false,
  "enumerable": false,
  "configurable": false
}
*/
```

> [!warning] Будьте осторожны, изменение `configurable` на `false` необратимо и его нельзя отменить.

`configurable: false` означает:

- ❌ Нельзя удалять свойство (`delete laptop.processor` тоже вызовет ошибку в strict mode).
- ❌ Нельзя менять его дескриптор (кроме `value` и `writable`, если `writable: true`).  `configurable` нельзя изменить обратно на `true` 
- ❌ Нельзя превратить его из **data property** в **accessor property** (геттер/сеттер) и наоборот.
- ✅ Если `writable: true`, то можно менять `value` через присваивание (`laptop.processor = 'M1'`).
- ✅ Если `writable: true`, то можно менять `writable`  с `true` на `false`, но не обратно
___

## `Object.defineProperties()`

Позволяет определять множество свойств сразу. Необходимо для определения множества свойств одной операцией. Синтаксис: 

```js
Object.defineProperties(obj, {
  prop1: descriptor1,
  prop2: descriptor2
  // ...
});
```

Пример: 

```js
Object.defineProperties(user, {
  name: { value: "John", writable: false },
  surname: { value: "Smith", writable: false },
  // ...
});
```
___

## `Object.getOwnPropertyDescriptor()`

Получение значений дескрипторов для конкретного свойства объекта:

```js
const laptop = {
  processor: 'M1',
  os: 'MacOS'
};

const descriptor = Object.getOwnPropertyDescriptor(laptop, 'processor');
console.log(JSON.stringify(descriptor, null, 2))
/*
{
  "value": "M1",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```
___

## `Object.getOwnPropertyDescriptors()`

 Получение значений дескрипторов для всех свойств объекта:

```js
const descriptors = Object.getOwnPropertyDescriptors(laptop);
console.log(JSON.stringify(descriptors, null, 2))
/*
{
  "processor": {
    "value": "M1",
    "writable": true,
    "enumerable": true,
    "configurable": true
  },
  "os": {
    "value": "MacOS",
    "writable": true,
    "enumerable": true,
    "configurable": true
  }
}
*/
```
___

Периодически разработчику нужно защищать объекты от вмешательства извне. По ошибке легко изменить свойство объекта. Для защиты объектов от подобных изменений и управления их иммутабельностью предлагается использовать дескрипторы, такие как `writable` и `configurable`, сеттеры, а также методы `Object.preventExtensions()`, `Object.seal()`, и `Object.freeze()` для ограничения доступа к объекту целиком.

### `Object.preventExtensions()` и `Object.isExtensible()`

 `Object.preventExtensions()` запрещает добавление новых свойств объекта, но позволяет изменять значение, удаление, переконфигурирование существующих свойств.
 
 `Object.isExtensible()` возвращает `false`, если добавление свойств запрещено, иначе `true`.

```js
const laptop = {
  displaySize: 15
};

Object.preventExtensions(laptop);

laptop.displaySize = 17;
laptop.storage = 256;

console.log(laptop.displaySize); // 17
console.log(laptop.storage); // undefined

console.log(Object.isExtensible(laptop)) // false
```


### `Object.seal()` и `Object.isSealed()`

`Object.seal()` запечатывает переданный ему объект, одновременно запрещая добавление новых свойств и конфигурирование существующих свойств. Значения свойств при этом изменять можно. Другими словами, `Object.seal()` является эквивалентом применения `Object.preventExtensions()` к объекту и `configurable: false` к его свойствам.

`Object.isSealed()` возвращает `true`, если добавление/удаление свойств запрещено и для всех существующих свойств установлено `configurable: false`.


### `Object.freeze()` и `Object.isFrozen()`

`Object.freeze()` замораживает объект, одновременно запрещая добавление новых свойств и изменение существующих свойств. Это соответствует применению `Object.preventExtensions()` к объекту и `writable: false` к его свойствам.

Обратим внимание, что метод поверхностный. У замороженного объекта остаётся возможность изменять вложенные объекты. На MDN есть пример глубокой заморозки — метод `deepFreeze()`.  Он позволяет сделать полностью иммутабельный (неизменяемый) объект. При этом невозможно сделать иммутабельными `Date`, `Map` или `Set`.

### Разница между методами защиты объектов

| Метод                         | Добавление свойств | Удаление свойств | Изменение значений | Изменение дескрипторов (`configurable`)                    |
| ----------------------------- | ------------------ | ---------------- | ------------------ | ---------------------------------------------------------- |
| `Object .preventExtensions()` | ❌ Запрещено        | ✅ Разрешено      | ✅ Разрешено        | ✅ Разрешено (если `configurable: true`)                    |
| `Object.seal()`               | ❌ Запрещено        | ❌ Запрещено      | ✅ Разрешено        | ❌ Запрещено (все `configurable: false`)                    |
| `Object.freeze()`             | ❌ Запрещено        | ❌ Запрещено      | ❌ Запрещено        | ❌ Запрещено (все `configurable: false`, `writable: false`) |

Из трёх методов **`Object.freeze()`** используется чаще всего, затем **`Object.seal()`**, а **`Object.preventExtensions()`** — реже.  

`Object.freeze()` самый строгий и предсказуемый.  Используется для **констант**, **enum-подобных объектов**, **иммутабельных данных**. Пример:  

```js
 const AppConstants = Object.freeze({
   API_KEY: '12345',
   MAX_USERS: 100,
 });
 ```
  
`Object.seal()` для случаев, когда нужно **зафиксировать структуру**, но разрешить менять значения.  Используется реже, но полезен для **настроек**, **конфигов**, где важно сохранить поля.  Пример:  

```js
const config = { theme: 'dark', fontSize: 14 };
Object.seal(config);
config.theme = 'light'; // ✅ Можно
config.language = 'en'; // ❌ Нельзя добавить
```
   
`Object.preventExtensions()` самый редкий.  Позволяет удалять и изменять свойства, что делает его менее безопасным. Применяется в специфичных сценариях, например, для **защиты API-ответов** от расширения. Пример:  

```js
const user = { name: 'Alex' };
Object.preventExtensions(user);
user.name = 'Anna'; // ✅ Можно
delete user.name;   // ✅ Можно (если configurable: true)
     ```

### Вывод:
- Если нужно **полностью запретить изменения** — `freeze()`.  
- Если нужно **запретить добавление/удаление полей**, но разрешить изменение значений — `seal()`.  
- Если нужно **только запретить добавление новых полей** — `preventExtensions()`.  

Для большинства проектов достаточно `Object.freeze()`.