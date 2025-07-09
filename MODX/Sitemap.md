```php
<section class="sitemap">
    <div class="container">
        <h1 class="sitemap__title">Карта сайта</h1>
        [[pdoMenu?
            &parents=`-15,-16,-52`
            &firstClass=`0`
            &lastClass=`0`
            &countChildren=`1`
            
        	&tplOuter=`@INLINE <ul class="sitemap__list">[[+wrapper]]</ul>`
        	&tpl=`@INLINE <li class="sitemap__item"><a class="sitemap__link" href="[[+link]]" [[+attributes]]>[[+menutitle]]</a>[[+wrapper]]</li>`
        	&tplParentRow=`@INLINE <li class="sitemap__item sitemap__item--sublist"><a class="sitemap__link sitemap__link--item" href="[[+link]]"[[+attributes]]>[[+menutitle]] ([[+children]])</a>[[+wrapper]]</li>`
        	&tplInner=`@INLINE <ul class="sitemap__sublist">[[+wrapper]]</ul>`
        	&tplInnerRow=`@INLINE <li class="sitemap__item"><a class="sitemap__link sitemap__link--sublist" href="[[+link]]" [[+attributes]]>[[+menutitle]]</a>[[+wrapper]]</li>`
        ]]
    </div>
</section>



<section class="sitemap">
    <div class="container">
        <h1 class="sitemap__title">Карта сайта</h1>
        [[pdoMenu?
            &parents=`-16,-651`
            &resources=`-3,-4,-5`
            
            &showUnpublished=`1`
            &showHidden=`1`
            
            &firstClass=`0`
            &lastClass=`0`
            
        	&tplOuter=`@INLINE <ul class="sitemap__list sitemap__list--1">[[+wrapper]]</ul>`
        	&tpl=`@INLINE <li class="sitemap__item"><a class="sitemap__link" href="[[+link]]" [[+attributes]]>[[+menutitle]]</a>[[+wrapper]]</li>`
        	&tplParentRow=`@INLINE <li class="sitemap__item sitemap__item--sublist"><a class="sitemap__link sitemap__link--item" href="[[+link]]"[[+attributes]]>[[+menutitle]]</a>[[+wrapper]]</li>`
        	&tplInner=`@INLINE <ul class="sitemap__sublist">[[+wrapper]]</ul>`
        	&tplInnerRow=`@INLINE <li class="sitemap__item"><a class="sitemap__link sitemap__link--sublist" href="[[+link]]" [[+attributes]]>[[+menutitle]]</a>[[+wrapper]]</li>`
        ]]
        
    </div>
</section>
```

```css
/* ##### SITEMAP SECTION ##### */
.sitemap {
	padding: 50px 0;
}

.sitemap__title {
	margin-bottom: 30px;
	font-size: 28px;
}

.sitemap__list {}

.sitemap__item {}

.sitemap__item:not(:last-child) {
	margin-bottom: 10px;
}

.sitemap__link {
    font-weight: 500;
}

.sitemap__link:hover {
    color: #faae41;
}

.sitemap__link--item {
    display: block;
    margin-bottom: 10px;
}

.sitemap__item--sublist .sitemap__sublist {
	padding-left: 30px;
}

.sitemap__link--sublist {
    font-weight: 400;
}


.seo-text {
	padding: 0 15px 60px;
}

.seo-text h2 {
	font-size: 32px;
	font-weight: 500;
}
.seo-text h3 {
	font-size: 26px;
	font-weight: 500;
}
.seo-text p, .seo-text ul li, .seo-text ol li {
	margin-bottom: 14px;
	font-size: 14px;
	color: rgba(0, 0, 0, 0.85);
}

```