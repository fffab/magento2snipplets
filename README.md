# magento2snipplets
Some quickies Magento 2 snipplets


        $message    = $observer->getEvent()->getMethodInstance()->getCode();
        $fileName   = 'payment.log';
        \Alekseon\Logger\Logger::info($message, $fileName);
