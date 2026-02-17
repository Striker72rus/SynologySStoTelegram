# Attention!
This project is not finished yet. It is currently in beta testing. During use, critical errors may occur and will be fixed later.


<a href="https://t.me/+j8Ohh3v0FZ8zNTgy">
  <img src="https://www.svgrepo.com/download/349527/telegram.svg" width="20"> Telegram Group
</a>


# SynologySStoTelegram
Russian version: [README.md](README.md)

Send motion-detection videos from Synology Surveillance Station to Telegram using a webhook.

[![Donate](./assets/donate-donationalerts.svg)](https://boosty.to/striker72rus/donate)

![](https://img.shields.io/github/watchers/Striker72rus/SynologySStoTelegram.svg)
![](https://img.shields.io/github/stars/Striker72rus/SynologySStoTelegram.svg)

![](https://badgen.net/static/API/Telegram)
![](https://badgen.net/static/API/Synology%20Surveillance%20Station)
![](https://badgen.net/static/Made%20with/PHP)

## Contents
- [Attention!](#attention)
- [SynologySStoTelegram](#synologysstotelegram)
  - [Contents](#contents)
  - [Preparation](#preparation)
  - [Installation via docker-compose](#installation-via-docker-compose)
  - [Installation via Container Manager](#installation-via-container-manager)
  - [Project setup](#project-setup)
  - [Telegram setup](#telegram-setup)
  - [Camera setup](#camera-setup)
  - [How to update the project](#how-to-update-the-project)
- [Acknowledgements](#acknowledgements)
- [Support the project](#support-the-project)

<a id="A1"></a>
## Preparation
1) Create a folder in File Station in any convenient location.
2) Right-click it and open its properties.
3) Find the "Location" value and save it somewhere, you will need it later.

![](/images/1.0.png)

For convenience, you can create a separate user with permissions as shown in the screenshot:

![](/images/10.png)

<a id="A2"></a>
## Installation via docker-compose
Config:
```yml
version: '3.8'
services:
    php:
        image: striker72rus/video-ss-to-tg-php:latest
        hostname: php
        restart: unless-stopped
        volumes:
            - '/PATH_TO_DATA:/usr/src/app/data'
        ports:
            - '8888:80'
            - '18081:18081' # Required for real-time dashboard updates.
```

<a id="A3"></a>
## Installation via Container Manager

First, open the project section and click "Create".

![](/images/1.png)
1) Enter the project name.
2) Select where `compose.yaml` will be stored (you can use the path created above for data).
3) Source: create `docker-compose.yml`.
4) Paste the config from [here](#installation-via-docker-compose). Do not forget to replace the path with your own.
5) If everything is correct, click next.

![](/images/2.png)

Click next.

![](/images/3.png)

Check the details and click "Done". Keep "Run this project after creation" enabled.

![](/images/4.png)

Wait until the manager downloads and installs everything.

![](/images/5.png)

When you see this message, the project is installed successfully. You can proceed to [project setup](#project-setup).

<a id="A4"></a>
## Project setup

Open in browser: `http://synology-ip-address:port` you set above (default is `8888`).
Example:
```
http://192.168.1.2:8888
```
If no data was set before, the registration page will open.

![](/images/6.png)

After registration, log in and open the settings page:

![](/images/6.1.png)
![](/images/6.2.png)
![](/images/6.3.png)

- Telegram settings
  * Enter the token received from BotFather.
  * Click Save.
  * Then proceed to <a id="A5">adding chats</a>.

  You can also replace the default notification text.

- Synology settings<br>
  Enter:
  * Synology IP address
  * Web interface port
  * Login
  * Password
  * OTP if enabled. It is better to use a separate user as shown in <a id="A1">Preparation</a> to avoid token expiration and repeated OTP input.

<a id="A5"></a>
## Telegram setup

An automatic mechanism is implemented for adding chats, so the user does not need to manually specify IDs.
1) Add your bot to the required chats/groups.
2) Click the auto-add chat button.
![](/images/14.0.png)
3) The project will start waiting for the `/chat` command. Send this command to all required chats/topics. The bot will detect and add them automatically.
![](/images/14.png)
4) When all chats/topics are added, stop listening by pressing the corresponding button.
5) The bot detects chat/topic names automatically, but if needed you can rename them manually.
6) You can test each chat/topic by clicking Test. The bot will send a test message there.

<a id="A6"></a>
## Camera setup
On first launch, click "Refresh list" to load the list of cameras.

After refresh, cameras will appear in the interface.<br>
Only cameras that record 24/7 or record on event are supported. **Hybrid mode is not supported.**

![](/images/8.png)

From here you can create an event interception rule using the "Create rule" button.<br>
After clicking it, a settings window will appear. You can open it later with the "Configure" button:

![](/images/9.png)

Here you can choose how notifications are sent:
- **Photo + video on request**: as soon as a hook arrives, the system takes a snapshot and sends it with a "request video" button.
- **Full video**: the system waits until the event ends and sends the full video.
- **Video chunks**: the system sends video fragments every N seconds while the event is active.

You can also manage settings via the bot.
Run command:
```
/menu
```

![](/images/13.1.png)

Select the required camera and configure it as needed:

![](/images/13.2.png)

<a id="A7"></a>
## How to update the project

You can update the project directly from the interface.
Open the "Update" tab.
If an update is available, the "Update to latest version" button will appear.

You can also see a short summary of what has changed.

![](/images/11.png)

After starting the update process, the system will download and install everything automatically.
The update may take from 1 to 5 minutes depending on your internet speed.

<a id="B6"></a>
## Acknowledgements
Thanks for the idea to [samoswall](https://github.com/samoswall)

<a id="A8"></a>
## Support the project

[![Donate](./assets/donate-donationalerts.svg)](https://boosty.to/striker72rus/donate)

Please include the project name in your message. Thank you!
