# protobuf

https://grpc.io/docs/protoc-installation/
### Ubuntu

```bash
sudo apt update
sudo apt install -y protobuf-compiler
protoc --version  # Ensure compiler version is 3+
```
![image](https://github.com/ahmed82/hyperledger-fabric-Notes/assets/9446035/b6096e53-6638-4550-9625-3a1565b34ee7)

### Verify the installation
```
protoc
man protoc
```

* https://grpc.io/
* https://grpc.io/docs/languages/go/quickstart/
* Install the protocol compiler plugins for Go using the following commands:
```
$ go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28
$ go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2
```
* Update your PATH so that the protoc compiler can find the plugins:

```
$ export PATH="$PATH:$(go env GOPATH)/bin"

```


```
protoc --proto_path=proto proto/*.proto --go_out=pb --go-grpc_out=pb
protoc --proto_path=proto proto/*.proto --go-grpc_out=gen
```

* Uninstall Protobuf 
```
sudo apt-get remove protobuf-compiler
sudo apt-get autoremove protobuf-compiler  # remove its dependent packages
```
