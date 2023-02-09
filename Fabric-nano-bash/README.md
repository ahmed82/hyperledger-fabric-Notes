# Test network - Nano bash

## Prep
* [Install yq](https://github.com/mikefarah/yq/#install)

```shell
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys CC86BB64
sudo add-apt-repository ppa:rmescandon/yq

sudo apt update
sudo apt install yq -y
```

```shell
cd ~/go/src/github.com/hyperledger/
# clone https://github.com/BDLS-bft/fabric-samples.git // do not clone it just run the ./install-fabric.sh script to avoid compile issue.
```
## downloading the Fabric samples and binaries. 
it can be manuly configured the binary as [the info link](https://stackoverflow.com/questions/55832896/how-do-i-install-hyperledger-fabrics-binaries-only) not tested 
```shell
curl -sSLO https://raw.githubusercontent.com/hyperledger/fabric/main/scripts/install-fabric.sh && chmod +x install-fabric.sh
or
wget https://raw.githubusercontent.com/hyperledger/fabric/main/scripts/install-fabric.sh && chmod +x install-fabric.sh
./install-fabric.sh binary samples

```
## To run the chaincode as a service
You need to configure the peer to use the ccaas external builder downloaded with the binaries above. The path specified in the default config file is only valid within the peer container which you won't be using. Edit the fabric-samples/config/core.yaml file and modify the externalBuilders field to point to the correct path. The configuration should look something like the following:

run the following command in the `fabric-samples directory to update the configuration:
```shell
yq -i 'del(.chaincode.externalBuilders) | .chaincode.externalBuilders[0].name = "ccaas_builder" | .chaincode.externalBuilders[0].path = env(PWD) + "/builders/ccaas" | .chaincode.externalBuilders[0].propagateEnvironment[0] = "CHAINCODE_AS_A_SERVICE_BUILDER_CONFIG"' config/core.yaml
```
or Manuly make change.
From:
```yaml
    externalBuilders:
       - name: ccaas_builder
         path: /opt/hyperledger/ccaas_builder
         propagateEnvironment:
           - CHAINCODE_AS_A_SERVICE_BUILDER_CONFIG
```
To:
```yaml
externalBuilders:
  - name: ccaas_builder
    path: /home/ahmed/go/src/github.com/BDLS-bft/fabric-samples/builders/ccaas
    propagateEnvironment:
      - CHAINCODE_AS_A_SERVICE_BUILDER_CONFIG
```
![Terminal setup](https://github.com/BDLS-bft/fabric-samples/blob/main/test-network-nano-bash/terminal_setup.png)
# Instructions for starting network 
<!--cd to the `test-network-nano-bash` directory in each terminal window -->
`time=Time.new() = 3:40 AM `


//TODO 

