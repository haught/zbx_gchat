# Zabbix Media Type for Google Chat

This project is a media type for Zabbix 5+ that allows for alerts to be sent without using any server scripts. Getting those scripts to work with their dependencies in a minimal container is a bit of a pain and not nearly as nice as these newer media types.

To install this media type, go to Administration->Media types and import the yaml file.

To configure the media type, set the *zabbix_url* to your global $ZABBIX_URL or manually set it and set the *gchat_endpoint*. Some messages will fail without the *zabbix_url* as Google verifies URLs. You can get your endpoint url by [adding](https://developers.google.com/hangouts/chat/how-tos/webhooks) an incomimg webhook from your Google Chat room.

Next you configure your alerts just like you would any other. It is a bit convoluted. For example, you can:
1. add a new *Google Chat Alert* user with the *Google Chat* media and severity/time set
2. put that user in a new *Google Chat Users* group, give the group permissions to your host groups you want to alert
3. then just add a Configuration->Actions action to *Send message*, to user group *Google Chat Users*, and Send only to *Google Chat*.

### Testing

Testing is a bit of a pain as the media type expects a bunch of variables to be set, oftentimes one set variable may change what is required. When an alert occurs Zabbix fills these out, but we need to do this manually for our test. To just test connectivity, here is a simple method:

1. In Administration->Media Types, click the Test action.
2. You will need to confirm that *zabbix_url* and *gchat_endpoint* are set as described above.
3. Fill in the following values for listed variables: 
  ```event_source = 1
alert_subject = Test alert
alert_message = This is an alert message
host_name = myhost.example.com
host_ip = 10.10.10.10
```
4. Click test and you will receive a test message. It is not formatted well but it will quickly test if your message make it into the Chat room.
