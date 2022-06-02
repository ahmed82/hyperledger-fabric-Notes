# GO
Download Go runtime
https://golang.org/doc/install

verify the instalation
```
$ go version
```
1- install GO plugin in VScode

2- Ctr+Shft+p then type:
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
