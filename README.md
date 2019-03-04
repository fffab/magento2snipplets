# magento2snipplets
Some quickies Magento 2 snipplets

###### Login with \Alekseon\Logger\Logger
https://packagist.org/packages/alekseon/logger
```php
$message    = 'something';
$fileName   = 'payment.log';
\Alekseon\Logger\Logger::info($message, $fileName);
```

###### php bin/magento modules stuff

```cli
php bin/magento module:status
php bin/magento module:enable Vendor_Module
php bin/magento module:disable Vendor_Module
php bin/magento module:uninstall Vendor_Module
```
