## Check Payment Status
```php
<? php
init();
function init()
{
 $url = 'https://maktapp.credit/v3/CheckStatus';
 $data = array(
 'token'		   => 'E4B73FEE-F492-4607-A38D-852B0EBC91C9', 
 'orderId'		   => 'XXXXXXX' 
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
									   SecurityProtocolType.Tls | SecurityProtocolType.Ssl3;
string apiUrl = "https://maktapp.credit/v3/CheckStatus";
using (WebClient wc = new WebClient())
{
wc.Headers[HttpRequestHeader.ContentType] = "application/x-www-form-urlencoded";
System.Collections.Specialized.NameValueCollection postData =new System.Collections.Specialized.NameValueCollection();
 {
	{"token","E4B73FEE-F492-4607-A38D-852B0EBC91C9"},        
	{"orderId","XXXXXXX"}
 };
var res 	  = wc.UploadValues(apiUrl, postData);
string resStr = Encoding.UTF8.GetString(res);

ValidateResponse result= Newtonsoft.Json.JsonConvert.DeserializeObject<ValidateResponse>(resStr);
public class ValidateResponse
{
	publicint result { set; get; }
	public PaymentDetails payment { set; get; }
}
public class PaymentDetails
{
public string  transactionID { set; get; }
public string  customerName { set; get; }
public string  customerPhone{set;get;}
public decimal PaymentAmount { set; get; }
public string  CurrencyCode { set; get; }
public string  CustomerEmail { set; get; }
public string  PaymentDate { set; get; }
public string  Paymentstate { set; get; }
public string  Mode { set; get; }
public string  auth { set; get; }
public decimal ExchangeRate { set; get; }
public decimal PaidAmount { set; get; }
public string  token{set;get;}public decimal description {set;get;}
public bool    refundState{set;get;}
public string  refundstatus{set;get;}        public stringrefundDescription{set;get;}
public string  refundTransactionId{set;get;}
}

```
```javascript

```
```shell
#POST curl https://maktapp.credit/v3/CheckStatus 
-H "Content-Type: application/json"
-d ' { 
"token":"E4B73FEE-F492-4607-A38D-852B0EBC91C9",
"orderId":"XXXXXXX"
}'

```
> The above request will returns JSON structured if Fatora gateway find the payment:

```
{ 
	"result": 1,
    "payment":
	{
		"transactionID": "XXXXXXX",
		"amount": XXXX,
		"currencyCode":  "XXX",
		"customerEmail": "XXXX",
		"customerPhone": "XXXX",
		"customerName":  "XXXX",
		"paymentDate":   "XXXX",
		"paymentstate":  "XXXX" [SUCCESS, PENDING,FAILURE], 
		"auth" : "XXXX",
		"mode": "XXX" [Live, Test] ,
		"ExchangeRate":0,
		"token":null,
		"description":"Transation Successfull",
		"refundState":false,
		"refundstatus":null,
		"refundDescription":null,
		"refundTransactionId":null
	} 
}
```
> Response if Fatora gatway dos not find the payment:

```
{ 
	"result": 0
}
```

> Response in case one of the request parameters is null or not valid:

```
{ 
	"result": X
} [-1, -2, -3, -6, -8, -10, -20, -21 ]

```


This API allows merchant application to get payment details, like status [PENDING, SUCCESS, FAILURE], transactionid, amount, currency code, client detail and other details.<br/>
You can use this API in each time your success page is requested:<br/>

- To ensure that Fatora gateway which issued that request.
- To ignore all the requests after first request, Fatora may request success page page more than once.


### HTTP Request

`POST https://maktapp.credit/v3/CheckStatus`

### Request Parameters

| Parameter |  Restriction	| Description                                          |
|-----------|---------------|------------------------------------------------------|
| token     		| Required |API token.|
| orderId    		| Required |OrderId of payment(sent by your application at requesting initialize Payment API).|
| transactionID     | Optional |Transactionid of payment.|

