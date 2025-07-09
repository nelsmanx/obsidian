
```html
<link rel="stylesheet" href="/path/to/simple-scrollbar.css">
<script src="/path/to/simple-scrollbar.min.js"></script>


<div class="ul-wrap ss-container">
	<ul>
		<li></li>
		<li></li>
		<li></li>
		...
	</ul>
</div>

<style>
	.ul-wrap {
        height: 200px;
        /* no overflow! */
    }
    
   .ul-wrap .ss-scroll {
        width: 4px;
        background-color: rgba(0, 0, 0, 0.15);
        opacity: 1;
    }
    
   .ul-wrap ul {
        padding-right: 28px;
    }
</style>

<script>
    const el = document.querySelector('.ul-wrap');
    SimpleScrollbar.initEl(el);
</script>
```

[Github](https://github.com/buzinas/simple-scrollbar)