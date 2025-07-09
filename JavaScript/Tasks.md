### Напишите функцию get, принимающую последовательность ключей и возвращающую значение в объекте произвольной вложенности. 

```
input: "red.big.apple", { red: { big: { apple: 🍎 } } }
output: 🍎 
```

```js
const get = (keySequence, nestedObject) => {
	const keysArray = keySequence.split('.');

	return keysArray.reduce((obj, key) => {
		return obj[key];
	}, nestedObject);
};

get("red.big.apple", { red: { big: { apple: 🍎 } } }
```

На каждой итерации мы возвращаем значение каждого последующего ключа (все значения, кроме последнего - это объекты) и, в итоге, доходим до последнего. Если значения ключа нет, то возвращается `undefined`
### Удалить элементы с falsy-значениями из массива

`Falsy` - значение, которое становится `false` в булевом контексте. 
К ним относятся:
1. `false`
2. `0` (zero)
3. `''` or `""` (empty string)
4. `null`
5. `undefined`
6. `NaN`

Все остальные - `truthy`

```js
const numbers = [false, true, 0, 1, '', 'hi', null, undefined, NaN];

let truthy = [];

truthy = numbers.filter(number => number);
truthy = numbers.filter(Boolean);

console.log(truthy) // [ true, 1, 'hi' ]
```
___



### Конвертация массива в объект

```js
const carsArray = ["BMW", "Audi", "Skoda", "Mercedes"];

const carsObject = { ...carsArray };
console.log(carsObject); // { 0: 'BMW', 1: 'Audi', 2: 'Skoda', 3: 'Mercedes' }
```

### Найти уникальные значения в массиве

```js
const carsArray = [1, 2, 3, 5, 6, 7, 8, 9, 2, 4, 5, 2, 0];

let unique = [];

unique = [... new Set(carsArray)];
unique = carsArray.filter((item, index, arr) => arr.indexOf(item) === index);

console.log(unique) // [ 1, 2, 3, 5, 6, 7, 8, 9, 4, 0 ]
```

### Очистить массив

```js
const arr = ['a', 'b', 'c', 'd', 'e', 'f'];

// 1 способ
arr.length = 0;

// 2 способ
arr.splice(0, arr1.length);
```

Также можно очистить массив, объявленный через `let`, таким способом

```js
arr = [];
```

Но при таком подходе есть особенности.

```js
let arr1 = [1, 2, 3, 4, 5];
let arr2 = arr1;
arr1 = [];

console.log(arr2); // [ 1, 2, 3, 4, 5 ]
```

Если есть ссылки на исходный массив как у переменной `arr2`, то `arr2` не будет очищен. 

Когда мы создаем `arr2` и присваиваем ему значение` arr1`, мы на самом деле присваиваем ссылку на массив `arr1`, а не сам массив. Таким образом `arr1` и` arr2` указывают на один и тот же массив в памяти.

Когда мы затем пишем `arr1 = [],` то мы создаем новый пустой массив и присваиваем ссылку на него переменной `arr1`. Но это не влияет на переменную `arr2` - она по-прежнему указывает на исходный массив `[ 1, 2, 3, 4, 5 ].`

Поэтому после выполнения `arr1 = []` переменная `arr1` будет указывать на новый пустой массив, а `arr2` будет по-прежнему указывать на исходный массив с элементами.

### Найти пересечение массивов

```js
const arr1 = [1, 1, 2, 2, 3, 3, 4, 4, 5, 5];
const arr2 = [4, 5, 6, 7, 8];

const arraysIntersection = [... new Set(arr1)]
    .filter(item => arr2.includes(item));


console.log(arraysIntersection) // [ 4, 5 ]
```

### Обратить порядок элементов в обратном направлении

```js
const arr = [4, 5, 6, 7, 8];
const reversedArray = [...arr].reverse();

console.log(reversedArray); // [ 8, 7, 6, 5, 4 ]
```

```js
const arr = [4, 5, 6, 7, 8];

function reverseArray(array) {
	const reversedArray = [];

	for (let i = arr.length - 1; i >= 0; i--) {
		reversedArray.push(array[i]);
	}
	
	return reversedArray;
}

console.log(reverseArray(arr)) // [ 8, 7, 6, 5, 4 ]
```

### Заполнить массив числами

Заполнение массива пятью одинаковыми числами 
```js
const arr = new Array(5).fill(1);
console.log(arr); // [ 1, 1, 1, 1, 1 ]
```

Заполнение массива случайными числами от 2 до 5
```js
const randomArray = [];

const randomArrayLength = 10;
const minRandomNumber = 2;
const maxRandomNumber = 5;

for (let i = 0; i <= randomArrayLength - 1; i++) {
	let randomNumber = Math.floor(Math.random() * (maxRandomNumber - minRandomNumber + 1) + minRandomNumber);
	randomArray.push(randomNumber);
}

console.log(randomArray); // [ 4, 2, 3, 2, 4, 2, 2, 3, 3, 4 ]
```

- `Math.random()` генерирует случайное число от 0 до 1, не включая `0`.
- `maxRandomNumber - minRandomNumber + 1`  получение кол-ва чисел от `min` до `max` включительно (в нашем случае от `2` до `5` получается `4)
- `Math.random() * (maxRandomNumber - minRandomNumber + 1) + minRandomNumber` - получение случайного числа от `0` до `4` и  сдвиг полученного значения на нужнжый диапазон с которого должны начинаться случайные числа (`2`)
- `Math.floor()` округляет до ближайшего целого числа в меньшую сторону




### Получить случайное число из массива

```js
const arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

const randomArrayItem = arr[Math.floor(Math.random() * Math.floor(arr.length))];
console.log(randomArrayItem); // 9
```