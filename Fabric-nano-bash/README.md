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


### Environment variable
- cd to the `test-network-nano-bash` directory in each terminal window
- In the first orderer terminal, run `./generate_artifacts.sh BFT` to generate crypto material (calls cryptogen) and system and application channel genesis block and configuration transactions (calls configtxgen). The artifacts will be created in the `crypto-config` and `channel-artifacts` directories.

### Orderers/Peers
- In the three orderer terminals, run `./orderer1.sh BFT `, `./orderer2.sh BFT`, `./orderer3.sh BFT`, `./orderer4.sh BFT`  respectively
- In the four peer terminals, run `./peer1.sh`, `./peer2.sh`, `./peer3.sh`, `./peer4.sh` respectively
- Note that each orderer and peer writes their data (including their ledgers) to their own subdirectory under the `data` directory

### Admin peers
- In the four peer admin terminals, run"
`source peer1admin.sh && ./create_channel.sh`, 
`source peer2admin.sh && ./join_channel.sh`, 
`source peer3admin.sh && ./join_channel.sh`, 
`source peer4admin.sh && ./join_channel.sh` respectively

## Join the orderers
```
 ./join_orderers.sh BFT
```
