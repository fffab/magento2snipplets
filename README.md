# magento2snipplets
Some quickies Magento 2 snipplets

###### Login with \Alekseon\Logger\Logger
```php
$message    = $observer->getEvent()->getMethodInstance()->getCode();
$fileName   = 'payment.log';
\Alekseon\Logger\Logger::info($message, $fileName);
```

###### php bin/magento modules stuff

```cli
php bin/magento module:status
php bin/magento module:enable MVendor_Module
php bin/magento module:disable MVendor_Module
php bin/magento module:uninstall MVendor_Module
```
