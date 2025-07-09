### Объявление массива

```js
const arr = [];
```

С помощью конструктора `new Array()`
```js
const arr = new Array()
```

`Array` constructor with a single parameter. 
An array is created with its length property set to that number, and the array elements are empty slots.
```js
const arr = new Array(2)
console.log(arr.length); // 2
console.log(arr[0]) // undefined; actually, it is an empty slot

const arr = new Array("2"); // Not the number 2 but the string "2"
console.log(arr.length); // 1
console.log(arr[0]); // "2"
```

`Array` constructor with a multiple parameters. 
If more than one argument is passed to the constructor, a `new Array` with the given elements is created.
```js
const fruits = new Array("Apple", "Banana");

console.log(fruits.length); // 2
console.log(fruits[0]); // "Apple"
```
___
### Итерация массива
```js
const arr = ['a', 'b', 'c', 'd', 'e'];
```
#### Цикл `for`
```js
for (let i = 0; i < arr.length; i++) {
	console.log(arr[i]);
}
```
#### Цикл `forEach`
```js
arr.forEach(item => console.log(item))
```
Есть особенность: нельзя прервать выполнение цикла ключевым словом `break` или выйти из внешней функции через `return`.
#### Цикл `for ... of`
```js
for (let item of arr) {
	console.log(item);
}
```
В отличие от `forEach()`, он работает с `break`, `continue` и `return`.   
Работает с большинством массивоподобных объектов, вроде списков `NodeList`, а также со строками, `Set` и `Map`
#### Цикл `for ... in` - не использовать!
```js
for (let index in arr) {
    console.log(index); // '0', '1', '2'
    console.log(arr[index]); // 'a', 'b', 'c'
}
```
Значения, присваиваемые `index` - это строки `'0'`, `'1'`, `'2'`,  а не числа.

Цикл `for ... in`  рассчитан на работу с обычными объектами `Object` с именами свойств в виде строк.
Цикл `for..in` выполняет перебор _всех свойств_ объекта, а не только цифровых. Cуществуют так называемые «псевдомассивы» – объекты, которые _выглядят, как массив_. Чаще всего встречаются при работе с DOM. То есть, у них есть свойство `length` и индексы, но они также могут иметь дополнительные нечисловые свойства и методы, которые нам обычно не нужны. Тем не менее, цикл `for..in` выведет и их.

```js
for (let index in headers) {
    console.log(index); 
    // '0', '1', 'entries', 'keys', 'values', 'forEach', 'length', 'item'
}
```
___