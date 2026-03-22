# 配置VPN

## Windows 

客户端：https://github.com/2dust/v2rayN

节点： https://v2free.net/user

## Linux

客户端 ： https://v2raya.org/

节点同Windows

先下载安装 v2ray-core， 再安装v2rayaGUI

```bash
pushd /tmp
wget https://github.com/v2fly/v2ray-core/releases/latest/download/v2ray-linux-64.zip
unzip v2ray-linux-64.zip -d ./v2ray
mkdir -p /usr/local/share/v2ray && cp ./v2ray/*dat /usr/local/share/v2ray
install -Dm755 ./v2ray/v2ray /usr/local/bin/v2ray
rm -rf ./v2ray v2ray-linux-64.zip
popd
```

https://v2raya.org/docs/prologue/installation/linux/#%E4%B8%8B%E8%BD%BD-v2rayxray-core