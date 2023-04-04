# ubuntu-22

wget https://go.dev/dl/go1.18.10.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.10.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
source $HOME/.profile
mkdir -p $HOME/go/{bin,src}
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOPATH/bin:/usr/local/go/bin
~/.profile

sudo apt-get update
sudo apt-get upgrade


sudo apt install build-essential
gcc --version
make --version

sudo apt-get install git
sudo apt-get install curl

sudo apt-get -y install docker-compose
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -a -G docker ahmed


sudo apt install make libssl-dev libghc-zlib-dev libcurl4-gnutls-dev libexpat1-dev gettext unzip -y

git config --global user.name "Ahmed Alsalih"
git config --global user.email "a.alsalih2@gmail.com"

mkdir -p github.com/BDLS-bft
cd github.com/BDLS-bft
git clone https://github.com/BDLS-bft/fabric
make gotools

sudo apt-get install python
