export PATH=export PATH=<path to download location>/bin:$PATH/bin:$PATH

confitx.yaml / for the Fabric arch

## Links

https://hyperledger.github.io/caliper/v0.4.2/installing-caliper/ 

https://hyperledger-fabric.readthedocs.io/en/release-1.4/build_network.html 

https://hyperledger-fabric.readthedocs.io/en/release-1.4/install.html 


https://hyperledger-fabric.readthedocs.io/en/release-1.4/write_first_app.html?highlight=fabcar#the-fabcar-smart-contract 

https://kctheservant.medium.com/demo-of-multi-channel-network-in-hyperledger-fabric-640f7158e2d3 

https://github.com/TheIanSim/FabCar 

-------
 ## Bring Up the Network

  ```
  cd fabric-samples/first-network
  ./byfn.sh up
  ```
  Generate Network Artifacts
  ```
  ./byfn.sh generate
  ```
# we use the -l flag to specify the chaincode language
# forgoing the -l flag will default to Golang
```
./byfn.sh up -l node
./byfn.sh up -l java
```
 In addition to support for multiple chaincode languages, you can also issue a flag that will bring up a five node Raft ordering service or a Kafka ordering service instead of the one node Solo orderer.
```
./byfn.sh up -o etcdraft
./byfn.sh up -o kafka 
 
```
Network Down
 ```
 ./byfn.sh down
 ```
 
  
  
  
