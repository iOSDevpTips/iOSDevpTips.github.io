---
layout: post
title: "树莓派上变身为iBeacon基站"
category:  Raspberry Pi
---

# 安装
奥睿科 BTA-406-RD USB蓝牙4.0适配器
`sudo apt-get install libdbus-1-dev libdbus-glib-1-dev libglib2.0-dev libical-dev libreadline-dev libudev-dev libusb-dev make`
`mkdir bluez`
`cd bluez`
`wget https://www.kernel.org/pub/linux/bluetooth/bluez-5.27.tar.gz`
`tar xvf bluez-5.27.tar.gz`
`cd bluez-5.27`
`sudo ./configure --disable-systemd`
如果此步make出错，用这种方法重新configure
`sudo LDFLAGS=-lrt ./configure –prefix=/usr –sysconfdir=/etc –localstatedir=/var –enable-library -disable-systemd`
`sudo make`
`sudo make install`
`hciconfig`
```
hci0:   Type: BR/EDR  Bus: USB
    BD Address: 00:1A:7D:DA:71:0B  ACL MTU: 310:10  SCO MTU: 64:8
    DOWN
    RX bytes:419 acl:0 sco:0 events:133 errors:0
    TX bytes:68 acl:0 sco:0 commands:16 errors:0
```
`sudo hciconfig hci0 up`
`hciconfig`
```
hci0:   Type: BR/EDR  Bus: USB
    BD Address: 00:1A:7D:DA:71:0B  ACL MTU: 310:10  SCO MTU: 64:8
    UP RUNNING
    RX bytes:1094 acl:0 sco:0 events:54 errors:0
    TX bytes:768 acl:0 sco:0 commands:54 errors:0
```
扫描：
`sudo hcitool lescan`
测试连接
`sudo hcitool lecc 00:1e:3f:22:4b:a7`
`sudo hcitool -i hci0 cmd 0x08 0x0008 1E 02 01 1A 1A FF 4C 00 02 15 E2 0A 39 F4 73 F5 4B C4 A1 2F 17 D1 AD 07 A9 62 00 00 00 00 C8 00`
UDID: E20A39F4-73F5-4BC4-A12F-17D1AD07A962

# 用Python控制蓝牙
`sudo apt-get install python-bluez`
