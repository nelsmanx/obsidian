#yii2 
### Registering after AppAsset

```php
	$this->registerCssFile('@web/css/vue-calc.css', ['depends' => 'frontend\assets\AppAsset']);

$this->registerJsFile('@web/js/vue-calc.js', ['depends' => 'frontend\assets\AppAsset']);

// another way
$this->registerCssFile("@web/css/contacts.css", ['depends' => [\app\assets\AppAsset::className()]]);

$this->registerJsFile('@web/js/contacts.js', ['depends' => [\app\assets\AppAsset::className()]]);
```
___

### Registering before AppAsset

```php
$this->registerCssFile("@web/css/contacts.css");

$this->registerJsFile('@web/js/contacts.js');
```
___

### Registering inline

```php
$this->registerCss("body { background: #f00; }");
```
___

## Creating and registration asset bundles

Creating asset bundle
```php
<?php
namespace frontend\assets;
use frontend\assets\AppAsset;

class AppAssetIndex extends AppAsset
{
    public $basePath = '@webroot';
    public $baseUrl = '@web';
    public $css = [
    	'css/pages/index.css'
    ];
    
    public $depends = [
        'frontend\assets\AppAsset'
    ];
}
```

Registration asset bundle on page
```php
use frontend\assets\AppAssetIndex;
AppAssetIndex::register($this);
```