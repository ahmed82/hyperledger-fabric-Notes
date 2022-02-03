# hyperledger-fabric-Notes


TODO: Write-ahead logging


```
make basic-checks integration-test-prereqs
```

2. Build Fabric Binaries and Docker Images
```
cd fabric
make clean
make configtxgen configtxlator cryptogen orderer peer docker
```

# Crypto
```
openssl x509 -in ks.file -text -noout
```
```
openssl pkcs12 -info -in sk.keystore
```
```
keytool -v -list -keystore sk.keystore
```
https://www.sslshopper.com/certificate-decoder.html
https://certlogik.com/decoder/

C:\Hyperledger-Fabric\fabric-samples\test-network\organizations\ordererOrganizations\example.com\msp
C:\Hyperledger-Fabric\fabric-samples\test-network\organizations\ordererOrganizations\example.com\orderers\orderer.example.com\msp
```
openssl x509 -in cert.pem -text
```
```
openssl x509 -in cert.pem -text -noout
keytool -printcert -file certificate.pem
```
```
keytool -printcert -file cert.pem
```
