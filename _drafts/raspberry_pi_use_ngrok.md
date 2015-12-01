---
layout: post
title:  "在树莓派上使用ngrok"
date:   2015-01-20
categories: Tutorial
---

1. 注册得到密钥
2. 下载 https://ngrok.com/download
3. 传到树莓派上去
scp ./ngrok.zip pi@192.168.1.106:/home/pi/Desktop
4. 解压
`unzip /home/pi/Desktop/ngrok.zip`
得到ngrok可执行文件
5. sudo mv ngrok /usr/bin/
6. ngrok -authtoken aqp+vXQEMt3qANcfuxpQ 8000
得到http://31b19914.ngrok.com -> 127.0.0.1:8000
https://31b19914.ngrok.com -> 127.0.0.1:8000
即可访问
