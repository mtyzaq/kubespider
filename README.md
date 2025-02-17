# Kubespider
<p align="center">
  <img src="./docs/images/logo.png" width="200" alt="Kubespider logo">
</p>

## 💥 A unified download orchestration system
[![GitHub release](https://img.shields.io/github/v/release/jwcesign/kubespider.svg)](https://github.com/jwcesign/kubespider/releases)
[![Total Downloads](https://img.shields.io/docker/pulls/cesign/kubespider.svg)](https://hub.docker.com/repository/docker/cesign/kubespider/general)
[![GitHub stars](https://img.shields.io/github/stars/jwcesign/kubespider?style=plastic)](https://github.com/jwcesign/kubespider/stargazers)
[![GitHub license](https://img.shields.io/github/license/jwcesign/kubespider?style=plastic)](https://github.com/jwcesign/kubespider/blob/master/LICENSE)


English | [简体中文](./README-CN.md)  
I am a cloud computing engineer🤓, focusing on Serverless/FinOps/Runtime, developed Kubespider in my spare time.

The original intention of developing Kubespider is: I have an idle server(in a local area network), and I want to use it as a NAS to realize automatic TV series downloads, trigger downloads on a local laptop, and adapt to various website(YouTube, BiliBili, etc.) and different type resources(TV series, movie, music, etc.)

I was exposed to Terraform before and was surprised by its versatility. Inspired by Terraform, Kubespider aims to build a general download orchestration system, which is compatible with various resource platforms and download software, compatible with multiple download methods(request trigger, cycle trigger, update trigger) and is the most perfect unified solution for resource download.

## ⛵ System architecture
To realize a general download orchestration system, it is necessary to adapt various resource websites and download software, so these two parts are abstracted from the core functions:
![image](./docs/images/kubespider-architecture.png)
* `kubespider-core`: The core module of Kubespider accepts the trigger download request, calls the resource provider for resolution (it will eventually be resolved into a standard mode, such as the user enters the bilibili blogger address, and the corresponding resource provider outputs the mp4 file address download list), and finally calls the download provider to implement Download; this module will also periodically call providers (such as TV series provider) to download additional resources.
* `source-provider`：The provider of each resource website accepts input of general resource addresses, and outputs standard resource address URLs, such as the personal homepage of a YouTuber, and outputs all mp4 download addresses of the blogger’s videos. The configuration file is in `.config/source_provider.cfg`.
* `download-provider`：The provider of each download software receives the request task from `kubespider-core`, and calls the corresponding service to realize the download. The configuration file is in `.config/download_provider.cfg`.

## 💽 Installation
### Premise
1. The operating computer and your server are on the same LAN. (I haven't tried the installation not in the same LAN yet)
2. The server is a Linux system。
3. Docker is installed on the server。

### Default installation(Deploy with Docker)
To adapt to multiple resource platforms, Kubespider provides many adapters. Enable or disable these adapters according to your needs. The configuration file is in the `.config` folder:

#### 1.Download the repository and install
```sh
git clone https://github.com/jwcesign/kubespider.git
cd kubespider
bash hack/install_kubespider.sh
```
Output like:
```sh
root@cesign [09:01:34 PM] [+22.0°C] [~/git/kubespider] [main *]
-> # bash hack/install_kubespider.sh
 _          _                     _     _
| | ___   _| |__   ___  ___ _ __ (_) __| | ___ _ __
| |/ / | | | '_ \ / _ \/ __| '_ \| |/ _` |/ _ \ '__|
|   <| |_| | |_) |  __/\__ \ |_) | | (_| |  __/ |
|_|\_\\__,_|_.__/ \___||___/ .__/|_|\__,_|\___|_|
                           |_|
[INFO] Start to deploy with default configuration...
b13820946878c4f00635e7fa3db64ea83506850ebb7d07ff77b62814db1d894a
9c0aa1059f1546a4f1a2accac445ce1389f1c400b96328f3e18c8af03f0bbc70
[INFO] Deploy successful, check the information:
*******************************************
Kubespider config path: /root/kubespider/.config/
Download file path: /root/kubespider/nas/
Kubespider webhook address: http://<server_ip>:3080
Aria2 server address: http://<server_ip>:6800/jsonrpc, you can use any gui or webui to connect it
Aria2 default secret is:kubespider
*******************************************
```
* This step will install `Kubespide` and `Aria2` resource downloader as the default downloader.
* After installation, **all downloaded files will be stored in `${HOME}/kubespider/nas`**.
* Configuration files are located in `${HOME}/kubespider/.config`.

#### 2.Connect to aria2
For checking the download tasks on your server conveniently, go to the chrome store to download [AriaNg plugin](https://chrome.google.com/webstore/detail/aria2-for-chrome/mpkodccbngfoacfalldjimigbofkhgjn).  
Then connect, the plugin configuration is as follows(shown at the end of the script `install_kubespider.sh` execution output):
![img](./docs/images/aria2-for-chrome-config-en.jpg)

Of course, it is not necessary, you can also use the desktop program to connect: [AriaNg](https://github.com/mayswind/AriaNg-Native/releases/)

#### 3.Install Chrome plugin
With Kubespider Chrome plugin, you can get a better experience.  
After step 1 has been done, open chrome, then install Kubespider plugin, and configure it as follows(The server address is `http://<server_ip>:3080`):
![img](./docs/images/kubespider-chrome-ext-install-en.gif)  

If I want to download a movie, I can right click and send the download task to Kubespider([demo website](https://thepiratebay.org/)):
![img](./docs/images/kubespider-chrome-ext-usage-en.png)

Then with aria2 chrome plugin, you can see the download task starts:
![img](./docs/images/kubespider-chrome-ext-aria2-en.jpg)

#### 4.More (Optional)
* Install Plex, watch videos on multiple platforms, [install now](./docs/zh/user_guide/plex_install_config/README.md).
* Install Baidu network disk(only in china), download in the background, [install now](TODO).
* Install Thunder(only in china), download in the background, [install now](./docs/zh/user_guide/thunder_install_config/README.md).

## 🔧 Feature list
### Basic feature   
* Supports magnet, torrent, and common resource downloads, such as zip, etc.

### Download provider
* Support aria2-type downloaders。

### Source provider
* Based on mikanani, it realizes fully automatic anime chasing, please refer to [link](./docs/zh/user_guide/mikanani_source_provider/README.md) for configuration.
* Download related seed resources of btbtt12, please refer to [link](./docs/zh/user_guide/btbtt12_disposable_source_provider/README.md) for configuration.
* Based on meijutt to automatically follow American TV series, please refer to [link](./docs/zh/user_guide/meijutt_source_provider/README.md) for configuration.
* Based on y2mate, download a YouTube video (TODO).

## 🤝 Contribution [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat)](http://makeapullrequest.com)

If you are interested in participating in joint development, welcome to STAR/FORK/PR, please check [link](./docs/zh/roadmap.md) for Roadmap.
* For how to write a resource provider for a resource website, please see [link](./docs/zh/contribute/implement_source_provider_cn.md).
* How to write a download provider for download software, please see [link](./docs/en/contribute/implement_download_provider_cn.md).
* For How to develop chrome extension webhook, please see [link](./docs/zh/contribute/develop_chrome_extension.md).

## 📝 Disclaimer
All downloading and use of the software are deemed to have carefully read and fully agreed to the following terms:
* The software is only for personal learning and communication, and it is strictly prohibited for commercial and bad purposes.
* If any commercial behavior and bad use are found, the software author has the right to revoke the right to use.
* The risk of using this software will be entirely borne by him, and the software author will not bear any responsibility.
* Except for the terms of service indicated in the software, the author of this software is not responsible for any accidents, negligence, contract damage, defamation, copyright or other intellectual property infringement and any losses caused by improper use of this software, and also no legal responsibility is assumed.
* The author of the software will not be responsible for the interruption of services or other defects caused by force majeure or uncontrollable reasons such as hacker attacks, communication line interruptions, etc., which cause users to fail to use normally but will try their best to reduce losses or impacts caused to users.
* For issues not covered in this statement, please refer to relevant national laws and regulations. When this statement conflicts with relevant national laws and regulations, the national laws and regulations shall prevail.
* The copyright of this software statement and its right to modify, update and final interpretation belong to the software author.