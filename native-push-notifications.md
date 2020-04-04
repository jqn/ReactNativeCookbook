# Native Push Notifications

Get client id

curl -H "Content-Type: application/json" -X POST -d '{"app\_key":"your app key", "platform\_type": "apns", "token": "devicetoken"}' [https://nativepushclient-cluster1.pusher.com/client\_api/v1/clients](https://nativepushclient-cluster1.pusher.com/client_api/v1/clients)

Subscribe to client

curl -H "Content-Type: application/json" -X POST -d '{"app\_key":"your app key", "platform\_type": "apns", "token": "devicetoken"}' [https://nativepushclient-cluster1.pusher.com/client\_api/v1/clients/:clientidfromprevious/interests/jqn](https://nativepushclient-cluster1.pusher.com/client_api/v1/clients/:clientidfromprevious/interests/jqn)

