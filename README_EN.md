# Attention!
This project is not finished yet. It is currently in beta testing. During use, critical errors may occur and will be fixed later.


<a href="https://t.me/+j8Ohh3v0FZ8zNTgy">
  <img src="./images/telegram-svgrepo-com.svg" width="20"> Telegram Group
</a>


# SynologySStoTelegram
Russian version: [README.md](README.md)

Notifications from Synology Surveillance Station: motion, faces, license plates, AI events, snapshots and video are delivered to **Telegram**, **VK**, **MAX**, **MQTT**, **Web Push** and external **webhooks**. Works with all Surveillance Station versions, including 9.3+ where Synology removed part of the API: the mode is detected automatically.

## Features
- **Messengers:** Telegram (also via WireGuard / AmneziaWG and VLESS VPN), VK, MAX; separate delivery queues with retries.
- **Other channels:** MQTT (e.g. for Home Assistant), Web Push, external webhooks, external API with Swagger docs.
- **Notification formats:** photo + video on request, full video, video in chunks; customizable message template with preview.
- **Dashboard:** latest motions with previews and video, 24-hour summary (hourly chart, delivery health, noisy cameras), pinned events, "Why didn't it arrive?" trace.
- **Cameras:** online/offline status, "snapshot now", motion rules, notification schedule per camera and Home Mode.
- **Bot control:** `/menu` command, activity digest with a chart.
- **Reliability:** system watchdog (workers, disk, Synology availability), automatic settings backups, "Health" page with storage, event log.
- **Compatibility:** automatic Synology API detection (SS 8.x, 9.x), compatibility diagnostics, 2FA with device token, HTTPS.
- **Security:** login throttling, per-section permissions, CSRF protection for AJAX, settings export/import without secrets.
- **Remote mode:** portal and bridge between several installations.
- **UI:** light/dark theme, Russian and English, mobile layout, installable as a PWA.

[![Donate_tbank](./assets/T-Bank_RU_logo.svg)](https://www.tinkoff.ru/rm/dontsov.sergey22/KEZ6r54259)

[![Donate boosty](./assets/Boosty_logo.svg)](https://boosty.to/striker72rus/donate)

![](https://img.shields.io/github/watchers/Striker72rus/SynologySStoTelegram.svg)
![](https://img.shields.io/github/stars/Striker72rus/SynologySStoTelegram.svg)

![](https://badgen.net/static/API/Telegram)
![](https://badgen.net/static/API/Synology%20Surveillance%20Station)
![](https://badgen.net/static/Made%20with/PHP)

## Installation statistics
![Total installs](https://img.shields.io/github/downloads/Striker72rus/SynologySStoTelegram/total?label=total%20installs&color=blue)
![Latest stable version](https://img.shields.io/github/downloads/Striker72rus/SynologySStoTelegram/latest/total?label=latest%20stable&color=green)
![Latest version including beta](https://img.shields.io/github/downloads-pre/Striker72rus/SynologySStoTelegram/latest/total?label=latest%20version%20(beta)&color=orange)

The counters show how many times the `project.tar.gz` file was downloaded from GitHub releases: every installation and every update of the application. The application sends nothing anywhere, the statistics are kept by GitHub itself.

## Interface
Dashboard: 24-hour summary, activity chart, latest motions (camera frames are blurred in the screenshots).

![](/images/ui-dashboard.png)

Per-camera notification schedule:

![](/images/ui-settings-schedule.png)

Channels: MQTT, external API and webhooks (secrets are hidden in the screenshots):

![](/images/ui-settings-mqtt.png)
![](/images/ui-settings-api.png)

Health center (processes, storage, delivery) and API docs:

![](/images/ui-health.png)
![](/images/ui-api-docs.png)

## Contents
- [Attention!](#attention)
- [SynologySStoTelegram](#synologysstotelegram)
  - [Contents](#contents)
  - [Features](#features)
  - [Installation statistics](#installation-statistics)
  - [Interface](#interface)
  - [Preparation](#preparation)
  - [Installation via docker-compose](#installation-via-docker-compose)
  - [Installation via Container Manager](#installation-via-container-manager)
  - [Project setup](#project-setup)
  - [Telegram setup](#telegram-setup)
  - [Camera setup](#camera-setup)
  - [Logs](#logs)
  - [Users](#users)
  - [Terms of use and data](#terms-of-use-and-data)
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

The menu (Telegram and MAX) provides:
- **Status** of notifications and a 24-hour summary in the header.
- **⏸ Pause** for 1, 3, 8, 24 hours or until morning, and **▶️ Resume**.
- **📊 Summary** for the day, **🩺 Health** (processes and free disk space), **🔁 Retry failed** deliveries.
- In the camera menu: status (online, recording), **📸 Snapshot** now, **🕘 Events** (latest in 24 hours with a video request) and **🔕 Mute** for 1, 8 or 24 hours.
- Video chunk settings appear only when "Video highlights" mode is on.

<a id="A7"></a>
## How to update the project

You can update the project directly from the interface.
Open the "Update" tab.
If an update is available, the "Update to latest version" button will appear.

You can also see a short summary of what has changed.

![](/images/11.png)

After starting the update process, the system will download and install everything automatically.
The update may take from 1 to 5 minutes depending on your internet speed.

<a id="A9"></a>
## Logs
The "More → Logs" section helps you find out what happened in the system.

![](/images/ui-logs.png)

- Log files are grouped by date; each file shows the number of critical errors (CRIT), errors (ERR) and warnings (WARN).
- Inside a file you can search the text and enable "follow the tail" (new lines appear automatically).
- The "Update logs" tab shows the log of the last application update.
- "Download logs for date (ZIP)" builds an archive for the selected day, "Send to support" opens the support Telegram bot so you can send the selected log to the developer.
- Third-party logs (for example, a server report) can be uploaded as a file or a ZIP archive and will appear in the list below.
- Old dates can be deleted with the trash button (with confirmation). Tokens and passwords are never written to logs.

<a id="A10"></a>
## Users
The "Users" section is available to the owner (super admin) only.

![](/images/ui-users.png)

- **Add** — create an account (email and password) and choose the sections it can access: dashboard, settings, cameras, documentation, logs, update, users.
- **Edit** — change permissions and status (active or blocked).
- **Reset password** — set a new password; the user will be asked to change it at login (the "Password change required" column).
- **Delete** — remove the account (with confirmation).
- Login is protected by attempt throttling and CSRF protection.

<a id="A11"></a>
## Terms of use and data
The project is distributed under the MIT license "as is", without warranties. The full terms open inside the application (bottom of the side menu — "Terms of use") and at registration; when the text changes, the application asks the administrator to confirm the new version.

- **Where data is stored.** Only on your own server (local SQLite database and the `data` folder). The project author does not receive your data, there is no telemetry.
- **Where data is sent.** Only where you configured it: your NAS, Telegram, VK, MAX, MQTT, webhooks, Web Push, VPN/proxy. Updates are checked and downloaded from GitHub. A log goes to the support bot only when you press "Send to support".
- **Video surveillance is your responsibility.** Lawfulness of recording, notifying people, retention periods, access and forwarding of video are up to you.
- **Security.** Do not expose the application port to the internet without protection (VPN, HTTPS proxy), use a dedicated Synology user with minimal rights, store backups securely.

<a id="B6"></a>
## Acknowledgements
Thanks for the idea to [samoswall](https://github.com/samoswall)

<a id="A8"></a>
## Support the project

[![Donate_tbank](./assets/T-Bank_RU_logo.svg)](https://www.tinkoff.ru/rm/dontsov.sergey22/KEZ6r54259)

[![Donate boosty](./assets/Boosty_logo.svg)](https://boosty.to/striker72rus/donate)

Please include the project name in your message. Thank you!
