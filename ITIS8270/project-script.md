# project-script

## 1- Install Fabric and Fabric Samples

```
curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.2.2 1.4.9
```

## 2- set the env

```
export PATH=${PWD}/../bin:$PATH
export FABRIC_CFG_PATH=$PWD/../config/

export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051
```

## 3- Bring up the test network

```
./network.sh up createChannel -c mychannel -ca
```

## 4- Deploy the CC
```
chaincode application
```

## 5- Run the application
```
cd ../asset-transfer-basic/application-javascript/
npm install
node app.js
```
