
```
sudo apt-get install curl
sudo apt-get install golang
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```
To verify the Path
```
vim ~/.bashrc
```

install Node.js, npm, 
```
sudo apt-get install nodejs
sudo apt-get install npm
sudo apt-get install python
```
--------------------------------------


removing old Docker setup;
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```   

## Downladthe latest Fabric network
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
  
sudo docker run hello-world

sudo apt-get install docker-compose  

apt-cache policy docker-ce
```
----------------------------------
install Fabric latest version
```
curl -sSL https://bit.ly/2ysbOFE | bash -s
```

fix docker issue:

1- sudo groupadd docker
2- sudo gpasswd -a $USER docker
sudo chown root:docker /var/run/docker.sock
  sudo chown -R root:docker /var/run/docker
  sudo systemctl enable docker
  newgrp docker
  
--------------------------------------------------------------
```
vim ~/.bashrc
export PATH=$PATH:/home/ahmed/Desktop/Fabric/fabric-samples/bin
source ~/.bashrc
```
-------------------------------
test network
```
cd fabric-samples/test-network
```

```
./network.sh up
./network.sh down
```
to print the script help text
```
/network.sh -h

```
-----------------------------------------------
## Create Channal

```
cd fabric-samples/test-network
/network.sh up createChannel
```
## 
```
./network.sh up createChannel -ca -s couchdb -c unccc-hannel
```
---------

## Deploy Smart Contract 
1st export the working directory to PATH
```
export PATH=${PWD}/../bin:$PATH
export FABRIC_CFG_PATH=$PWD/../config/
```

```
peer lifecycle chaincode package basic.tar.gz --path ../asset-transfer-basic/chaincode-javascript/ --lang node --label basic_1.0
```



