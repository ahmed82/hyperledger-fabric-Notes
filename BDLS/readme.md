# GO
Download Go runtime
https://golang.org/doc/install

* GOROOT
```shell
%USERPROFILE%\Go
```
* Path
```shell
%GOPATH%\bin

%GOROOT%\bin
#or
%USERPROFILE%\Go\bin

```
verify the installation
```
$ go version
```
1- install GO plugin in VScode

2- Ctr+Shift+p Then Type:
```
Go: install/update
```
![image](https://user-images.githubusercontent.com/9446035/125891712-b1b0d067-46aa-47f2-a22b-47f3115662ae.png)

After success setup...

![image](https://user-images.githubusercontent.com/9446035/125891997-5a6be85f-0990-41ba-840d-a305c85d23a7.png)

Select all.
3-  Ctr+Shft+p then type:
```
preferences: Open Setting (JSON)
```
add:
```
{
    "ibm-blockchain-platform.fabric.wallets": [],
    "ibm-blockchain-platform.fabric.gateways": [],
    "ibm-blockchain-platform.fabric.environments": [],
    "ibm-blockchain-platform.ext.repositories": [],
    "ibm-blockchain-platform.ext.enableLocalFabric": true,
    "files.autoSave": "afterDelay",
    "ibm-blockchain-platform.v2.fabric.runtime": {
        "1 Org Local Fabric": 8081
    }
}
```


# RUN
```
Go: build <main.go>
```
bdls
```
./emucon run
```

## run + Test
```
./emucon run --id 0 --listen ":4680
./emucon run --id 1 --listen ":4681"
./emucon run --id 2 --listen ":4682"
./emucon run --id 3 --listen ":4683"

cd ../..
go test -v -cpuprofile=cpu.out -memprofile=mem.out -timeout 2h
 ```
