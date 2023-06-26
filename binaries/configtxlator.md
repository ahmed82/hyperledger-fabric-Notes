# configtxlator
Configuration + Translator

![image](https://github.com/ahmed82/hyperledger-fabric-Notes/assets/9446035/4b9b9a10-456a-481f-8cfc-eb4754afc1d6)


1. Rest Service API.
   Lunch the Rest Service for configuration
```shell
configtxlator start
```
The server will run on:
--hostname: localhost
--port:     7059

2. CLI

configtxlator command --flags

![image](https://github.com/ahmed82/hyperledger-fabric-Notes/assets/9446035/b0722a78-4b1b-433c-a403-2db1c3f7a1bb)
![image](https://github.com/ahmed82/hyperledger-fabric-Notes/assets/9446035/79af847c-c967-48e2-9fb0-63431385a3ca)

![image](https://github.com/ahmed82/hyperledger-fabric-Notes/assets/9446035/446fd1f7-145e-4092-bce9-b7ed469f6906)
CLI example
```shell
configtxlator proto_decode --type common.Block --input cb.block --output cb.json
```

![image](https://github.com/ahmed82/hyperledger-fabric-Notes/assets/9446035/3f582e87-a92b-4465-a482-c51a6f157ecb)

## jq
https://jqlang.github.io/jq/download/

* Install
```
sudo apt-get install jq
```

* Learn jq : https://jqplay.org

### jq CLI

cat file.json | jq expression > result.json

