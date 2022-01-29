# hyperledger-fabric-Notes


TODO: Write-ahead logging


```
make basic-checks integration-test-prereqs
```

2. Build Fabric Binaries and Docker Images
```
cd fabric
make clean
make configtxgen configtxlator cryptogen orderer peer docker
```
