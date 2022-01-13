# Orderer.yaml

Orderer intialaized starting by checking Genesis Block as needed.
1- look for the orderer.yaml
as configure environment variable
```
FABRIC_CFG_PATH
```
Control Model level logging
FATAL | PANIC | ERROR | WARNING | INFO | DEBUG
```
export FABRIC_LOGGING_SPEC=INFO
```
Control logging format:
```shell
The Default logging format:
"%{color}%{time:2006-01-02 15:04:05.000 MST} [%{module}] %{shortfunc} -> %{level:.4s} %{id:03x}%{color:reset} %{message}"
```
to use custom loggin format:
```
export FABRIC_LOGGING_FORMAT=
```

# Ledger Folder
Location set in FileLedger section in orderer.yaml.
That location can be overwritten by setting environment variable
```
export ORDERER_FILELEDGER_LOCATION=$HOME/ledgers/orderer/com/ledger
```
Orderer binary use levelDB, use the Ledger Folder location to write the block data.
if Ledger Folder location is set to network drive or to host file system from the VM, error will be throws
