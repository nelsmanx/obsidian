### SASS compiler

```bash
npm i -D sass gulp-sass gulp-sourcemaps
```

```js
const scss = require('gulp-sass')(require('sass'));
const sourcemaps = require('gulp-sourcemaps');
```
___

### Необходимо сигнализировать об окончании задачи.

- Через `callback`

```js
const html = cb => {
    src('./src/index.html')
        .pipe(dest('./public'));

    cb();
};
```

- Через `return`
```js
const html = () => {
    return src('./src/index.html')
        .pipe(dest('./public'));
};
```
___

### Перечисление расширений файлов через `{,}`
```js
src('./src/*.{html,css}')
```
___


![[Pasted image 20240505200629.png]]



