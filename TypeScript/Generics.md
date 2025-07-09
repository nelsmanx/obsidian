Generics - это обобщенный тип данных. Дженерики помогают писать универсальный, переиспользуемый код. Главная задача дженериков — помочь разработчику писать код, который одинаково будет работать со значениями разных типов.

Можно написать два разных класса для обработки массива чисел и массива строк 
```ts
class ArrayOfNumbers {
	collection

	constructor(collection: number[]) {
		this.collection = collection;
	}

	getItem(index: number): number {
		return this.collection[index]
	}
}

class ArrayOfStrings {
	collection

	constructor(collection: string[]) {
		this.collection = collection
	}

	getItem(index: number): string {
		return this.collection[index]
	}
}

const numberArray = new ArrayOfNumbers([1, 2, 3])
const stringArray = new ArrayOfStrings(['a', 'b', 'c'])
```

Можно воспользоваться дженериком
```ts
class ArrayOfUniversalType<T> {
	constructor(public collection: T[]) {
		this.collection = collection
	}

	getItem(index: number): T {
		return this.collection[index]
	}
}

const numberUniversalArray = new ArrayOfUniversalType<number>([5, 6, 7])
const stringUniversalArray = new ArrayOfUniversalType<string>(['a', 'b', 'c'])
```

Примеры использования дженериков
```ts
const printUniversal = <T>(array: T[]): void => {
    array.forEach(item => console.log(item))
}

printUniversal<number>([1, 2, 3])
```

```ts
function fillArray<T>(arrayLength: number, el: T): T[] {
    return new Array<T>(arrayLength).fill(el)
}

fillArray<string>(5, '*');
```

```ts
interface Lengthwise {
	length: number
}

function getLength<T extends Lengthwise>(el: T): number {
	return el.length
}

console.log(getLength<string>('hello'));
console.log(getLength<number[]>([1, 2, 3]));


interface Foo {
	a: number
	length: number
}

console.log(getLength<Foo>({ a: 1, length: 2 }));
```

```ts
interface User {
	firstName: string,
	lastName: string
}

function getProperty<T, K extends keyof T>(object: T, key: K): T[K] {
	return object[key]
}

const user1: User = {
	firstName: 'Ivan',
	lastName: 'Ivanov'
}
// <K> ==== 'firstName' | 'lastName'

console.log(getProperty<User, 'firstName'>(user1, 'firstName'));
```