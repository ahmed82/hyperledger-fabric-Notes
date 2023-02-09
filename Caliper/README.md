# Caliper
Ref:
* https://hyperledger.github.io/caliper/v0.5.0/installing-caliper/
* https://hyperledger.github.io/caliper/v0.3.2/fabric-tutorial/tutorials-fabric-existing/

This tutorial takes you through performance testing a smart contract on a pre-existing Fabric network using Caliper.

This tutorial follows from the test-network fabric tutorial. You need to have installed and instantiated the fabcar smart contract (part two of the Fabric tutorial).

* Remove your images and start from scratch:
```shell
docker rm -f $(docker ps -aq)
docker rmi -f $(docker images -q)
```

* Remove all unused containers, volumes, networks and images
```shell
docker system prune -a --volumes
```
# create a directory for this environment
https://github.com/hyperledger/caliper-benchmarks/blob/main/networks/fabric/README.md
```
mkdir fabric-benchmarks
cd fabric-benchmarks
```
## Install Fabric and Fabric Samples
```shell
curl -sSLO https://raw.githubusercontent.com/hyperledger/fabric/main/scripts/install-fabric.sh && chmod +x install-fabric.sh
OR use the excutable commend for Fabric 2.2
curl -sSL --insecure https://bit.ly/2ysbOFE | bash -s -- 2.2.0 1.4.7
all above fail use:
curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.4.0 1.5.2
```
* If no arguments are supplied, then the arguments `docker` `binary` `samples` are assumed.
```shell
./install-fabric.sh --fabric-version 2.2
```
# clone caliper-benchmarks 
(ensure it is created inside the directory you created above)
```
git clone https://github.com/hyperledger/caliper-benchmarks
cd caliper-benchmarks
npm install --only=prod @hyperledger/caliper-cli
npx caliper bind --caliper-bind-sut fabric:2.2
```
## Bring up the test network
```cnd
cd fabric-samples/test-network
git checkout -b release-2.2 origin/release-2.2
[Set environment vairiable ](https://github.com/ahmed82/hyperledger-fabric-Notes/blob/main/ITIS8270/set-env4Org1.md)
```
cd fabric-samples/test-network
setx /m CORE_PEER_TLS_ENABLED true
setx /m CORE_PEER_LOCALMSPID Org1MSP
setx /m CORE_PEER_TLS_ROOTCERT_FILE %CD%\organizations\peerOrganizations\org1.example.com\peers\peer0.org1.example.com\tls\ca.crt
setx /m CORE_PEER_MSPCONFIGPATH %CD%\organizations\peerOrganizations\org1.example.com\users\Admin@org1.example.com\msp
setx /m CORE_PEER_ADDRESS localhost:7051
cd..
setx /m FABRIC_CFG_PATH %CD%\config
setx /m FABRIC %CD%\bin
```
## Creating a channel
./network.sh up createChannel
## or  depend or chaincode db support (if it not support level-DB)
./network.sh up createChannel -s couchdb
```
![image](https://user-images.githubusercontent.com/9446035/217735428-9a285822-57b4-4a59-8895-7ecd97e11244.png)


## Starting a chaincode on the channel
```
# DO NOT USE those samples NOT WORKING./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go
./network.sh deployCC -ccn fabcar -ccp ../../caliper-benchmarks/src/fabric/samples/fabcar/node -ccl javascript
```
# Caliper
## Benchmark execution
Ensure you are in the caliper-benchmarks directory
```
npx caliper launch manager --caliper-workspace ./ --caliper-networkconfig networks/fabric/test-network.yaml --caliper-benchconfig benchmarks/samples/fabric/fabcar/config.yaml --caliper-flow-only-test --caliper-fabric-gateway-enabled
```
## Step 1 - Install and Bind Caliper
Globally install caliper CLI using the following terminal command:
```
# npm install -g --only=prod @hyperledger/caliper-cli@0.3.2 # old version
npm uninstall -g @hyperledger/caliper-cli

npm install -g --only=prod @hyperledger/caliper-cli@0.5.0

caliper bind --caliper-bind-sut fabric:2.4 --caliper-bind-args=-g

caliper launch manager \
    --caliper-workspace ~/caliper-benchmarks \
    --caliper-benchconfig benchmarks/scenario/simple/config.yaml \
    --caliper-networkconfig networks/fabric/test-network.yaml
```
If you wish to change the version of binding make sure to unbind your current binding 
(for example if you bound to fabric:2.2 unbind first with `caliper unbind --caliper-bind-sut fabric:2.2`) before binding to the new one.
```
-caliper bind --caliper-bind-sut fabric:1.4.8 --caliper-bind-args=-g-
or
npx caliper bind --caliper-bind-sut fabric:2.4
```


https://hyperledger.github.io/caliper/v0.3.2/fabric-tutorial/tutorials-fabric-existing/
https://hyperledger.github.io/caliper-benchmarks/ **Benchmarks of very old Hyperledger Fabric versions have now been removed.**









