# magento2snipplets
Some quickies Magento 2 snipplets

## cli 

### php bin/magento modules stuff

```cli
php bin/magento module:status
php bin/magento module:enable Vendor_Module
php bin/magento module:disable Vendor_Module
php bin/magento module:uninstall Vendor_Module
```

### php bin/magento cache
```cli
php bin/magento cache:status
php bin/magento cache:disable
php bin/magento cache:enable
php bin/magento cache:clear - c:c
php bin/magento cache:flush - c:f
```

### remove code generated
```cli
rm -r generated/code
```

### deploy mode
```cli
php bin/magento deploy:mode:show
php bin/magento deploy:mode:set developer
php bin/magento deploy:mode:set production
php bin/magento setup:static-content:deploy
php bin/magento setup:static-content:deploy fr_FR (specify if it's not defaul language)
```

### best way to deploy in production
```cli
php bin/magento setup:upgrade
php bin/magento indexer:reindex
php bin/magento deploy:mode:set production -s (skip di:compile)
php bin/magento setup:di:compile
php bin/magento setup:static-content:deploy (fr_FR -specify if it's not defaul language)
```

## phtlm 

### call helper in phtml, fast
```php
$_myHelper = $this->helper('Vendor\Module\Helper\Helpername');
$_myHelper->myFunction();
```

### call static cms bloc in phtml, fast
```php
echo $block->getLayout()->createBlock('Magento\Cms\Block\Block')->setBlockId('blockIdentifier')->toHtml();
```

### get base url in phtml (most of the cases it works)
```php
$this->getBaseUrl();
```

### adding js that require some other js in phtml
```js
<script type="text/javascript">
  require([ 'jquery'], function($){ $(document).ready(function($) {
    // jscode
  }); });
</script>
```

### override .html / ko
create a module Vendor_Module
add html to overide in view/frontend/web/template
add requirejs-config.js in view/frontend
like this one
```js
var config = {
    map: {
       '*': {
           'Magento_CheckoutAgreements/template/checkout/checkout-agreements.html': 'Vendor_Module/template/checkout/checkout-agreements.html'
       }
    }
};
```

### override template
https://www.classyllama.com/blog/template-override-m2


## attributes

### adding category attribute
https://www.mageplaza.com/devdocs/magento-2-category-attributes-programmatically/


## others howtos

### log levels
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

### remove a module installed by ftp

```
Table : setup_module, delete the according line
File  : app/ect/config.php, delete the according line
Database : delete the according tables if any
Cli : setup:upgrade
```

### products not showing up in catogeries
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
### re-index only products
```
php bin/magento indexer:reindex catalog_product_flat catalog_category_flat catalog_category_product catalog_product_category catalogrule_rule catalog_product_attribute catalog_product_price catalogrule_product cataloginventory_stock catalogsearch_fulltext
```

### limit street field length in checkout
https://magento.stackexchange.com/questions/143337/add-html-attribute-in-checkout-street-address/143352#143352

### limit street field length in address book
app/design/frontend/Theme/vendor_name/Magento_Customer/templates/address/edit.phtml
````
<input type="text" name="street[]"
       value="<?= $block->escapeHtmlAttr($block->getStreetLine($_i + 1)) ?>"
       title="<?= $block->escapeHtmlAttr(__('Street Address %1', $_i + 1)) ?>"
       id="street_<?= /* @noEscape */ $_i + 1 ?>"
       class="input-text <?= $block->escapeHtmlAttr($_streetValidationClass) ?>"
       maxlength="38">
````

### using log
````
$logger = \Magento\Framework\App\ObjectManager::getInstance()->get('\Psr\Log\LoggerInterface');
$logger->debug(__METHOD__ . ' - ' . __LINE__);       
````
or
````
    protected $_logger;
 
    public function __construct(
        \Psr\Log\LoggerInterface $logger,
        ...
    )
    {        
        $this->_logger = $logger;
    }
    
    function... {
        $this->_logger->debug('debug1234'); 
        //Output: [2017-02-22 04:48:44] main.DEBUG: debug1234 {"is_exception":false} []
        
        $this->_logger->info('info1234'); 
        // Write to default log file: var/log/system.log
        //Output: [2017-02-22 04:52:56] main.INFO: info1234 [] []
        
        $this->_logger->alert('alert1234'); 
        // Write to default log file: var/log/system.log
        //Output: [2017-02-22 04:52:56] main.ALERT: alert1234 [] []
        
        $this->_logger->notice('notice1234'); 
        // Write to default log file: var/log/system.log
        //Output: [2017-02-22 04:52:56] main.NOTICE: notice1234 [] []
        
        // Write to default log file: var/log/system.log
        $this->_logger->error('error1234'); 
        //Output: [2017-02-22 04:52:56] main.ERROR: error1234 [] []
        
         // Write to default log file: var/log/system.log
        $this->_logger->critical('critical1234'); 
        //Output: [2017-02-22 04:52:56] main.CRITICAL: critical1234 [] []
     }
````

#### magento path hint
Stores > Configuration > Advanced > Developer > Debug: Enable Template Path Hints for Admin/Front 

## backoffice

### noindex / nofollow
Content > Design > Configuration > Choose Overall Design (Edit) > Search Engines Robots

### Menu xml in backend
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


## installation

### Magento php7.2 - Packages

sudo apt-get install php7.2-opcache php7.2-gd php7.2-mysql php7.2-curl php7.2-intl php7.2-xsl php7.2-mbstring php7.2-zip php7.2-bcmath php7.2-soap


## using third party librairires

### Login with \Alekseon\Logger\Logger
https://packagist.org/packages/alekseon/logger
```php
$message    = 'something';
$fileName   = 'payment.log';
\Alekseon\Logger\Logger::info($message, $fileName);
```

### Theme Porto - Errors and pitfalls
In case of config css not loading, 404 and finally not regenerating
go to admin, configuration > porto > settings, save each levels again
should be enough
if note, try design, and even content > theme
