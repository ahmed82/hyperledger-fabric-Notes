
/98//28659**9814o apt-get install curl
sudo apt-get install golang
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
To verify the Path
vim ~/.bashrc

install Node.js, npm, 

sudo apt-get install nodejs
sudo apt-get install npm
sudo apt-get install python
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
  
sudo apt-get install docker-ce docker-ce-cli containerd.io
  
sudo docker run hello-world

sudo apt-get install docker-compose  

apt-cache policy docker-ce

----------------------------------
install Fabric latest version
curl -sSL https://bit.ly/2ysbOFE | bash -s


fix docker issue:
1- sudo groupadd docker
2- sudo gpasswd -a $USER docker
sudo chown root:docker /var/run/docker.sock
  sudo chown -R root:docker /var/run/docker
  sudo systemctl enable docker
  newgrp docker
  
--------------------------------------------------------------
vim ~/.bashrc
export PATH=$PATH:/home/ahmed/Desktop/Fabric/fabric-samples/bin
source ~/.bashrc

-------------------------------
test network
  
  
  
  
  
