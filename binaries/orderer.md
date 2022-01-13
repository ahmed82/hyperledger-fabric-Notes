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
```
General:  		General properties of the Orderer.
FileLedger: 	Filesystem location of the block data.
Consensus:	  Used for managing storage for Orderer type etcdraft
Kafka: 		    kafka environment setup.
Debug: 		    Debug information control.
Operations: 	Operation server info (network monitoring| alerts) & TLS configuration 				for the operations server..
Metrics:		  Orderer generate metrics info, collected by 3ed party
	            Metrics provider is one of statesd, Prometheus, or disabled
 ```             
All configuration can be overridden at runtime by setting the environment variables:
```
ORDERER_GENERAL_...
ORDERER_FILELEDGER_...
```

CSP expose the cryptographic functions:
1- Software CSP
* Implemented as software libraries
 		such as windows DLL or Linux shared object
2- Hardware CSP 
* Hardware Security Modules (HSM)
* Smart Cards
PKCS#11
```
BCCSP:        Blockchain Crypto Service Provider
  Default: SW		#For Software CSP
  Default: PKCS11 	#For Hardware CSP
```
