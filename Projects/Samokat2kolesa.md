#samokat2kolesa 


### Base style reset
```css
h1 {
	line-height: initial;
	letter-spacing: initial;
}

p {
	margin-bottom: 0;
	line-height: initial;
	letter-spacing: initial;
}

ol,
ul {
	margin-bottom: 0;
	padding-left: 0;
}
```
___


### urlManager

Файл `./config/web.php`
```php
$config = [
	'components' => [
		'urlManager' => [
			'enablePrettyUrl' => true,
			'showScriptName' => false,
			'suffix' => '/',
			'rules' => [
				'' => 'site/glavnaya',
				'api/<action>' => 'admin/<action>',
				'build/<action>' => 'build/<action>',
				'articles/<alias:[^|]+>' => 'site/articles',
				'vakansii/<action>' => 'site/<action>',
				['pattern' => 'sitemap', 'route' => 'site/sitemap', 'suffix' => '.xml'],
				['pattern' => 'turbo', 'route' => 'site/turbo', 'suffix' => '.xml'],
				['pattern' => 'robots', 'route' => 'site/robots', 'suffix' => '.txt'],
				'<action>' => 'site/<action>',
				'<action>/<id:\d+>' => 'site/<action>',
			],
		],
	],
];
```
___
