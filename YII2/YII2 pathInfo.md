#yii2 

If URL is main page
```php
<? if (yii::$app->request->pathInfo == '') { ?>
	<?= $item['name']?>
<?} else {?>
	<a href="/foo/bar/">
		<?= $item['name']?>
	</a>
<?}?>

echo yii::$app->request->pathInfo //on main page will be blank or '/info/contacts' on contacts page
```