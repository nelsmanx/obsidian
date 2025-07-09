#### push  `arr.push(...items)`

Добавляет элемент/элементы в конец массива. 
```js
const fruits = ["Яблоко", "Апельсин", "Груша"];

fruits.push("Груша");
console.log(fruits); // [ 'Яблоко', 'Апельсин', 'Груша', 'Груша' ]

const newFruits = ["Дыня", "Слива"];
fruits.push(...newFruits);
console.log(fruits); // [ 'Яблоко', 'Апельсин', 'Груша', 'Груша', 'Дыня', 'Слива' ]
```
Вызов `fruits.push(...)` равнозначен `fruits[fruits.length] = ...`
___

#### unshift  `arr.unshift(...items)`

Добавляет элемент/элементы в начало массива.
```js
const fruits = ["Яблоко", "Апельсин", "Груша"];

fruits.unshift("Груша");
console.log(fruits); // [ 'Груша', 'Яблоко', 'Апельсин', 'Груша' ]

const newFruits = ["Дыня", "Слива"];
fruits.unshift(...newFruits);
console.log(fruits); // [ 'Дыня', 'Слива', 'Груша', 'Яблоко', 'Апельсин', 'Груша' ]
```
___
#### pop  `arr.pop()`

Удаляет последний элемент из массива и возвращает его
```js
const fruits = ["Яблоко", "Апельсин", "Груша"];

const lastRemovedFruit = fruits.pop();

console.log(fruits); // [ 'Яблоко', 'Апельсин' ]
console.log(lastRemovedFruit); // 'Груша'
```
___

#### shift  `arr.shift()`

Удаляет из массива первый элемент и возвращает его:
```js
const fruits = ["Яблоко", "Апельсин", "Груша"];

const firstRemovedFruit = fruits.shift();

console.log(fruits); // [ 'Апельсин', 'Груша' ]
console.log(firstRemovedFruit); // 'Яблоко'
```
___

