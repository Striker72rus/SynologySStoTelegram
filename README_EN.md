# Attention!
This project is not finished yet. It is currently in **Beta testing**.  
During use, critical errors may occur, which will be fixed in future updates.

<a href="https://t.me/+j8Ohh3v0FZ8zNTgy">
  <img src="https://www.svgrepo.com/download/349527/telegram.svg" width="20"> Telegram Group
</a>

# SynologySStoTelegram
Русская версия: [README.md](README.md)

Sending videos triggered by motion detection from **Synology Surveillance Station** to **Telegram** using Webhooks

[![Donate](https://img.shields.io/badge/donate-Tinkoff-yellow.svg)](https://www.tinkoff.ru/rm/dontsov.sergey22/KEZ6r54259)
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
  - [Project configuration](#project-configuration)
  - [Camera configuration](#camera-configuration)
  - [Acknowledgements](#acknowledgements)
  - [Support the project](#support-the-project)

## Preparation
1) Create a folder via **File Station** in any convenient location.  
2) Right-click on it and select **Properties**.  
3) We are interested in **Location** — remember or write it down, it will be needed later.

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
```

<a id="A3"></a>
## Installation via Container Manager

First, open **Container Manager** and click the **Create** button.

![](/images/1.png)

1) Enter the project name.  
2) Choose the path where `compose.yaml` will be stored (you can use the data path created above).  
3) Source — create `docker-compose.yml`.  
4) Paste the configuration from [here](#installation-via-docker-compose) into the input field. Don’t forget to replace the path with your own.  
5) If everything is correct, click **Next**.

![](/images/2.png)

Click **Next**.

![](/images/3.png)

Review the data and click **Done**. Leave the checkbox **Start the project after creation** enabled.

![](/images/4.png)

Wait while the manager downloads and installs everything.

![](/images/5.png)

When you see this message, the project has been successfully installed. You can proceed to the [project configuration](#project-configuration).

<a id="A4"></a>
## Project configuration

Open a browser and go to  
`http://synology-ip:port` specified above (default: 8888)

Example:
```
http://192.168.1.2:8888
```

If there is no existing data, the registration page will open.

![](/images/6.png)

After registration, log in to the application and you will see the settings page:

![](/images/6.1.png)
![](/images/6.2.png)
![](/images/6.3.png)

- **Telegram settings**
  * Enter the token obtained from **BotFather**.
  * Enter the chat ID where notifications will be sent.
  * Click **Save**.
  * Click **Test** — a test message will be sent to the chat. If you receive it, Telegram is configured correctly.

  You can also change the default notification text.

- **Synology settings**  
  You need to enter:
  * Synology IP address
  * Web interface port
  * Login
  * Password
  * OTP, if enabled. However, it is recommended to use a separate user as described in the <a id="A1">Preparation</a> section to avoid token expiration and repeated OTP entry.

<a id="A5"></a>
## Camera configuration
On the first launch, click **Refresh list** to retrieve the list of cameras.

![](/images/7.png)

After refreshing, the cameras will appear in the interface.  
Only cameras that record **24/7** or **event-based recording** are supported.  
**Hybrid recording mode is not supported.**

![](/images/8.png)

From here, you can create an event interception rule using the **Create rule** button.  
After clicking, a configuration window will appear. It can also be opened later via the **Configure** button:

![](/images/9.png)

Here you can configure how notifications are sent:
- **Send image on motion** — as soon as a webhook is received, the system takes a snapshot from the camera and sends it.
- **Send full motion video** — the system waits until motion ends and sends the full video.
- **Send in chunks** — the system sends video fragments in 15-second intervals as long as motion continues.

These options can be combined.

You can also manage settings via the bot.  
Call it with the command:
```
/menu
```

![](/images/13.1.png)

Select the desired camera and configure it as you like:

![](/images/13.2.png)

<a id="A6"></a>
## How to update the project

Updating the project is very easy via the interface.  
Go to the **Update** tab.  
If an update is available, the **Update to latest version** button will appear.

You can also see a brief summary of what has changed.

![](/images/11.png)

After starting the update process, the system will automatically download and install everything.  
The update process may take from **1 to 5 minutes**, depending on your internet speed.

<a id="B6"></a>
## Acknowledgements
Thanks for the idea to [samoswall](https://github.com/samoswall)

<a id="A9"></a>
## Support the project

[![Donate](https://img.shields.io/badge/donate-Tinkoff-yellow.svg)](https://www.donationalerts.com/r/striker_72rus)

Please include the project name in the message. Thank you!
