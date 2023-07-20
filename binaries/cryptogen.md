# Hyperledger Fabric Binaries

## 1- cryptogen Tool

```bash
cryptogen version
cryptogen help
cryptogen showtemplate
```
## configuration
The sample configuration of the cryptogen tool/ to save the template we pipe the output to name.yaml

```bash
cryptogen showtemplate > crypto-config.yaml
```
## Generate
To Generate the Sample crypto mateial we use generate and the previos config file - Default result under the folder name crypto-config
![image](https://user-images.githubusercontent.com/9446035/220531931-d815926f-a6b2-4e49-84f4-48d2bd86dcce.png)

```bash
cryptogen generate  --config=./crypto-config.yaml
```
* Specify the output folder for the crypto materials
```bash
cryptogen generate  --config=./crypto-config.yaml --output=./crypto-config
```
NOTE: for usable code make .sh file including the above command line.

![image](https://user-images.githubusercontent.com/9446035/155017494-8901f4ae-dc8c-4fed-9bde-8ab1a3077d8d.png)
# Orderer
![image](https://user-images.githubusercontent.com/9446035/220533753-58cea54b-e1c9-446d-8ac1-4d3b5f037453.png)
## Usage
![image](https://user-images.githubusercontent.com/9446035/220535781-4169503f-9fed-40a6-b454-bec8149f5092.png)

# Peer
![image](https://user-images.githubusercontent.com/9446035/220535985-ecfb1f31-8b40-45c8-b5f2-e5522137f089.png)
-----
# Extend 
* for adding components to the existing setup
```bash
cryptogen extend  --config=./extension-crypto-config.yaml --input=<<path Crypto folder>>
```
![image](https://user-images.githubusercontent.com/9446035/220537014-4020be73-ba6e-4185-9af7-a6ee8b661efe.png)

## Summery
![image](https://user-images.githubusercontent.com/9446035/220533429-db7b5fa5-591a-41ae-8445-5fec2313ff80.png)


## Generated components
![image](https://user-images.githubusercontent.com/9446035/220534951-c9c81b44-cd44-489c-9dfd-77954ce41038.png)
