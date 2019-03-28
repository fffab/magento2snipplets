# magento2snipplets
Some quickies Magento 2 snipplets

##### cli 

###### php bin/magento modules stuff

```cli
php bin/magento module:status
php bin/magento module:enable Vendor_Module
php bin/magento module:disable Vendor_Module
php bin/magento module:uninstall Vendor_Module
```

###### php bin/magento cache
```cli
php bin/magento cache:status
php bin/magento cache:disable
php bin/magento cache:enable
php bin/magento cache:clear - c:c
php bin/magento cache:flush - c:f
```

###### remove code generated
```cli
rm -r generated/code
```

##### phtlm 

###### call helper in phtml, fast
```php
$_myHelper = $this->helper('Vendor\Module\Helper\Helpername');
$_myHelper->myFunction();
```

###### call static cms bloc in phtml, fast
```php
echo $block->getLayout()->createBlock('Magento\Cms\Block\Block')->setBlockId('blockIdentifier')->toHtml();
```

###### get base url in phtml (most of the cases it works)
```php
$this->getBaseUrl();
```

###### adding js that require some other js in phtml
```js
<script type="text/javascript">
  require([ 'jquery'], function($){ $(document).ready(function($) {
    // jscoque
  }); });
</script>
```

##### using third party librairires

###### Login with \Alekseon\Logger\Logger
https://packagist.org/packages/alekseon/logger
```php
$message    = 'something';
$fileName   = 'payment.log';
\Alekseon\Logger\Logger::info($message, $fileName);
```

##### hacks

###### remove a module installed by ftp

```
Table : setup_module, delete the according line
File  : app/ect/config.php, delete the according line
Database : delete the according tables if any
Cli : setup:upgrade
```

###### products not showing up in catogeries
```
1.General->Status = Enabled
2.general->Visibility = Catalog,Search (at last Catalog)
3.Inventory->Qty > 0
4.Inventory->Stock Availability = In Stock
5.Websites = checking your site
6.Catgories = checking your category.

optionnal - php bin/magento setup:static-content:deploy
php bin/magento indexer:reindex
```

##### backoffice

###### noindex / nofollow
Content > Design > Configuration > Choose Overall Design (Edit) > Search Engines Robots
