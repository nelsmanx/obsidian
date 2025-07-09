```html
<html lang="ru" prefix="og: https://ogp.me/ns#">
<head>
	<meta property="og:locale" content="ru_RU" />
	<meta property="og:title" content="[[*pagetitle]]" />
	<meta property="og:description" content='[[*description]]' />
	<meta property="og:type" content="website" />
	<meta property="og:image" content="[[!++site_url]]assets/img/logo-open-graph.png" />
	<meta property="og:url" content="[[++site_url]][[*uri]]" />
</head>


Также можно использовать такую запись 
<meta property="og:url" content="[[~[[*id]]? &scheme=`full`]]" />
```