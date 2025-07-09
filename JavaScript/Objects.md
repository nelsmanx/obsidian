#js 

Пустой объект («пустой ящик») можно создать, используя один из двух вариантов синтаксиса:
```js
const person = {};  // синтаксис "литерал объекта"
const person = new Object(); // конструктор Object()
const person = Object.assign({}); 

// функция-конструктор
function Person(firstName, lastName) {         
  this.firstName = firstName;
  this.lastName = lastName;
}

// класс ES6
class Person {
  constructor(firstName, lastName) {          
    this.firstName = firstName;
	this.lastName = lastName;
  }
}

// ключевое слово "New". Одинаковая запись для создания объекта через класс и функцию-конструктор.
const person = new Person('Иван', 'Петров'); 

```

Объект, объявленный через  `const` , может быть изменён.  Объявление  `const`  защищает от изменений только саму переменную, а не её содержимое.

Удаление свойства
```js
delete user.age;
```

Если имя свойства состоит из нескольких слов, но тогда оно должно быть заключено в кавычки: 
```js
let user = {
  "likes birds": false,
};
```

Для свойств, имена которых состоят из нескольких слов, доступ к значению «через точку» не работает: 
```js
  user.likes birds = true; // синтаксическая ошибка
```

Для таких случаев существует альтернативный способ доступа к свойствам через квадратные скобки.
```js
    user["likes birds"] = true;       // успешное присваивание значения свойству
    console.log(user["likes birds"]); // true
    delete user["likes birds"];       // удаление свойства
```

Квадратные скобки также позволяют обратиться к свойству, имя которого может быть результатом выражения.
```js
let key = "likes birds";
user[key] = true; // то же самое, что и user["likes birds"] = true;
```

Вычисляемые свойства
```js
let fruit = 'apple';
let brand = 'Dell';

let bag = {
	[fruit + 'Computers']: 5,
	[`${brand} Computers`]: 2,
};

console.log(bag.appleComputers); // 5
console.log(bag['Dell Computers']); // 2
```

Если название свойств совпадают с названиями переменных, которые мы подставляем в качестве значений этих свойств, то можно использовать специальные _короткие свойства_ для упрощения этой записи.
```js
function makeUser(name, age) {
  return {
    name, // то же самое, что и name: name
    age   // то же самое, что и age: age
  };
}
```

Мы можем использовать как обычные свойства, так и короткие в одном и том же объекте:
```js
let user = {
  name,  // тоже самое, что и name:name
  age: 30
};
```


### Проверка существования свойства оператором «in»
```js
let user = {name: "John", age: 30};

console.log("age" in user); //true
console.log("foo" in user); //false
```

Cлева от оператора in должно быть имя свойства. Обычно это строка в кавычках.
Если мы опускаем кавычки, это значит, что мы указываем переменную, в которой находится имя свойства.

```js
let user = { age: 30 };
let key = "age";

console.log(key in user); // true
```


### Цикл `"for...in"`
```js
let user = {name: "John", age: 30, isAdmin: true};

for (let key in user) {
	console.log(key)       // name, age, isAdmin
	console.log(user[key]) // John, 30, true
}
```

### Цикл `"for...of"` для  `Object.entries()`

```js
const entries = Object.entries(person);

for (let [key, value] of entries) {
	console.log(`${key} : ${value}`);
}
```


### Упорядочение свойств объекта
Cвойства упорядочены особым образом: свойства с целочисленными ключами сортируются по возрастанию, остальные располагаются в порядке создания. 
Термин «целочисленное свойство» означает строку, которая может быть преобразована в целое число и обратно без изменений.
То есть, `"49"`  – это целочисленное имя свойства, потому что если его преобразовать в целое число, а затем обратно в строку, то оно не изменится. А вот свойства  `"+49"`  или  `"1.2"`  таковыми не являются.

```js
let codes = {
	"49": "Германия",
	name: "Alex",
	"41": "Швейцария",
	"44": "Великобритания",
	1: "США",
};

for (let code in codes) {
	console.log(code); // 1, 41, 44, 49, name
}
```

Нет никаких ограничений к именам свойств. Они могут быть в виде строк или символов. Все другие типы данных будут автоматически преобразованы к строке. Например, если использовать число `1` в качестве ключа, то оно превратится в строку  `"1"`

___
## Копирование объектов

Одно из фундаментальных отличий объектов от примитивов заключается в том, что объекты хранятся и копируются «по ссылке», тогда как примитивные значения: строки, числа, логические значения и т.д. – всегда копируются «как целое значение».

```js
let message = "Привет!"; 
let phrase = message;
```
![[js-object-copy.png | 300]]


Переменная, которой присвоен объект, хранит не сам объект, а «ссылку» на него. 
Объект хранится где-то в памяти, в то время как переменная имеет лишь «ссылку» на него.
При копировании переменной объекта копируется ссылка, но сам объект не дублируется.

```js
let user = { name: "John" }; 
let admin = user;
```

То есть две переменные, каждая из которых содержит ссылку на один и тот же объект.

![[js-object-copy-2.png | 500]]

```js
let user = { name: 'John' };
let admin = user;

admin.name = 'Pete'; // изменено по ссылке из переменной "admin"
console.log(user.name);  // 'Pete', изменения видны по ссылке из переменной "user"
```


### Сравнение по ссылке

Два объекта равны только в том случае, если это один и тот же объект.
```js
let a = {}; 
let b = a; // копирование по ссылке 
console.log( a === b ); // true, обе переменные ссылаются на один и тот же объект 
```

В этом случае два объекта не равны, не смотря на то, что оба выглядят одинаково (оба пусты)
```js
let a = {}; 
let b = {}; // два независимых объекта 
console.log( a === b ); // false
```


### Поверхностное клонирование и объединение
Клонирование объектов, свойства которого являются примитивами.

```js
let user = {name: "John", age: 30};

let clone = {}; 

for (let key in user) {
  clone[key] = user[key];
}
```

#### Метод Object.assign

```javascript
Object.assign(dest, [src1, src2, src3...])
```
- Первый аргумент  `dest`  - целевой объект.
- Остальные аргументы  `src1, ..., srcN`  (может быть столько, сколько необходимо) являются исходными объектами
- Метод копирует свойства всех исходных объектов  `src1, ..., srcN`  в целевой объект  `dest`. Другими словами, свойства всех аргументов, начиная со второго, копируются в первый объект.
- Возвращает объект  `dest`.

```js
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

Object.assign(user, permissions1, permissions2);
// или можно использовать сам объект
Object.assign(user, permissions1, { canEdit: true });

console.log(user) // { name: "John", canView: true, canEdit: true }
```

Если скопированное имя свойства уже существует, оно будет перезаписано:
```js
let user = { name: "John" };

Object.assign(user, { name: "Pete" });

console.log(user.name); // "Pete"
```

Мы также можем использовать  `Object.assign`  для замены цикла  `for..in`  для простого клонирования. Он копирует все свойства  `user`  в пустой объект и возвращает его.
```js
let user = {name: "John",age: 30};
let clone = Object.assign({}, user); 
```

Также объект можно клонировать с использованием оператора расширения (spread)
```js
let user = {name: "John", age: 30};
let clone = {...user}; 
```

Можно клонировать объект и сразу добавить новые свойства
```js
let user = {name: "John", age: 30};
let clone = {...user, isMarried: true}; 
```

### Вложенное клонирование
Клонирование объектов, свойства которого могут являются не только примитивами, но и ссылками на другие объекты.

```js
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

console.log( user.sizes.height ); // 182
```

Теперь недостаточно просто скопировать  `clone.sizes = user.sizes` , потому что  `user.sizes`- это объект, он будет скопирован по ссылке. Таким образом, `clone`  и  `user`  будут иметь общий объект  `sizes`:

```js
let clone = Object.assign({}, user); 
console.log( user.sizes === clone.sizes ); // true, тот же объект 

// user и clone обладают общим свойством sizes

user.sizes.width++; // изменяем свойства в объекте user
console.log(clone.sizes.width); // 51, видим результат в другом объекте clone
```

Чтобы исправить это, мы должны использовать цикл клонирования, который проверяет каждое значение  `user[key]`  и, если это объект, тогда также копирует его структуру. Это называется «глубоким клонированием».

Глубокое копирование может быть выполнено:
- функция `structuredClone`
```js
const deepCopy = structuredClone(obj)
```
- библиотека `Lodash`
```js
import cloneDeep from 'lodash/cloneDeep'
const deepCopy = cloneDeep(obj);
```
- JSON
```js
const deepCopy = JSON.parse(JSON.stringify(obj))
```
`JSON.stringify` может обрабатывать только базовые объекты, массивы и примитивы.
Любой другой тип может быть обработан непредсказуемым образом. Например, Dates преобразуются в string. Но `Set` просто преобразуется в `{}`. Что-то `JSON.stringify` даже игнорирует – например, `undefined` или функции.

### Object.keys, Object.values, Object.entries

```js
const person = {
	firstName: 'Alex',
	lastName: 'Newman'
};

console.log(Object.keys(person)); // [ 'firstName', 'lastName' ]

console.log(Object.values(person)); // [ 'Alex', 'Newman' ]

console.log(Object.entries(person)) // [[ 'firstName', 'Alex' ], [ 'lastName', 'Newman' ]]
```


### Sealing \[запечатывание\] an object globally   

```js
Object.preventExtensions(obj) // запрещает добавлять новые свойства в объект
```

```js
Object.seal(obj) // запрещает добавлять/удалять свойства
```

```js
Object.freeze(obj) // запрещает добавлять/удалять/изменять свойства. 
//Отличие от Object.seal(obj) только в запрете изменения свойства
```

```js
const frozen = Object.freeze({ username: 'johnsmith' });
const sealed = Object.seal({ username: 'johnsmith' });

frozen.name = 'John Smith';  // frozen = { username: 'johnsmith' }
sealed.name = 'John Smith';  // sealed = { username: 'johnsmith' }

delete frozen.username;      // frozen = { username: 'johnsmith' }
delete sealed.username;      // sealed = { username: 'johnsmith' }

frozen.username = 'jsmith';  // frozen = { username: 'johnsmith' }
sealed.username = 'jsmith';  // sealed = { username: 'jsmith' }
```

