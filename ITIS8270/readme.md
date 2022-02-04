
# HLF Cheatsheet
```
curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.2.2 1.4.9
cd fabric-samples/test-network
./network.sh up createChannel
```

## re-lounch the network
Delete the folder
```
..\fabric-samples\test-network\organizations\ordererOrganizations
..\fabric-samples\test-network\organizations\peerOrganizations
..\fabric-samples\test-network\channel-artifacts
```

# Deploy CC
1- deploy go cc
```
./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go
```
2- Register the app and init
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"InitLedger","Args":[]}' 
```
3- Invoke chaincode function
```
peer chaincode query -C mychannel -n basic -c '{"Args":["GetAllAssets"]}' 
```

# Start the Network + CA + couchdb
```
 ./network.sh up createChannel -c mychannel -ca -s couchdb
```
# Deploy CC
1- deploy js cc
```
./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-javascript/ -ccl javascript
```
## review the certificate
C:\Users\1426391\Desktop\Desktop\test\fabric-samples\test-network\organizations\peerOrganizations\org1.example.com\msp\cacerts
```
openssl x509 -in ca.org1.example.com-cert.pem -text
```

```
docker exec -it peer0.org1.example.com sh
/opt/gopath/src/github.com/hyperledger/fabric/peer
peer channel list
docker system prune -a --volumes
```
