# ubuntu-22
```shell
# GOLANG
## remove old GO version
sudo apt-get purge golang*
sudo rm -rf /usr/local/go 

## download
wget https://go.dev/dl/go1.18.10.linux-amd64.tar.gz

sudo tar -C /usr/local -xzf go1.18.10.linux-amd64.tar.gz

# export PATH=$PATH:/usr/local/go/bin
mkdir -p $HOME/go/{bin,src}
echo 'export GOROOT=/usr/local/go' >> .profile
echo 'export GOPATH=$HOME/go' >> .profile
echo 'export PATH=$GOPATH/bin:$GOROOT/bin:$PATH' >> .profile
source ~/.profile  #source $HOME/.profile
go version
go env

sudo apt-get update
sudo apt-get upgrade
sudo ubuntu-drivers autoinstall # Optional for my extention HDMI 2nd screen

# build-essential
sudo apt install build-essential && gcc --version && make --version

# Git
sudo apt-get install git
git config --global user.name "Ahmed Alsalih"
git config --global user.email "a.alsalih2@gmail.com"

# CURL
sudo apt-get install curl

# docker
sudo apt-get -y install docker-compose
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -a -G docker ahmed

# Optional
sudo apt install make libssl-dev libghc-zlib-dev libcurl4-gnutls-dev libexpat1-dev gettext unzip -y

# Fabric
mkdir -p github.com/BDLS-bft
cd github.com/BDLS-bft
git clone https://github.com/BDLS-bft/fabric
make gotools

# python
sudo apt-get install python
```
