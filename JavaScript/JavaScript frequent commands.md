#js 

### Get element's ID value
```js
const identifier = foo.id
```


### Attributes
```js
item.dataset.src; //get value of data-src 
item.dataset.quizQuestion = '1'; //set value data-quiz-question
delete item.dataset.src // delete data-src
```


### Unicode character insert
```js
USD: `${amount * 70}\u0024`,
```


### Create CSS variable in :root
```js
document.documentElement.style.setProperty("--variable", `100px`);
```

### Elements width and height
```js
let box = document.querySelector('.box'); 
let width = box.offsetWidth; 
let height = box.offsetHeight;
```