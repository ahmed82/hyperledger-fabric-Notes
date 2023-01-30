# Compile & full test Fabric on WSL 2 `Finaly success`
## For Fabric developer

* OS Ubuntu 22.04.1 LTS

## Setting up the development environment

Fabric setup for developer

1- compleate the [WSL 2 setup](https://github.com/ahmed82/hyperledger-fabric-Notes/blob/main/wsl/wsl-2.md)
```
sudo apt update

* Install GIT
```
sudo apt-get install git
```
* CURL
```
sudo apt-get install curl
```
* Docker
-- sudo apt-get install docker.io
sudo apt install gnome-terminal
 sudo apt remove docker-desktop
 rm -r $HOME/.docker/desktop
 sudo rm /usr/local/bin/com.docker.cli
 sudo apt purge docker-desktop
 ```
## Install Docker Desktop

```	
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo apt-get install ./docker-desktop-4.16.2-amd64.deb
``` 

 
bugs
------------------------------------
```
getent passwd _apt
sudo chown _apt /var/lib/update-notifier/package-data-downloads/partial/
sudo chown _apt /home/ahmed/docker-desktop-4.16.2-amd64.deb
systemctl --user unmask docker-desktop


sudo systemctl enable docker
sudo usermod -a -G docker ahmed
sudo service docker start
sudo service docker status
```
In Ubuntu 22.04, it might have an issue with the iptables version. When checking the status, it might still show up not running. Hence, please make sure to switch the iptables to legacy.

* To switch to iptables-legacy, run the below command to change the default iptables version.
```
sudo update-alternatives --config iptables
```
* Restart docker service
```
sudo service docker start
```
* Check docker status
```
sudo service docker status
```
------------------------------------------

## Go

# Clean old Go
```
sudo rm -rf /usr/local/go
$ sudo nano /etc/profile        # remove the source code line from $PATH
$ source /etc/profile
```
* Step 1 — Install Go
```
curl -OL https://golang.org/dl/go1.18.10.linux-amd64.tar.gz
or 
wget  https://golang.org/dl/go1.18.10.linux-amd64.tar.gz
```
```
sudo tar -C /usr/local -xvf go1.18.10.linux-amd64.tar.gz
```
* Step 2 — Setting Go Paths

```
sudo nano ~/.profile
```
* Add to the end of the file:
```
export PATH=$PATH:/usr/local/go/bin
```
```
source ~/.profile
```
```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```
# Jq
```
sudo apt install -y jq
jq --version
```
unintasll `sudo apt purge --autoremove -y jq`

# clone Fabric
```
mkdir -p github.com/BDLS-bft
mkdir -p $HOME/go/src/github.com/BDLS-bft
cd $HOME/go/src/github.com/BDLS-bft
git clone https://github.com/BDLS-bft/fabric.git

cd fabric

sudo apt install make -y
sudo apt install build-essential
make gotools
```
After installing the tools, the build environment can be verified by running a few commands.

## Fabric test environment
```
make basic-checks integration-test-prereqs
```
![image](https://user-images.githubusercontent.com/9446035/215386415-1cc33d48-94b0-4ad7-9078-96c5ba16eea9.png)

```
ginkgo -r ./integration/nwo
```

![image](https://user-images.githubusercontent.com/9446035/215386490-7bef2bfb-d1c2-4f47-b5cd-8876380b696c.png)



 


