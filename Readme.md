SMSGH Unity API PHP SDK (Release 2)
===================================

## **Overview**

The Unity API PHP SDK is a wrapper built to assist php-driven applications developers to interact in a more friendly way with the Unity API.
Its goal is also to provide an easy way for those who do not have much knowledge about the whole HTTP Restful technology to interact with the Unity API.
In that direction there is no need to go and grab a knowledge about HTTP and REST technology. 
All one needs is to have the basic knowledge about the PHP language and its Object Oriented pattern. *We mean the basics not advanced knowledge*.


## **Installation**

The SDK can smoothly run on a platform running **PHP 5.3 and above with CURL extension enabled**.
 
To use the SDK all you have to do is to download the **Smsgh** folder from the repository and all of its contents and add it to your project. 
You may then <code>include</code> the Smsgh/Api.php file by referring to the
appropriate path like such: <pre><code>include '/path/to/location/Smsgh/Api.php';</code></pre>


## **Usage**

The SDK currently is organized around four main classes:

* *MessagingApi.php* : 
    It handles sending and receiving messages, NumberPlans, Campaigns, Keywords, Sender IDs and Message Templates management.(For more information about these terms refer to [Our developer site](http://developers.smsgh.com/).)
* *ContactApi.php* : 
        It handles all contacts related tasks. 
* *AccountApi.php* : 
        It handles the API Account Holder data.
* *SupportApi.php* : 
        It helps any developer to interact with our support platform via his application.
* *ContentApi.php* : 
        It handles all content related tasks.

## **Some Quick Start**

* **How to Send a Message**

To send a message just copy this code snippet and do the necessary modifications:
```php
    require './Smsgh/Api.php';

    $hostname = "api.smsgh.com";
    $contextPath = "v3";
    $timeout = -1;
    $port = -1;
    // Here we assume the user is using the combination of his clientId and clientSecret as credentials
    $auth = new BasicAuth("user233", "password23");

    // instance of ApiHost
    $apiHost = new ApiHost($auth, $hostname, $port, $contextPath, $timeout);
    $messagingApi = new MessagingApi($apiHost);
    try {
        // Quick Send approach 
        $messageResponse = $messagingApi->sendQuickMessage("+233245657867", "+233245098456", "<message>");

        if ($messageResponse instanceof MessageResponse) {
            echo $messageResponse->getStatus();
        } elseif ($messageResponse instanceof HttpResponse) {
            echo "\nServer Response Status : " . $messageResponse->getStatus();
        }

        // Default Approach
        $mesg = new Message();
        $mesg->setContent("I will see you soon...");
        $mesg->setTo("+233245098456");
        $mesg->setFrom("+233245657867");
        $mesg->setRegisteredDelivery(true);
    
        $messageResponse = $messagingApi->sendMessage($mesg);
    
        if ($messageResponse instanceof MessageResponse) {
            echo $messageResponse->getStatus();
        } elseif ($messageResponse instanceof HttpResponse) {
            echo "\nServer Response Status : " . $messageResponse->getStatus();
        }
    } catch (Exception $ex) {
        echo $ex->getTraceAsString();
    }
```
* **How to Schedule a Message**

To schedule a message just copy this code snippet and do the necessary modifications.
However please do refer to PHP datetime functions to know how to set the message time it is very crucial.
```php
    require './Smsgh/Api.php';

    $hostname = "api.smsgh.com";
    $contextPath = "v3";
    $timeout = -1;
    $port = -1;
    // Here we assume the user is using the combination of his clientId and clientSecret as credentials
    $auth = new BasicAuth("user233", "password23");

    // instance of ApiHost
    $apiHost = new ApiHost($auth, $hostname, $port, $contextPath, $timeout);
    $messagingApi = new MessagingApi($apiHost);
    try {
        // Default Approach
        $mesg = new Message();
        $mesg->setContent("I will see you soon...");
        $mesg->setTo("+233245098456");
        $mesg->setFrom("+233245657867");
        $mesg->setRegisteredDelivery(true);
        $mesg->setTime(date('Y-m-d H:i:s', strtotime('+1 week'))); // Here we are scheduling the message to be sent next week
        $messageResponse = $messagingApi->sendMessage($mesg);
    
        if ($messageResponse instanceof MessageResponse) {
            echo $messageResponse->getStatus();
        } elseif ($messageResponse instanceof HttpResponse) {
            echo "\nServer Response Status : " . $messageResponse->getStatus();
        }
    } catch (Exception $ex) {
        echo $ex->getTraceAsString();
    }
```
*Please do explore the MessagingApi class for more functionalities.*

* **How to view Account Details**

To send a message just copy this code snippet and do the necessary modifications:
```php
    require './Smsgh/Api.php';

    $hostname = "api.smsgh.com";
    $contextPath = "v3";
    $timeout = -1;
    $port = -1;
    // Here we assume the user is using the combination of his clientId and clientSecret as credentials
    $auth = new BasicAuth("user233", "password23");

    // instance of ApiHost
    $apiHost = new ApiHost($auth, $hostname, $port, $contextPath, $timeout);
    // instance of AccountApi
    $accountApi = new AccountApi($apiHost);
    try {
        // Get the Account Profile
        $profile = $accountApi->getProfile();
        if ($profile instanceof AccountProfile) {
            echo "\n\n" . $profile->getAccountId();
        } else if($profile instanceof HttpResponse){
            echo "\n\n".$profile->getStatus();
        }
    } catch (Exception $ex) {
        echo $ex->getTraceAsString();
    }
```
*Please do explore the AccountApi class for more functionalities.*

The ContactApi, SupportApi and ContentApi classes follow almost the same pattern of functionalities, please do explore them to grab their capabilities.