# peer
Configuration by using `core.yaml`.

path to configuration file set in:
```
FABRIC_CFG_PATH
```

configuration can be overridden by set environment variable at runtime. as:
```
CORE_...
```
* peer command pattern looks like:
```
peer [command] [subcommand] --flags
peer help
peer [command] help
```
* Example
```
Peer channel create -o localhost:7050 -c myChannel -f $CONFIG_PATH/channel.tx
Peer node start -o localhost:7050
Peer channel join -o localhost:7050 -b ./fist.block
peer channel list

peer channel fetch config -c mychannel -o localhost:7050
peer channel getinfo -c mychannel
```


# peer lifecycle chaincode
```
 peer lifecycle chaincode <cmd> --flag
```
1- package
* Packages the chaincode
2- install
* installs the chaincode to the specified peer
3- queryinstalled
* Lists the installed chaincode on the peer
