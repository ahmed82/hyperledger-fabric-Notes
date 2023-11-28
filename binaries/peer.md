# peer binary

The configuration file for the peer is  `core.yaml`.

path to the configuration file set in:
```
FABRIC_CFG_PATH
```

The configuration can be overridden, by an environment variable. as:
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


## peer lifecycle chaincode
```
 peer lifecycle chaincode <cmd> --flag
```
1- package
* Packages the chaincode.
```
peer lifecycle chaincode package $HOME/gocc.1.0-1.0.tar.gz -p opt/chaincode_app --label gocc.1.0-1.0
```
The flag -p : for the chaincode code location

2- install
* installs the chaincode to the specified peer.
* peer need to be up
```
CC_LABEL=gocc.1.0-1.0
CC_PACKAGE_FILE=$HOME/$CC_LABEL.tar.gx
peer lifecycle chaincode install $CC_PACKAGE_FILE
```
Check the installation:
```
ls $CORE_PEER_FILESYSTEMPATH/lifecycle/chaincodes/
```
3- queryinstalled
* Lists the installed chaincode on the peer.
```
peer lifecycle chaincode queryinstalled
```

