# Hyperledger Fabric Binaries

## 1- cryptogen Tool

The sample configuration of the cryptogen tool/ to save the template we pipe the output to name.yaml

```
cryptogen showtemplate > crypto-config.yaml
```

To Generate the Sample crypto mateial we use generate and the previos config file - Default result under the folder name crypto-config
```
cryptogen generate  --config=./crypto-config.yaml
```
2- configtxgen Tool

Generate the configtx.yaml
configure a file at a path specified in Environment Variable
```
FABRIC_CFG_PATH
```
if NOT the configtxgen Tool search for configtx.yaml in the current folder.

To overwrite by export CONFIGTX_`ORDERER_ORDERERTYPE=bdls
