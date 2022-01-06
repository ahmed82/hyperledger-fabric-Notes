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

Organizations
```
Orgs defined as anchors (&) element to avoid repetitions.
- &Org1
    Name: Org1MSP
    ID: Org1MSP
    MSPDir: ../organizations/peerOrganizations/org1.example.com/msp
```
![image](https://user-images.githubusercontent.com/9446035/148457035-463eb13b-4b14-43fe-a5e4-a7d30e9f46de.png)
