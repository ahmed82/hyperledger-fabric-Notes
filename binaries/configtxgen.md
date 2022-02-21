## 2- configtxgen Tool

Generate the configtx.yaml
configure a file at a path specified in Environment Variable
```
FABRIC_CFG_PATH
```
if NOT the configtxgen Tool search for configtx.yaml in the current folder.

To overwrite by export CONFIGTX_ORDERER_ORDERERTYPE=bdls
## configtx.yaml
6 sections
Organizations:  `List the Member Orginazations`
Capabilities:   `Binary version managment acrose network`
Application:    `Application configuration`
Orderer:        `Configuration for the Orderer`
Channel:        `Channel related parameters`
Profiles:       `Setup multiple configs in a file`

![image](https://user-images.githubusercontent.com/9446035/148457171-abe37c21-b671-413a-ae49-4848d808162f.png)

![image](https://user-images.githubusercontent.com/9446035/148458272-2b09a708-b110-46b2-b824-614ae8ed5df7.png)

The Application Section has a subsection organizations under which there is a list of organizations that participate in the application type transaction.

The Profiles are set a named sub-section. each of the profiles are needed for the generation of cofig components.
```
configtxgen -profile BdlsChannel
```

## Genesis Block
The genesis file is used to lounch the orderer
```
configtxgen -outputBlock ./first-channel.block -profile TwoOrgsChannel -channelID myChannel

configtxgen -inspectBlock first-channel.block > ./temp/channel.json
```

##  Channel Tx
The Tx file is used by peer for submitting transaction

```
configtxgen -outputCreateChannelTx ./first-channel.tx -profile TwoOrgsChannel -channelID myChannel

configtxgen -inspectCreateChannelTx first-channel.tx > ./temp/channel.json
```

debuge
```
C:\Users\1426391\Desktop\Desktop\test\fabric-samples\test-network\configtx
To:
C:\Users\1426391\Desktop\Desktop\test\fabric-samples\config
```

## Create .sh file for create the Artifacts
```shell
chmod -R 0755 ./crypto-config
# Delete existing artifacts
rm -rf ./crypto-config
rm genesis.block mychannel.tx
rm -rf ../../channel-artifacts/*

#Generate Crypto artifactes for organizations
cryptogen generate --config=./crypto-config.yaml --output=./crypto-config/



# System channel
SYS_CHANNEL="sys-channel"

# channel name defaults to "mychannel"
CHANNEL_NAME="mychannel"

echo $CHANNEL_NAME

# Generate System Genesis block
configtxgen -profile OrdererGenesis -configPath . -channelID $SYS_CHANNEL  -outputBlock ./genesis.block


# Generate channel configuration block
configtxgen -profile BasicChannel -configPath . -outputCreateChannelTx ./mychannel.tx -channelID $CHANNEL_NAME

echo "#######    Generating anchor peer update for Org1MSP  ##########"
configtxgen -profile BasicChannel -configPath . -outputAnchorPeersUpdate ./Org1MSPanchors.tx -channelID $CHANNEL_NAME -asOrg Org1MSP

echo "#######    Generating anchor peer update for Org2MSP  ##########"
configtxgen -profile BasicChannel -configPath . -outputAnchorPeersUpdate ./Org2MSPanchors.tx -channelID $CHANNEL_NAME -asOrg Org2MSP
```
