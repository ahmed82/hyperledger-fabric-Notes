
# Install Hyperledger Fabric
```shel
curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.2.2 1.4.9
cd fabric-samples/test-network
./network.sh up createChannel -c mychannel -ca
```

## Deploy CC (ChainCode)

```
./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go
./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-javascript/ -ccl javascript
```
## Call the CC using CLI
1- Register the app and init
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"InitLedger","Args":[]}' 
```
2- Invoke chaincode function
```
peer chaincode query -C mychannel -n basic -c '{"Args":["GetAllAssets"]}' 
```

## Call the CC using WebApp
2- 
```
cd asset-transfer-basic/application-javascript
npm install
node app.js
```

Cleanup
```
docker system prune -a --volumes
```
