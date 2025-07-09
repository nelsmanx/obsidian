### Bootstrap 3: how to make head of dropdown link clickable in navbar

```html
<nav class="navbar navbar-fixed-top admin-menu" role="navigation">
  <div class="navbar-header">...</div>
   <!-- Collect the nav links, forms, and other content for toggling -->
   <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
     <ul class="nav navbar-nav navbar-right">
       <li class="dropdown">
         <a class="dropdown-toggle" data-toggle="dropdown" href="#">
           I DONT WORK! <b class="caret"></b>
         </a>
         <ul class="dropdown-menu">
           <li><a href="/page2">Page2</a></li>
         </ul>
       </li>
       <li><a href="#">I DO WORK</a></li>
     </ul>               
   </div><!-- /.navbar-collapse -->
 </nav>
```

Just add the class `disabled` on your anchor:

```html
<a class="dropdown-toggle disabled" href="{your link}">Dropdown</a>
```