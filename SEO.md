### Установка Вебмастера

Устанавливается в конец тэга \<head>
```html
<meta name="yandex-verification" content="0f19741690a66f72" />
```
___

### Установка Яндекс Метрики

Создается чанк \[\[$YandexMetrika]] и вставляется после тэга \<footer>

```html
<!-- Yandex.Metrika counter -->
<script type="text/javascript">
    (function (m, e, t, r, i, k, a) {
        m[i] = m[i] || function () { (m[i].a = m[i].a || []).push(arguments); };
        m[i].l = 1 * new Date();
        for (var j = 0; j < document.scripts.length; j++) { if (document.scripts[j].src === r) { return; } }
        k = e.createElement(t), a = e.getElementsByTagName(t)[0], k.async = 1, k.src = r, a.parentNode.insertBefore(k, a);
    })
        (window, document, "script", "https://mc.yandex.ru/metrika/tag.js", "ym");
    ym(92673755, "init", {
        clickmap: true,
        trackLinks: true,
        accurateTrackBounce: true,
        webvisor: true
    });
</script>
<noscript>
    <div><img src="https://mc.yandex.ru/watch/92673755" style="position:absolute; left:-9999px;" alt="" /></div>
</noscript>
```
___

### Разметка  OpenGraph

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


