# Install aapanel Ubuntu22.04

```
# update ubuntu
sudo apt-get update

# install curl jika belum ada
sudo apt install curl

#install aapanel
sudo -s
URL=https://www.aapanel.com/script/install_7.0_en.sh && if [ -f /usr/bin/curl ];then curl -ksSO "$URL" ;else wget --no-check-certificate -O install_7.0_en.sh "$URL";fi;bash install_7.0_en.sh ipssl
```
