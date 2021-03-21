## Onetime Payment

This API allows the merchant application to initialize a onetime payment and get the URL of checkout page.<br/>

### HTTP Request

`POST https://maktapp.credit/v3/AddTransaction`

### Request Parameters

| Parameter   |	Restriction	| Description                                          |
|-------------------|----------------------|----------------------------------------------|
| token     		| Required |String value.<br/>API token. |
| FcmToken          | Optional |String value.<br/>Mobile FCM Token. |
| currencyCode      | Optional |String value.<br/>currency code of your country as an ISO 4217 alpha code.<br/> The default code is QAR. <br/> If you want to support more than one currency, contact with Maktapp support.|
| orderId    		| Required |String value.<br/>A unique identifier for each payment in your application.|
| amount            | Required |Decimal value.<br/>Payment amount that will be deducted from the client account.|
| customerEmail     | Required |String value, it should have valid email address format.<br/>Client email address.|
| customerName      | Optional |String value.<br/>Client Name.|
| customerPhone     | Optional |String value.<br/>Client phone, including country code.|
| customerCountry   | Optional |String value.<br/>Client country.|
| lang  		    | Optional |String value, it should have one of these value{"en","ar"}.<br/>Language of checkout page.<br/>Default value is "ar".|
| note  			| Optional |String value.<br/>Notes about the payment.|

If all the required parameters are valid, Fatora gateway will return URL of checkout page in a JSON format, like following:
### Response JSON 
{ 
	"result": "https://maktapp.credit/pay/MCPaymentPage?paymentID=XXXXXXXXXX" 
}

If the value of one parameter are not valid, Fatora gateway will return a response in JSON format containing error code, like following:
### Response JSON 
{ 
	"result": x 
} [-1, -2, -3, -6, -8, -10, -20, -21 ] 


```php
<? php
init();
function init()
{
 $url = 'https://maktapp.credit/v3/AddTransaction';
 $data = array(
 'token'		   => "E4B73FEE-F492-4607-A38D-852B0EBC91C9",
 'FcmToken'		   => "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
 'currencyCode'    => "XXX", 
 'orderId'		   => "XXXXXXX",
 'amount'		   =>  XXXX, 
 'customerEmail'   => "XXXX",
 'customerName'	   => "XXXX",
 'customerPhone'   => "XXXX",
 'customerCountry' => "XXXX",
 'lang' 		   => "ar",
 'note' 		   => "XXXXX"
);

 $response = curl_post( $url , $data );
 $data_json_decode = json_decode($response);
 $result = $data_json_decode->{'result'};
 
 return  $result;
}

function curl_post($url, array $post = NULL, array $options = array())
{ 
 $defaults = array(
 CURLOPT_POST 			=> 1,
 CURLOPT_HEADER 		=> 0,
 CURLOPT_URL 			=> $url,
 CURLOPT_FRESH_CONNECT  => 1,
 CURLOPT_RETURNTRANSFER => 1,
 CURLOPT_FORBID_REUSE 	=> 1,
 CURLOPT_TIMEOUT 		=> 500,
 CURLOPT_POSTFIELDS 	=> http_build_query($post)
 );
 $ch = curl_init();
 curl_setopt_array($ch, ($options + $defaults));
 if( ! $result = curl_exec($ch))
 {
 trigger_error(curl_error($ch));
 }
 curl_close($ch);
 return $result;
}
?>
```

```csharp
ServicePointManager.ServerCertificateValidationCallback = delegate { return true; };
ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls11 | 
									   SecurityProtocolType.Tls   | SecurityProtocolType.Ssl3;
string apiUrl = "https://maktapp.credit/v3/AddTransaction ";
using (WebClient wc = new WebClient())
{
wc.Headers[HttpRequestHeader.ContentType] = "application/x-www-form-urlencoded";
System.Collections.Specialized.NameValueCollection postData = new System.Collections.Specialized.NameValueCollection();
 {
	{"token", "E4B73FEE-F492-4607-A38D-852B0EBC91C9"}, 
	{"FcmToken", "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"},
	{"currencyCode", "XXX"},
	{"orderId", "XXXXXXX"}, 
	{"amount", XXX},
	{"customerEmail", "XXXX"},   
	{"customerName",  "XXXX"},
	{"customerPhone", "XXXX"},
	{"customerCountry", "XXXX"},  
	{"lang", "ar"},	
	{"note", "testpayment"}
 };
var res 	  = wc.UploadValues(apiUrl, postData);
string resStr = Encoding.UTF8.GetString(res);
}
```

```javascript
<form>
<input type="button" id="SubmitPay" name="SubmitPay" value="submit" onclick="return pay();" />
</form>
<script>
function pay() {    
	var dataToPost = {
	"token": "E4B73FEE-F492-4607-A38D-852B0EBC91C9",
	"FcmToken ":"XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
	"currencyCode": "XXX",
	"orderId": "XXXXXXX",
	"amount": XXXX,      
	"customerEmail": "XXXX",
	"customerName":"XXXX",
	"customerPhone":"XXXX",
	"customerCountry": "XXXX",
	"lang": "ar",
	"note": ""                
	  };
  
	$.ajax({            
	url: 'https://maktapp.credit/v3/AddTransaction',
	data: dataToPost,
	type: "POST",
	dataType: "json",
	success: function (res) {                
			 window.location.href = res.result;                                
		},
	error: function (result) {               
			alert("error: " + result);
		}
	});        
}
</script>

```
```shell
#POST curl https://maktapp.credit/v3/AddTransaction 
-H "Content-Type: application/json" 
-d ' {
"token":"E4B73FEE-F492-4607-A38D-852B0EBC91C9",
"FcmToken":"XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
"currencyCode":"XXX", 
"orderId":"XXXXXXX", 
"amount":XXXX,
"customerEmail":"XXXX",
"customerName":"XXXX",
"customerPhone":"XXXX",
"customerCountry":"XXX",
"lang":"ar", 
"note":"XXXX"
}'

```
> The above request will returns JSON structured if all parameters are valid like this:

``` 
{ 
	"result": "https://maktapp.credit/pay/MCPaymentPage?paymentID=XXXXXXXXXX" 
}
```

> The above request will returns JSON structured if one parameter is null or not valid like this:

```
{ 
	"result": x 
} [-1, -2, -3, -6, -8, -10, -20, -21 ] 


```
After getting the URL of checkout page, the merchant application should redirect his client to this URL, when the client enters his card data in checkout page and clicks on pay button, Fatora gateway will process the payment and redirects the client to success or failure page of the merchant application.

In success case, Fatora gateway will redirect client to merchant success page, and will request the merchant success page with the following parameters:
###Redirect to success page 
`Merchant_SUCCESS_URL?transId=XXX&orderId=XXX&mode=XXX`

In failure case, Fatora gateway will redirect client to merchant failure page, and will request merchant failure page with the following parameters:

###Redirect to failure page
`Merchant_FAILURE_URL?transId=XXX&orderId=XXX&Failerdescription=XXX`

### Request Parameters
| Parameter   |	Description                                          |
|-------------------|------------------------------------------------|
| transId     		| Transaction id of payment created by bank. |
| orderId           | OrderId of payment(sent by your application at requesting Initialize Payment API). |
| mode              | It has one of the following values {"test", "live"} according to merchant mode.|
| Failerdescription | It describes why the payment is failed.|
