# Install Fabric Certificate Authority (FabCA)

## Setting Up Hyperledger Fabric Certificate Authority 
* Install Fabric CA
```shell
apt-get update
apt install libtool libltdl-dev make
mkdir ~/go && cd ~/go && mkdir src && cd src && mkdir github.com && cd github.com && mkdir hyperledger && cd hyperledger
git clone https://github.com/hyperledger/fabric-ca
cd fabric-ca/

GO_TAGS=pkcs11 make fabric-ca-server
make fabric-ca-client
```
## Environment variable:
```shell
vim ~/.profile 
# Sdd the two line at the end of the file and save the file
export SOFTHSM2_CONF=/etc/softhsm2.conf
export PATH=$PATH:~/go/src/github.com/hyperledger/fabric-ca/bin
```
```shell
source  ~/.profile 
# Testing the CA binary
fabric-ca-server
```
## prep:
```shell
mkdir /usr/local/bin/fabricwork
mkdir  /usr/local/bin/fabricwork/server
mkdir /usr/local/bin/fabricwork/client
mkdir /var/log/fabricserver
# This file for log 
touch /var/log/fabricserver/fabca.log
```
## Create the systemd Service File:
```shell
cat > fabric-ca-server.service << EOF

[Unit]

Description=Hyperledger Fabric ca server service

After=network.target

[Service]

Type=simple

User=root

Group=root

WorkingDirectory=/usr/local/bin/

ExecStart=/bin/sh -c "/root/go/src/github.com/hyperledger/fabric-ca/bin/fabric-ca-server start -b admin:adminpw "angled bracket""angled bracket" /var/log/fabricserver/fabca.log 2"angled bracket"&1"

Restart=on-failure

[Install]

WantedBy=multi-user.target ~

EOF
```
```shell
mv fabric-ca-server.service /etc/systemd/system/
systemctl enable fabric-ca-server.service  
systemctl start fabric-ca-server.service
systemctl status fabric-ca-server.service
```
## Stream / Exam the log file
```shell
# Stream live log activity by monitor the log file changes
tail -f /var/log/fabricserver/fabca.log
# Check the full log
cat /var/log/fabricserver/fabca.log
```
