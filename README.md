In app billing documentation
===================================

Installation 
-------------
* Get acquainted with the Android [In-app Billing documentation](http://developer.android.com/google/play/billing/index.html).
* Add in your src folder the *com* folder  
It contains:
    * [Google Play In-app Billing library]( http://developer.android.com/guide/google/play/billing/billing_overview.html)
	* Cordova InAppBillingPlugin
* Add inappbilling.js in your www folder 
* Add in your index.html
`<script type="text/javascript" charset="utf-8" src="inappbilling.js"></script>`
* In res/xml/config.xml, add  

```xml  
<feature name="InAppBillingPlugin">   
      <param name="android-package" value="com.smartmobilesoftware.inappbilling.InAppBillingPlugin"/>  
</feature>  
```
* Open the AndroidManifest.xml of your application
	* add this permission  
`<uses-permission android:name="com.android.vending.BILLING" />`
* Create a release apk of your app and sign it. 
* Create a new application in the Developer Console. 
* Upload your apk 
* Enter the app description, logo, etc. then click on save
* Add in-app purchases items from the Developer Console (activate them but do not publish the app)
* Click on Services and APIs to get your public license key
* In com.smartmobilesoftware.inappbilling open InAppBillingPlugin.java
	* Add your public key (base64EncodedPublicKey)
* Wait 6-8 hours
* Install the signed app on your test device in release mode. The Google Account on the test device should not be the same as the developer account).
* Read carefully the Google testing guide to learn how to test your app : http://developer.android.com/guide/google/play/billing/billing_testing.html

Usage
-------
#### Initialization
Initialize the billing plugin. The plugin must be inialized before calling any other methods. 

    inappbilling.init(success, error)
parameters
* success : The success callback.
* error : The error callback.

#### Retrieve owned products
The list of owned products are retrieved from the local database.

	inappbilling.getPurchases(success, fail)
parameters
* success : The success callback. It provides a json array of the list of owned products as a parameter. Example :  
[  
	"sword_001",  
	"saber_001" 
]    

* error : The error callback.

#### Purchase
Purchase an item. You cannot buy an item that you already own.

    inappbilling.buy(success, fail, productId)
parameters
* success : The success callback. It provides the productId of the purchased item as a parameter. Example :
sword_001
* error : The error callback.
* productId : The in app billing porduct id (example "sword_001")

#### Subscribe
Subscribe to an item

	inappbilling.subscribe(success, fail, subcriptionId)
parameters
* success : The success callback.
* error : The error callback.
* productId : The in app billing porduct id (example "premium_001")

#### Consume
Consume an item. You can consume an item that you own. Example of consumable items : food, additional life pack, etc. Example of non-consumable item: levels pack. Once an item is consumed, it is not owned anymore.

    inappbilling.consumePurchase(success, fail, productId)
parameters
* success : The success callback. It provides a json object with the transaction details. Example :  
{
	"orderId":"12999763169054705758.1321583410745163",
	"packageName":"com.smartmobilesoftware.trivialdrivecordova",
	"productId":"gas",
	"purchaseTime":1369402680000,
	"purchaseState":0,
	"purchaseToken":"ccroltzduesqaxtuuopnqcsc.AO-J1Oyao-HWamJo_6a4OQSlhflhOjQgYWbb-99VF2gcj_CB1dd1Sfp5d-olgouTWJ13Q6vc5zbl0SFfpofmpyuyeEmJ"
}
* error : The error callback.
* productId : The in app billing porduct id (example "5_lifes")

Quick example
---------------
```javascript
inappbilling.init(successInit,errorCallback)

function successInit(result) {    
	// display the extracted text   
	alert(result); 
	// make the purchase
	inappbilling.buy(successPurchase, errorCallback,"gas");
	
}    
function errorCallback(error) {
   alert(error); 
} 

function successPurchase(productId) {
   alert("Your item has been purchased!");
} 
```

Full example
----------------
```html
<!DOCTYPE HTML>
<html>
	<head>
		<title>In App Billing</title>
		<script type="text/javascript" charset="utf-8" src="cordova-2.7.0.js"></script>
		<script type="text/javascript" charset="utf-8" src="inappbilling.js"></script>
		<script type="text/javascript" charset="utf-8">
			function successHandler (result) {
                var strResult = "";
                if(typeof result === 'object') {
                    strResult = JSON.stringify(result);
                } else {
                    strResult = result;
                }
                alert("SUCCESS: \r\n"+strResult );
            }
			
			function errorHandler (error) {
			    alert("ERROR: \r\n"+error );
			}

            // Click on init button
			function init(){
				// Initialize the billing plugin
				inappbilling.init(successHandler, errorHandler); 
			}

			// Click on purchase button
			function buy(){
				// make the purchase
				inappbilling.buy(successHandler, errorHandler,"gas");
				
			}
			
			// Click on ownedProducts button
			function ownedProducts(){
				// Initialize the billing plugin
				inappbilling.getPurchases(successHandler, errorHandler);
				
			}

            // Click on Consume purchase button
            function consumePurchase(){

                inappbilling.consumePurchase(successHandler, errorHandler, "gas");
            }

            // Click on subscribe button
            function subscribe(){
                // make the purchase
                inappbilling.subscribe(successHandler, errorHandler,"infinite_gas");

            }

        </script>
		
	</head>
	<body>
		<h1>Hello World</h1>
		<button onclick="init();">Initalize the billing plugin</button>
		<button onclick="buy();">Purchase</button>
        <button onclick="ownedProducts();">Owned products</button>
        <button onclick="consumePurchase();">Consume purchase</button>
        <button onclick="subscribe();">Subscribe</button>


    </body>
</html>
```

Common issues
----------------
If you have an issue, make sure that you can answer to theses questions:  
Did you create your item in the Developer Console?  
Is the id for your item the same in the Developer Console and in your app?  
Is your item active?  
Have you waited at least a few hours since you activated your item and uploaded your apk on the Developer Console?  
Are you using a different Google account than your developer account to make the purchase?  
Are you testing on a real device, rather than the emulator?  
Are you using a signed apk?  
Is the version code of your app the same as the one uploaded on the Developer Console?  
  
If any of these questions is answered with a "no", you probably need to fix that.  
