### Installing

```bash
npm install -D gulp-nunjucks
```

### Gulp task

```jsx
const nunjucks = require('gulp-nunjucks');
const nunjucks_lib = require('nunjucks'); // 'nunjucks' is a part of 'gulp-nunjucks', no need to install seperatly

//nunjucks can't reach path "outside the box", so you should add the base directory in 'evn'.

function njk() {
	return src('assets/html/pages/*.njk')
		.pipe(nunjucks.compile({}, {
			env: new nunjucks_lib.Environment(new nunjucks_lib.FileSystemLoader('./assets/html/'))
		}))
		.pipe(dest('./'));
}

function watching() {
	watch(['assets/html/**/*.njk'], njk);
}

exports.default = parallel(watching);
```

### File structure

```html
html
	|- layouts
		|- main.njk
	|- pages
			|- index.njk
	|- partials
			|- _header.njk
			|- _footer.njk
```

### main.njk

```html
<!DOCTYPE html>
<html lang="ru">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
		<link rel="stylesheet" href="assets/css/main.css" />
		<title>{{ title }}</title>
	</head>
	<body></body>
</html>

{% include "partials/_header.njk" %}

<main>
	{% block content %}{% endblock %}
</main>

{% include "partials/_footer.njk" %}
```

### index.njk

```html
{% extends "layouts/main.njk" %}
{% set title = 'Main page' %}

{% block content %}

<!-- ===== HERO SECTION ===== -->
<section class="hero"></section>

{% endblock %}
```


[Nunjucks](https://siteok.org/blog/html/nunjucks)
![[nunjucks.png]]