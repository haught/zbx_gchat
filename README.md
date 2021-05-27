# Zabbix Media Type for Google Chat
<img width="450" alt="Screen Shot 2021-05-27 at 12 36 10 PM-2" src="https://user-images.githubusercontent.com/1930408/119869513-53104080-beee-11eb-9f1e-eaf02986e881.png">


This project is a media type for Zabbix 5+ that allows for alerts to be sent without using any server scripts. Getting those scripts to work with their dependencies in a minimal container is a bit of a pain and not nearly as nice as these newer media types.

To install this media type, go to Administration->Media types and import the yaml file.

To configure the media type, set the *zabbix_url* to your global $ZABBIX_URL or manually set it. Some messages will fail without the *zabbix_url* as Google verifies URLs. Also a Google Chat *gchat_endpoint* will be needed. Just create a room and [add a webhook](https://developers.google.com/hangouts/chat/how-tos/webhooks).

Once you have those items ready, you are ready to configure 3 things - User media, action, and the endpoint macro. 

First, a Media entry needs to be added to a user that has permissions to *read* your hosts.
<img width="564" alt="User Media" src="https://user-images.githubusercontent.com/1930408/119862228-4ab40780-bee6-11eb-9b6a-130f914f70bf.png">

Second, an action will need to be created that will send only to Google Chat for your user or group.
<img width="659" alt="Action" src="https://user-images.githubusercontent.com/1930408/119862957-1260f900-bee7-11eb-84f5-476967379560.png">

Third and finally, a macro for the Google Chat room url will be needed to be added. I have found adding it to a template - either its own or one that is always included for you hosts works well especially if you have many different groups of users using Zabbix.
<img width="834" alt="Template Macro" src="https://user-images.githubusercontent.com/1930408/119863520-a59a2e80-bee7-11eb-850c-eab11803c46f.png">

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
