

Hyperledger Fabric 2.2 



Contents
Setup on VM	1
Development Setup	2
Cryptogen Tool	12
Implementation	13
OrdererOrgs	14
PeerOrgs	14
Orderer keys/certs	16
Peer keys/certs	17
Addition of components to existing setup	17


Setup on VM
1-	Oracle Virtual machine
2-	Vagrant is a tool for building and managing virtual machine environments in a single workflow. 
a.	CLI tool for managing virtual machines.
b.	Workflow automated by way of Vagrant file. 
```
$> vagrant up 		Starts the VM
$> vagrant ssh 		Log into the VM
$> vagrant halt 		Gracefully shuts down the VM
$> vagrant destroy 	Deletes the VM
```
Visual Studio code:
Setup EOL termination on Windows Change the CRLF to LF from the bottom right corner to avoid Linux through errors.
Install plugins:
•	Vagrant
•	YAML Support by Red Hat
•	Go
•	Docker






Development Setup for Study cases 
Option -1 VM Setup
•	Update the Vagrantfile
Config.vm.box = “acloudfan/hlfdev2.0-0”

$> vagrant up
$> vagrant ssh
$> cd setup
$> sudo  ./init-vexpress.sh

Standard Setup 
config.vm.box = “bento/ubuntu-18.04”
1-	Lunch the virtual Machine using Vagrant:
$> vagrant up
2-	Login to the virtual machine.
$> vagrant ssh
3- installing the required Softwares.

o	Docker
o	Golang
o	Hyperledger Fabric binaries and sample
o	CA server
o	./jq utility 
We will use a bash script for the setup.
Check the bash file permission.
Under the setup folder I create a script for infrastructure setup along with the ‘vagrantfile’ in order to share files between the hos local machine and the virtual machine.
cd /vagrant/setup/
grants all the secripts files below the right execution permission.
$/> chmod 755 *.sh






1-	Install Docker	 

a.	sudo ./docker.sh
b.	validate the setup: exist && vagrant ssh && docker version.
 
docker.sh file 
#!/bin/bash

if [ -z $SUDO_USER ]
then
    echo "===== Script need to be executed with sudo ===="
    echo "Change directory to 'network/setup'"
    echo "Usage: sudo ./docker.sh"
    exit 0
fi

export DOCKER_VERSION=18.03

install_docker() {
    apt-get update
    apt-get install -y apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
    apt-key fingerprint 0EBFCD88
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    apt-get update
    apt-get install -y "docker-ce=${DOCKER_VERSION}.*"
    # apt-get install -y "docker-ce"
    docker info
    # usermod -aG docker vagrant
    echo "======= Adding $SUDO_USER to the docker group ======="
    usermod -aG docker $SUDO_USER
}


# Install docker
install_docker

service docker restart
systemctl daemon-reload
systemctl restart docker

# Installing docker compose
./compose.sh

echo "======= Done. PLEASE LOG OUT & LOG Back In ===="
echo "Then validate by executing    'docker info'   "





2-	Install Golang.	 

a.	Sudo ./go.sh
b.	validate the setup: exist && vagrant ssh && go version.
 
go.sh file
#!/bin/bash
# Updated : Fabric 2.x : April 2021aw

if [ -z $SUDO_USER ]
then
    echo "===== Script need to be executed with sudo ===="
    echo "Change directory to 'setup'"
    echo "Usage: sudo ./caserver.sh"
    exit 0
fi

source ./to_absolute_path.sh

echo "=======Set up go lang======"

# Get the version 1.13 from google
wget https://dl.google.com/go/go1.13.3.linux-amd64.tar.gz
act='ttyout="*"'
tar -xf go1.13.3.linux-amd64.tar.gz --checkpoint --checkpoint-action=$act -C /usr/local 
rm go1.13.3.linux-amd64.tar.gz

# If GOROOT already set then DO Not set it again
if [ -z $GOROOT ]
then
    echo "export GOROOT=/usr/local/go" >> ~/.profile
    echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile

    GOPATH=$PWD/../gopath
    to-absolute-path $GOPATH
    GOPATH=$ABS_PATH

    echo "export GOPATH=$GOPATH" >> ~/.profile
    echo "======== Updated .profile with GOROOT/GOPATH/PATH===="

    echo "export GOROOT=/usr/local/go" >> ~/.bashrc
    echo "export GOPATH=$GOPATH" >> ~/.bashrc
    echo "======== Updated .profile with GOROOT/GOPATH/PATH===="

    # UPDATED Apr 15, 2021
    GOCACHE="$HOME/.go-cache"
    echo "export GOCACHE=$HOME/.go-cache" >> $HOME/.bashrc
    mkdir -p $GOCACHE
    chown -R $USER $GOCACHE

else
    echo "======== No Change made to .profile ====="
fi

echo "======= Done. PLEASE LOG OUT & LOG Back In ===="
echo "Then validate by executing    'go version'"




3-	Install Fabric Binaries and Samples
	 

a.	sudo -E ./fabric-setup.sh  
b.	validate the setup: log off: exist && login vagrant ssh && orderer version.
The setup will be generated three directories: ‘fabric-samples’, ‘gopath’, ’nodechaincode’. The sample project downloaded from GitHub open source: 
https://github.com/hyperledger/fabric-chaincode-go
fabric-setup.sh file 
#!/bin/bash
# Updated : Fabric 2.x : April 2021

if [ -z $SUDO_USER ]; then
    echo "Script MUST be executed with 'sudo -E'"
    echo "Abroting!!!"
    exit 0
fi

if [ -z $GOPATH ]; then
    echo "GOPATH not set!!! You must use 'sudo -E ./install-fabric.sh'"
    echo "Aborting!!!"
    exit 0
fi

SETUP_FOLDER=$PWD

source ./to_absolute_path.sh

export PATH=$PATH:$GOROOT/bin

echo "GOPATH=$GOPATH"
echo "GOROOT=$GOROOT"

mkdir -p $GOPATH

# Execute in the setup folder
echo "=== Must execute in the setup folder ===="

rm -rf ./temp 2> /dev/null
# create temp directory
mkdir temp  &> /dev/null
cd temp

echo "====== Starting to Download Fabric ========"
curl -sSL http://bit.ly/2ysbOFE -o bootstrap.sh
chmod 755 ./bootstrap.sh
# bash ./bootstrap.sh -- 2.0.1 1.4.6 0.4.18
# bash ./bootstrap.sh -- 2.0.1 2.0.0 0.4.18
bash ./bootstrap.sh  2.1.1 1.4.6 0.4.18

echo "======= Copying the binaries to /usr/local/bin===="
cp ./fabric-samples/bin/*    /usr/local/bin
rm -rf ./fabric-samples/bin
cp -r ./fabric-samples ../../

# This downloads the shim package 
echo "======= Setting up the HLF Shim (Takes time - get a coffee :)===="
go get github.com/hyperledger/fabric-chaincode-go/shim
go get github.com/hyperledger/fabric-chaincode-go/shimtest

# Clean up
cd ..
rm -rf  temp

# The sample chaincode is under the subfolder go and need to come under gopath/src subfolder
cd $SETUP_FOLDER

BIN_PATH=$PWD/../bin
to-absolute-path $BIN_PATH
BIN_PATH=$ABS_PATH

echo "export PATH=$PATH:$BIN_PATH:$GOPATH/bin" >> $HOME/.profile
echo "export PATH=$PATH:$BIN_PATH:$GOPATH/bin" >> $HOME/.bashrc

chmod u+x $BIN_PATH/*.sh

# Update /etc/hosts
cd $SETUP_FOLDER

./update_etc_hosts.sh

# Add the peer & orderer binary to the cloud folders
cp /usr/local/bin/orderer   ../cloud/bins/orderer/orderer
cp /usr/local/bin/peer   ../cloud/bins/peer/peer

# Create a folder for managing the packages
mkdir -p $HOME/packages
chown -R $USER $HOME/packages
chmod a+rwx $HOME/packages

echo "Done. Logout and Log back in !!"



4-	Install CA Server	 

a-	vagrant@vagrant:/vagrant/setup$ sudo -E ./caserver-setup.sh

 

b-	Validating the setup by sign-out: ‘exit’ && sign-in ‘vagrant ssh’
i.	fabric-ca-server version
ii.	fabric-ca-client version


caserver-setup.sh file that combines the fabric-ca installation


# echo "NOT Needed"

# exit

export PATH=$PATH:$GOROOT/bin

# Sets up the fabric-ca-server & fabric-ca-client
sudo apt install -y libtool libltdl-dev

# Document process leads to errors as it leads to pulling of master branch
echo "----------------------------------------------"
echo "Please Wait - getting the fabric-ca-server ..."
go get -u github.com/hyperledger/fabric-ca/cmd/...

sudo cp $GOPATH/bin/*    /usr/local/bin

# sudo cp $GOPATH/bin/*    $PWD/../bin

sudo rm $GOPATH/bin/* 

echo "Done. Log out & Log back in ...."





5-	Install JQ utility 	 

Jq is a lightweight and flexible command-line JSON processor.
a-	sudo ./jq.sh
b-	validate: type $ jq
 
Jq.sh file
#!/bin/bash
# Installs the JQ Utility

sudo apt-get install -y jq

 

 Cryptogen Tool
The setup used:
vagrant@vagrant:/vagrant$ cryptogen version
 
Starting with the default template as you can review by running:
vagrant@vagrant:/vagrant$ cryptogen showtemplate
Attached it the pipe output into a .yml file by running:
vagrant@vagrant:/vagrant$ cryptogen showtemplate > temp.yml
 
As I mention in the architecture document: ‘cryptogen generate’ command to generate a sample crypto material folder named as crypto-config same as cryptogen showtemplate commend that include two organization and one Orderer are defined in this configuration. 
```
cryptogen command –flags <args>
cryptogen showtemplate > temp.yml 
cryptogen generate ## generate sample crypto material under crypto-config directory the configuration is used here are the same as the showtemplate command line.
There are two flags with the cryptogen generate:
--config =<<config file path>> ## provide full path to the config file
--output=<<output folder name>> ## Used the provided folder path that will be used to generate the crypto matrial.




Implementation
We will start generating a sample configuration:
vagrant@vagrant:/vagrant$ cryptogen showtemplate > crypto-config.yml
we will use the generate file as an input to the cryptogen:
vagrant@vagrant:/vagrant$ cryptogen generate --config =./crypto-config.yml
error/debug: run the next command if you get: cryptogen: error: open =./crypto-config.yml: no such file or directory, try --help
vagrant@vagrant:/vagrant$ cryptogen generate --config=crypto-config.yml
The success commend will display the two organizations:
 
Moreover, create the folder structure as shown:
 
Will make change to the crypto-config.yml by rename the organizations:
•	Org1.rbs.com
•	Org2.foodlion.com
The purpose of setup the cryptogen is for 
•	learning Hyperledger Fabric.
•	Experiment with different configurations.
•	Development environment.












Put together a diagram including the orgs, Orderer and peer.











 
Crypto-config has two types of organizations:
OrdererOrgs: A list of Organizations managing Orderers.
PeerOrgs: A list of member Organizations managing Peers.
Phace one implementation by add the new crypto-config.yml

 

vagrant@vagrant:/vagrant/cryptogen/simple-two-org$ cryptogen generate --config=crypto-config.yml
There are two ways to define the anchor peer within the organization.
1-	The specification, used to define peer with hostname as listed bellow it has single peer with hostname devpeer.
2-	The template.  By replace the Specs section with Template and use -Count: 2 for defining multiple peers, for the count equal 2, it will be two peers peer0 and peer1.
 
Admin user created by default and you can add more users using the Users section with Count as above will generate user1 and user2.
After combining the configuration and run it will generate the folder structures as below:
 

Orderer keys/certs
orderer needs access to the msp and tls folders.
The access gets enable by using the environment variable ORDERER_GENERAL_LOCALMSPDIR or by using configuration file.
 
The Admin of the organization that get created by default also has the msp and the tls folders as admin will perform:
•	Start/Stop orderer.
•	Config changes.
 








Peer keys/certs
Peer also required the same access to msp and tls within the peer organization and the access gets enable by using the environment variable CORE_PEER_MSPCONFIGPATH or by using configuration file.
The Admin of the organization that get created by default also has the msp and the tls folders as admin will perform:
•	Start/Stop peer.
•	Install/Start chaincode
•	Config changes.
The Users have the same set of folders as (msp and tls) as the user will use the certificate for:
•	Invoke chaincode
•	Query chaincode
 
Addition of components to existing setup
•	Full crypto regeneration = Resetting the component setup.
To avoid resetting the components, the Extend will be used.
•	Extend existing crypto setup using the extend command.
cryptogen extend 
o	--config=CONFIG          The new configuration template to use
o	--input="crypto-config" The input directory in which existing network place
The commend is used to:
1-	Addition of a member (organization)
2-	Addition of orderer
3-	Addition of peer(s) to an organization
4-	Addition of user(s)
Attached is the complete .yml file for configuring three organization:
1-	Orderer
2-	RBS.com
3-	Foodlion.com
 

configtxgen
Configuration artifact types there are three types of artifacts that we can managed.
1-	Genesis Block * The order channel block used by the order binary.
2-	Channel Tx * Create channel transaction when create application channel.
3-	Anchor Peer Tx  Deprecated in Fabric 2.x

Commands
•	Output – Generate the configuration artifacts. Requires the configuration parameters in a file in YAML format.
 
•	Inspection – outputs configuration as JSON because the configuration is not readable and in binary format.
Configtxgen does not have any dependency on Fabric runtime to execute the tool transactions.
The reason: sometimes policies associated with network, the number of signatures is needed to execute a transaction, for example a transaction requires organization A and B to sign, then configtxgen generate the transaction and both organizations Admin’s sign that transaction, then one of the admin submit the signed transaction.
 


```
configtxgen -version
configtxgen -help
```
 

Create environment variable to point to the folder that contain the configtx.yml file:
FABRIC_CFG_PATH

Configtx.yml is 6 sections:
1-	Organizations: List the Member Organizations.
2-	Orderer: Configuration for the Orderer setup.
3-	Application: parameters defining the application configuration.
4-	Channel: channel related parameters.
5-	Capabilities: Binary version management. Across the network.
6-	Profiles: setup multiple configs in a single file.

If the environment variable does not exist. Then the tool searches in the current folder for configtx.yml.
Also, you can overridden the setting by environment variable 
Export CONFIGTX_ORDERER_ORDERERTYPE=kafka
By using the above formula, you can overwrite any properties specified in the configtx.yml.
Configtx Commands:
1-	Genesis Block:  -outputBlock -inspectBlock
2-	Channel Tx: -outputCannelCreatTx -inspectCannelCreatTx 
3-	Anchor Peer Tx: -outputAnchorPeersUpdate , There is no corresponding command to inspect.

1-	Genesis Block: 
a.	Orderer.
b.	Orderer Organizations.
i.	MSP for organizations.
c.	Consortiums.
i.	MSP for organizations.

Inside the CryptoTx folder I will run the cryptogen config yml”
# Uses the configtx in this folder
# 1. Generates the cryptogen using the crypto-config in cryptogen/simple-two-org folder
#    Create the ./crypto folder
# 2. Generates the genesis
# 3. Generates the channel

# 1.
```
cryptogen generate --config=../../cryptogen/simple-two-org/crypto-config.yaml
```
 
# 2. 
NOTE:  The generate binary file is used by lunching the Orderer. – explained in Orderer section. 

export FABRIC_CFG_PATH=$PWD
```
configtxgen  -outputBlock ./acme-genesis.block -profile AcmeOrdererGenesis -channelID ordererchannel
```
 

We can read the generated file by inspect it.
configtxgen  -inspectBlock ./acme-genesis.block 
 
# 3. 
NOTE:  The generate binary Tx File is used by peer for submitting transaction for creation of the application channel. 
It used the configuration info from the configtx.yaml file {Channel Consortium  & Application  configuration}
Profiles:
  AcmeChannel:
    <<: *ChannelDefaults
    Consortium: AirlineConsortium
        
    Application:

```
configtxgen -outputCreateChannelTx ./acme-channel.tx  -profile AcmeChannel -channelID acmechannel
```
 
```
configtxgen -inspectChannelCreateTx acme-channel.tx > temp/acme-channel.json
```

4-	configtxgen -outputAnchorPeersUpdate
5-	configtxgen -printOrg  Org1 > temp/org1.json







Orderer
Required the Genesis Block for initialization and at runtime needs access to orderer.yml and orderer MSP, write to the filesystem.

1-	SOLO: is built into the binary. Cannot be used as cluster, Single instance of the orderer. NOT for production.
2-	Kafka: high performance messaging system. Multiple Orderer instances connect to the Kafka cluster.










3-	RAFT is built into the binary but support clustering.



	Orderer Cluster




Solo and Raft are deprecated for Fabric 2.x
Orderer need access to Genesis Block by the genesis-block file that generated by using the configtxgen generated the acme-genesis.block file. Then write the information about the block in the file system in the Ledger data.
The location of the Ledger data provided to the Orderer by configuration.


 
Orderer Exposes gRPC services to peers & clients. All the messages are carried using Protocol Buffers in use for messaging. 
Message and services are secured by way of support Transport Layer Security (TLS) the security can be enable by the Orderer. It used the crypto service provider the (Encrypt, Decrypt & Message signing…)
  
Orderer write log message to stderr and it controlled be setting the environment variable.
FABRIC_LOGGING_SPEC
Allows module level logging: set the type of logging since it allows:
 {Fatal, Panic, Error, Warning, Info, Debug}
Ex: 
```
export FABRIC_LOGGING_SPEC=INFO
```
To change the default log format setting, it can be done by setting the environment variable as: FABRIC_LOGGING_FORMAT 










Total Topic:
•	Orderer Binary
•	Peer Binary
•	Fabric 2.x Chaincode Lifecycle
•	Peer setup
•	Fabric System Chaincode
•	Network configuration Tools – Configtxlator & jq
•	Resource level Access control list
•	Fabric Certification Authority
•	Multi org setup with root certification authority
•	Fabric setup on cloud
•	Docker Based Fabric Network setup
•	Setting up Fabric on Kubernetes (Google Cloud)
•	Fabric RAFT setup / replace with BDLS protocol.


