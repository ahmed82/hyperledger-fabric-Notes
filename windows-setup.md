# Using the Fabric test network
## Before you begin
```powershell
curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.2.2 1.4.9
```
Important: This tutorial is compatible with the Fabric test network sample v2.2.x. After you have installed the prerequisites, you must run the following command to clone the required version of the hyperledger/fabric samples repository and checkout the correct version tag. The command also installs the Hyperledger Fabric platform-specific binaries and config files for the version into the /bin and /config directories of fabric-samples.
```powershell
git fetch --tags
git tag
git checkout tags/v2.2.2
```


## Bring up the test network
```powershell
cd fabric-samples/test-network
./network.sh -h
./network.sh up
```

## The components of the test network

```powershell
docker ps -a
```

# Creating a channel
```powershell
./network.sh createChannel
./network.sh createChannel -c channel1
./network.sh up createChannel

```
