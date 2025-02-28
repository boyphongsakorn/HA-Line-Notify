# HA Line Notify
Line Notify custom component for Home Assistant.

Line is a messaging application widely used in Asia. It has notification service called "Line Notify" which you can use their API to send messages and media to your Line account. This integration let you send message, image and sticker to your Line account with Home Assistant.

~~This integration is almost based on this [repository](https://github.com/yun-s-oh/Homeassistant/tree/master/custom_components/notify_line) and [this article](https://community.home-assistant.io/t/line-notify-api-integration/56328), but it seems to inactive and contains few errors. I fixed those and add proper documentation.~~

## Installation
 1. Add This Repository to HACS
 2. Go to Settings > Devices & Services
 3. Add Integration > Search Line Notify
 4. Set Name
 5. Call `notify.NameYouSet` (with `service parameters and data` described below) from script or automation as you desire.

 Or
 
 1. Copy `boy_notify_line`folder from `custom_components` to your custom_components in Home Assistant directory.
 2. Add configuration to configuration.yaml
```
notify:
  - name: line_notification
    platform: boy_notify_line
 ```
3. Reboot your Home Assistant instance.
4. Call `notify.line_notification`(with `service parameters and data` described below) from script or automation as you desire.

## Supported parameters
Service data can be added in order to send message, image and sticker.

| Key            | Example value                                                                            | Description                        |
|:---------------|:-----------------------------------------------------------------------------------------|:-----------------------------------|
| `message `     | `Hello`                                                                                  | Message to be sent out to recipient|
| `data `        | `{"url":"https://picsum.photos/600/400","access_token":"xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"}` | data to be send to line            |

## Supported data
Service data can be added in order to send message, image and sticker.

| Key            | Example value                   | Description                   |
|:---------------|:--------------------------------|:------------------------------|
| `access_token` | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxx` | Access Token From Line Notify [Obtain Line Notify personal token.](https://notify-bot.line.me/en/) |
| `url`          | `https://picsum.photos/600/400` | URL of image file             |
| `file`         | `/config/tmp/test.jpg`          | Directory of image file       |
| `stkpkgid`     | `1`                             | Sticker package ID            |
| `stkid`        | `2`                             | Sticker ID                    |
| `notification_disabled`        | `false`                             | if set true doesn't receive a push notification when the message is sent.|

In order to send sticker, `stkpkgid` and `stkid` must be used together. List of sticker package ID and Sticker ID can be found [here](https://developers.line.biz/en/docs/messaging-api/sticker-list/).

JPG and PNG image format are support, but you have to either choose to send with a file or url per round. 


## Example
**Test call with service developer tools**
![test call](https://raw.githubusercontent.com/maxmacstn/HA-Line-Notify/master/sample_show.png)


**Send Camera Snapshot to Line when something was triggered.**

configuration.yaml
```
notify:
  - name: line_notification
    platform: boy_notify_line
    
homeassistant:
  whitelist_external_dirs:
    - /tmp
 ```
Note: You need to add whitelist directory in order to save camera image snapshot. (if you use /media folder, don't set that.)


script.yaml
```
'send_line_notify':
  alias: Send Line notify
  sequence:
 - data:
      filename: /tmp/snapshot.jpg
    entity_id: camera.living_room
    service: camera.snapshot
 - delay: '2'
 - data:
      message: Snapshot from living room camera.
      data:
        file: /tmp/snapshot.jpg
        access_token: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    service: notify.line_notification
```
## To-Do
 - Multi-user support : let each user input their own token upon service call.
 - Simple login: using OAuth to obtain token from Line Notify API.
