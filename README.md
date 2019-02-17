# KnzkLive Publish Server

[![PRs Welcome](https://badgen.net/badge/PRs/welcome/green)](http://makeapullrequest.com)
[![Dependabot Status](https://api.dependabot.com/badges/status?host=github&repo=KnzkDev/knzklive-publish-server)](https://dependabot.com)
[![CircleCI](https://badgen.net/circleci/github/KnzkDev/knzklive-publish-server?icon=circleci)](https://circleci.com/gh/KnzkDev/knzklive-publish-server)
[![eslint: airbnb](https://badgen.net/badge/eslint/airbnb/red?icon=airbnb)](https://github.com/airbnb/javascript)
[![code style: prettier](https://badgen.net/badge/code%20style/prettier/pink)](https://github.com/prettier/prettier)
[![MIT License](https://badgen.net/badge/license/MIT/blue)](LICENSE)

このサーバーでは Node-Media-Server を用いて配信者側からの RTMP 受信 → 視聴者側に flv で送信を行っています。

また、RTMP の認証処理や接続イベントによる人数取得なども行っています。

(認証処理に関しては Node-Media-Server を改造して KnzkLive の API と無理やり噛み合わせています)

## インストールガイド(仮)

割と適当

```
sudo su -
yum install -y gcc-c++ make
curl -sL https://rpm.nodesource.com/setup_8.x | sudo bash -
yum install -y nodejs
yum -y install epel-release
rpm -ihv http://awel.domblogger.net/7/media/x86_64/awel-media-release-7-6.noarch.rpm
yum update -y
yum install -y git ffmpeg ffmpeg-devel
curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
yum install -y yarn

adduser knzklive
useradd knzklive
su - knzklive
git clone https://github.com/KnzkDev/knzklive-publish-server
cd knzklive-publish-server
yarn install
cp config.sample.js config.js
vi config.js
```

`vi /etc/systemd/system/knzklive.service`

```
[Unit]
Description=KnzkLive
After=syslog.target network.target

[Service]
Type=simple
ExecStart=/usr/bin/yarn run start
WorkingDirectory=/home/knzklive/knzklive-publish-server
KillMode=process
Restart=always
User=root
Group=root

[Install]
WantedBy=multi-user.target
```
