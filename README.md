# udocker-uberspace
This manual shows the use of custom-created docker containers on uberspace.
The provided example runs a Qt5-based web service.

## Installation
```
mkdir -p ~/.udocker
wget https://raw.githubusercontent.com/indigo-dc/udocker/019e4d663d226aaa0caa3b173da6d1ce8264a0ab/udocker.py -O ~/.udocker/udocker.py
chmod u+x ~/.udocker/udocker.py
ln -sf ~/.udocker/udocker.py ~/bin/udocker
```

## Install Ubuntu 18:04 with Qt5
```
udocker pull ubuntu:bionic
udocker create --name=Ubuntu_Qt5 ubuntu:bionic

cat << EOF > ~/install.sh
#!/bin/bash
apt-get update && apt-get -y upgrade
apt-mark hold dbus # this creates new groups which is not possible in udocker
apt-get install -y make g++ qt5-default
EOF
chmod a+x ~/install.sh

udocker run --user=root -v ~/install.sh -i -t Ubuntu_Qt5 ~/install.sh
```

## Setup Qt5 WebApp
```
wget http://stefanfrings.de/qtwebapp/QtWebApp.zip -O ~/QtWebApp.zip
cd ~ && unzip ./QtWebApp.zip
```

### Find free local port
```
WWW_PORT=$(( $RANDOM % 4535 + 61000)); netstat -tulpen | grep $HOODIE_WWW_PORT && echo "try again"
sed -i 's/^port=8080/port='$WWW_PORT'/ ~/QtWebApp/Demo1/etc/Demo1.ini
cat << EOF > ~/html/.htaccess
RewriteEngine On
RewriteRule (.*) http://localhost:$WWW_PORT/$1 [P]
EOF
```

### Compile and run TODO
```
udocker run --user=$USER --hostauth --hostenv --bindhome -i -t Ubuntu_Qt5 bash
```
