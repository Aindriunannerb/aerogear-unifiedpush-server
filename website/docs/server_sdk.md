---
id: server_sdk
title: Unified Push Server SDK
sidebar_label: Unified Push Server SDK
---

This page documents how to use UPS

## Senders
### JavaSender
 - AEROGEAR-10139	
### Node Sender
 - AEROGEAR-10138

### REST Sender 
- AEROGEAR-10137

The UnifiedPush Server supports a RESTful endpoint found at /rest/send. For connecting to RESTful clients this endpoint consumes and produces json serialised messages that are protected by HTTP Basic authentication, returning a 202 status for message acceptance and a 401 status when there is an authentication error between the server and application.

#### Authentication
The UPS RESTful endpoint receives a HTTP POST request and checks the HTTP Basic header to extract the  pushApplicationId and the masterSecret which make up a token in custom header and are sent as part of the application message for authentication against the push client.

This can be seen in an example curl request 


      curl -u "PushApplicationID:MasterSecret"
        -v -H "Accept: application/json" -H "Content-type: application/json"
        -X POST
        -d '{
          "message": {
           "alert": "HELLO!",
           "sound": "default",
           "user-data": {
               "key": "value",
           }
        }'
        https://SERVER:PORT/CONTEXT/rest/sender
     

#### Data Fortmat 
The UnifiedPush message sent is Json formatted instances of `org.jboss.aerogear.unifiedpush.message`. This message is made up of three different collections of information:

- The Message 

    The format of message is made up of seven further parts 

        - alert
        - sound
        - badge
        - priority
        - consolidationKey
        - user-data
        - simple-push

     ```Java
     public String toString() {
            return "Message{" +
                ", alert='" + alert + '\'' +
                ", sound='" + sound + '\'' +
                ", badge=" + badge +
                ", priority=" + priority +
                ", consolidationKey=" + consolidationKey +
                ", user-data=" + userData +
                ", simple-push='" + simplePush + '\'' +
                '}';
    }
    ```

    
    
   

- The Criteria
    
    This class contains the "query criteria" options for a message sent to the Send-HTTP endpoint

        - aliases
        - deviceTypes
        - categories
        - variants


    ```Java
        public String toString() {
        return "Criteria{" +
                "aliases=" + aliases +
                ", deviceTypes=" + deviceTypes +
                ", categories=" + categories +
                ", variants=" + variants +
                '}';
    }
    ```
   

- The Config 
    
    The config class contains the timeToLive value for the message 

    ```Java
      public String toString() {
        return "Config{" +
                "timeToLive=" + timeToLive +
                '}';
    }
    ```


#### Message Responses

Two different response messages can be expected.

202 message is received on a successful connection between the application and the sender endpoint


A 401 message is receieved on an authorization failure between the application and the sender endpoint, this indicates that the variantId or the masterSecret are incorrect. The expected response header is

`header("WWW-Authenticate", "Basic realm=\"AeroGear UnifiedPush Server\"")`




 
 
 