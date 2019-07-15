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

###### deploy mode
```cli
php bin/magento deploy:mode:show
php bin/magento deploy:mode:set developer
php bin/magento deploy:mode:set production
php bin/magento setup:static-content:deploy
php bin/magento setup:static-content:deploy fr_FR (specify if it's not defaul language)
```

###### best way to deploy in production
```cli
php bin/magento setup:upgrade
php bin/magento indexer:reindex
php bin/magento deploy:mode:set production -s (skip di:compile)
php bin/magento setup:di:compile
php bin/magento setup:static-content:deploy (fr_FR -specify if it's not defaul language)
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

###### adding category attribute
https://www.mageplaza.com/devdocs/magento-2-category-attributes-programmatically/

##### using third party librairires

###### Login with \Alekseon\Logger\Logger
https://packagist.org/packages/alekseon/logger
```php
$message    = 'something';
$fileName   = 'payment.log';
\Alekseon\Logger\Logger::info($message, $fileName);
```

##### hacks

###### log levels
vendor/magento/framework/Logger/Handler/System.php
vendor/psr/log/Psr/Log/LogLevel.php
```php
    const EMERGENCY = 'emergency';
    const ALERT     = 'alert';
    const CRITICAL  = 'critical';
    const ERROR     = 'error';
    const WARNING   = 'warning';
    const NOTICE    = 'notice';
    const INFO      = 'info';
    const DEBUG     = 'debug';
```

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

###### limit street field length in checkout
https://magento.stackexchange.com/questions/143337/add-html-attribute-in-checkout-street-address/143352#143352

###### limit street field length in address book
app/design/frontend/Theme/vendor_name/Magento_Customer/templates/address/edit.phtml
````
<input type="text" name="street[]"
       value="<?= $block->escapeHtmlAttr($block->getStreetLine($_i + 1)) ?>"
       title="<?= $block->escapeHtmlAttr(__('Street Address %1', $_i + 1)) ?>"
       id="street_<?= /* @noEscape */ $_i + 1 ?>"
       class="input-text <?= $block->escapeHtmlAttr($_streetValidationClass) ?>"
       maxlength="38">
````



##### backoffice

###### noindex / nofollow
Content > Design > Configuration > Choose Overall Design (Edit) > Search Engines Robots

###### Menu xml in backend
````
System  (Magento_Backend::system)
  Report  (Magento_Backend::system_report)
  Tools   (Magento_Backend::system_tools)
  Data Transfer   (Magento_Backend::system_convert)
  Other Settings  (Magento_Backend::system_other_settings)
  Extensions  (Magento_Integration::system_extensions)
  Permissions (Magento_User::system_acl)
Dashboard   (Magento_Backend::dashboard)
System  (Magento_Backend::system)
Marketing   (Magento_Backend::marketing)
Content (Magento_Backend::content)
Stores  (Magento_Backend::stores)
Products    (Magento_Catalog::catalog)
Cuurency     (Magento_Backend::system_currency)
Customers   (Magento_Customer::customer)
Find Partners & Extensions (Magento_Marketplace::partners)
Reports    (Magento_Reports::report)
Sales  (Magento_Sales::sales)
````
https://alanstorm.com/magento_2_admin_menu_items/


Magento php7.2 - Packages
sudo apt-get install php7.2-opcache php7.2-gd php7.2-mysql php7.2-curl php7.2-intl php7.2-xsl php7.2-mbstring php7.2-zip php7.2-bcmath php7.2-soap
