  ```js
<script>
   require([
       'Magento_Customer/js/customer-data'
   ], function(customerData) {
       // Funkcja do pobierania danych z różnych źródeł
       function getStoreData(key) {
           const value = window.checkout?.[key] || 
                        window?.[key.toUpperCase()] || 
                        window.STORE_CONFIG?.[key] ||
                        document.querySelector(`[data-store-${key}]`)?.getAttribute(`data-store-${key}`);

           return value || 'Cannot get data';
       }

       var storeData = {
           storeId: getStoreData('storeId'),
           websiteId: getStoreData('websiteId'), 
           storeCode: getStoreData('storeCode'),
           baseUrl: window.location.origin + '/',
           currencyCode: getStoreData('currencyCode')
       };

       // Pobierz dodatkowe dane z customerData
       var storeConfig = customerData.get('store-data')();
       if (storeConfig) {
           if (storeConfig.currency_code) {
               storeData.currencyCode = storeConfig.currency_code;
           }
           if (storeConfig.store_code) {
               storeData.storeCode = storeConfig.store_code;
           }
       }

       // Wyświetl informacje o sklepie
       console.log('Store Information:');
       console.log('- Store ID:', storeData.storeId);
       console.log('- Website ID:', storeData.websiteId);
       console.log('- Store Code:', storeData.storeCode);
       console.log('- Base URL:', storeData.baseUrl);
       console.log('- Currency Code:', storeData.currencyCode);

       // Pobierz dane klienta
       var customer = customerData.get('customer');
       customer.subscribe(function(customerInfo) {
           console.log('Customer Data:', customerInfo);
       });
   });
</script>
```
