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
specify the output folder for the crypto materials
```
cryptogen generate  --config=./crypto-config.yaml --output=./crypto-config
```
NOTE: for usable code make .sh file including the aove commend line.
