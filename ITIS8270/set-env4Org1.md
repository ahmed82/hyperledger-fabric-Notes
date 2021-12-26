# Creat .bat filr 'set-env4Org1.bat' 

run 'cmd as admin then navigate to the drectory <PATH>\fabric-samples\test-network\

add the code below in .bat file and make sure the location of the file in 'fabric-samples\test-network'
  
```shell
rem Environment variables for Org1
setx /M CORE_PEER_TLS_ENABLED "true"
setx /M CORE_PEER_LOCALMSPID "Org1MSP"
setx /M CORE_PEER_TLS_ROOTCERT_FILE "%CD%\organizations\peerOrganizations\org1.example.com\peers\peer0.org1.example.com\tls\ca.crt"
setx /M CORE_PEER_MSPCONFIGPATH "%CD%\organizations\peerOrganizations\org1.example.com\users\Admin@org1.example.com\msp"
setx /M CORE_PEER_ADDRESS "localhost:7051"
@pause
```
