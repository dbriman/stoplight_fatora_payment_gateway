#	Introduction
Fatora gateway provides robust features for processing online payments, it processes payments securely and conveniently in real time.<br/>

- Easy integration.
- A flexibly adaptable solution for meeting your needs. 
- High level of data transfer security.


##	Start With Fatora Gateway
If you are new to Fatora gateway, you have to subscribe at the beginning.<br/>
To subscribe, you have to send the following information to Maktapp support.<br/>

- Commercial registration.
- A scan copy of a personal identification.
- Application name.
- Application type (website or mobile application).
- API key of your application in Firebase Cloud Messaging (in case mobile application), Fatora gateway will send push notifications after the end of processing payments.
- URL of success and failure page of your application to redirect clients to these URL after the end of proccessing payments.

After subscription, Maktapp support will send you an **APIKEY**, which is provide to merchant, and later you will use it in calling Fatora API.<br/>

<aside class="notice">
 A sample test **API key "E4B73FEE-F492-4607-A38D-852B0EBC91C9"** is included in all the examples here, so you can test any example using it. <br/>
 To test requests using your account, replace the sample API key with your actual API key.

</aside>

##Steps Of Payment
An online payment goes through the following steps:
###1.	Initialize payment:<br/>

- a client clicks on a pay or subscribe button in the merchant application.
- the merchant application sends a request to Fatora gateway to get the URL of checkout page.
- the merchant application redirects the client to this URL.

###2.	Process payment:<br/>

- the client enters his card data and clicks pay button in checkout page.
- Fatora gateway processes the payment and redirects the client to success page of the merchant application or the failure page according to payment status (SUCCESS or FAILURE).  

###3.	Verify request source:<br/>

- the merchant application receives a request to success or failure page.
- the merchant application should verify source of request (from Fatora gateway or from hacker) by sending request to Fatora gateway to check payment status.<br/>
<br/>
![enter image description here](./images/Paymentstep.jpg)

##Tokenization
Fatora gateway support tokenization to save your clients from having to re-enter card details in checkout page in each time they pay from your application. This will improve clients' user experience when making payments through your application.


**Examples:**

### 1.Autosave payment
When a client makes his first purchase in your application, after processing payment and in success case Fatora gateway will send token (which is Card Token) to your application, your application should save it. 

Therefore, when the client returns to your application to make another purchase, your application should send this token to Fatora gateway to use it in deducting amount directly from the client account **without redirecting him to the checkout page**.



### 2.Recurring payment or subscription
When a client subscribes in a service in your application, after processing subscription and in success case, Fatora gateway will send token to your application.

Therefore, in each time a payment is due, your application will send this token to Fatora gateway to use it in deducting amount from the client account automatically.

