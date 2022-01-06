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
