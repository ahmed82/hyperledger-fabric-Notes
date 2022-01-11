# peer
```
Peer channel create -o localhost:7050 -c myChannel -f $CONFIG_PATH/channel.tx
Peer node start -o localhost:7050
Peer channel join -o localhost:7050 -b ./fist.block
peer channel list

peer channel fetch config -c mychannel -o localhost:7050
peer channel getinfo -c mychannel
```
