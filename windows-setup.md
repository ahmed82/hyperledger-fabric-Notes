# Using the Fabric test network
## Before you begin
```
curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.2.2 1.4.9
```


## Bring up the test network
```
cd fabric-samples/test-network
```

## The components of the test network

```
docker ps -a
```

# Creating a channel
```
./network.sh createChannel
./network.sh createChannel -c channel1
./network.sh up createChannel

```
