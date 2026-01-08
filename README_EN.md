# Warning!
This project is still unfinished and currently in beta testing. Critical errors may occur during use and will be fixed later.

# SynologySStoTelegram
Send motion-triggered videos from Synology Surveillance Station to Telegram using Webhooks

[![Donate](https://img.shields.io/badge/donate-Tinkoff-yellow.svg)](https://www.tinkoff.ru/rm/dontsov.sergey22/KEZ6r54259)
![](https://img.shields.io/github/watchers/Striker72rus/SynologySStoTelegram.svg)
![](https://img.shields.io/github/stars/Striker72rus/SynologySStoTelegram.svg)

![](https://badgen.net/static/API/Telegram)
![](https://badgen.net/static/API/Synology%20Surveillance%20Station)
![](https://badgen.net/static/Made%20with/PHP)

## Table of Contents
- [Warning!](#warning)
- [SynologySStoTelegram](#synologysstotelegram)
  - [Table of Contents](#table-of-contents)
  - [Preparation](#preparation)
  - [Installation via docker-compose](#installation-via-docker-compose)
  - [Installation via Container Manager](#installation-via-container-manager)
  - [Project configuration](#project-configuration)
  - [Camera configuration](#camera-configuration)
  - [Updating the project](#updating-the-project)
  - [Credits](#credits)
  - [TODO](#todo)
  - [Support the project](#support-the-project)

## Preparation
1. Create a folder via File Station anywhere you like.
2. Right-click the folder and choose **Properties**.
3. Note the value in **Location**—you will need it later.

![](/images/1.0.png)

For convenience you can create a dedicated user with the permissions shown below:

![](/images/10.png)

<a id="A2"></a>
## Installation via docker-compose
Configuration example:
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

Open the project page and click **Create**.

![](/images/1.png)
1. Enter the project name.
2. Select the path where `compose.yaml` will be stored (you can reuse the path created earlier for the data).
3. Source – choose **Create docker-compose.yml**.
4. Paste the configuration from the [docker-compose installation section](#installation-via-docker-compose) and replace the path with your own.
5. Once everything looks correct, click **Next**.

![](/images/2.png)

Click **Next** again.

![](/images/3.png)

Review the data and click **Done**. Leave the **Start project after creation** option enabled.

![](/images/4.png)

Wait for the manager to download and install all components.

![](/images/5.png)

When you see this message, the project was installed successfully and you can move on to the [configuration](#project-configuration).

<a id="A4"></a>
## Project configuration

Open a browser and go to `http://<synology-ip>:<port>` (default port is `8888`).
Example:
```
http://192.168.1.2:8888
```

A registration page will open if no data exists yet.

![](/images/6.png)

After registering, sign in to the application to see the settings page:

![](/images/6.1.png)

- **Telegram settings**
  * Enter the token from BotFather.
  * Enter the chat ID where notifications should be sent.
  * Click **Save**.
  * Click **Test**. A test message should arrive in the chat—if it does, Telegram is configured correctly.

- **Synology settings**<br>
  Provide the following:
  * Synology IP address
  * Web interface port
  * Login
  * Password
  * OTP (if applicable). Creating a dedicated user, as described in the [preparation](#preparation) section, is recommended.

<a id="A5"></a>
## Camera configuration
At first launch click **Refresh list** to retrieve the list of cameras.

![](/images/7.png)

After refreshing, they will appear in the interface.<br>
Only cameras recording 24/7 or on event are supported. **Hybrid recording is not supported.**

![](/images/8.png)

From here you can create a rule that intercepts events by clicking **Create rule**.<br>
You can reopen the configuration window later with the **Configure** button.

![](/images/9.png)

Configure how notifications are delivered:
- **Send snapshot on motion** – once a webhook arrives, the system takes a picture from the camera and sends it.
- **Send full motion video** – the system waits for the motion to end and sends the full video.
- **Send in chunks** – the system sends 15-second video fragments for as long as motion is detected.

These options can be combined.

<a id="A6"></a>
## Updating the project

Updates can be installed directly from the interface.
Go to the **Update** tab.
If an update is available, you will see the **Update to the latest version** button.

A short changelog is also displayed here.

![](/images/11.png)

After starting the update process, the system will download and install everything automatically.
Depending on your internet speed, the update may take from 1 to 5 minutes.

<a id="B6"></a>
## Credits
Thanks to [samoswall](https://github.com/samoswall) for the idea.

<a id="B7"></a>
## TODO
- [x] Make video fragment duration configurable.

<a id="A9"></a>
## Support the project

[![Donate](https://img.shields.io/badge/donate-Tinkoff-yellow.svg)](https://www.tinkoff.ru/rm/dontsov.sergey22/KEZ6r54259)

Please mention the project name in the message. Thank you!