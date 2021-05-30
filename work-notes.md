Hyperledger Fabric v2.x

Contents
Hyperledger Introduction	2
Hyperledger Fabric Introduction	2
Nodes	2
Orderer	4
Peers	6
Client Node	8
CA Server	8
Hyperledger Fabric binary	9
Peer	9
Orderer	9
Configtxgen	9
Configtxlator	9
Cryptogen	9
Fabric-ca-server	9
fabric-ca-client	9
Fabric binary Details	9
Cryptogen	9
configtxgen	12
Commands	12
Orderer Binary	13
What’s new in Hyperledger Fabric v2.x	14
Decentralized governance for smart contracts	14
Using the new chaincode lifecycle	15
New chaincode application patterns for collaboration and consensus	15
Private data enhancements	16
External chaincode launcher	17
State database cache for improved performance on CouchDB	18
Alpine-based docker images	18
Sample test network	18
Upgrading to Fabric v2.x	18



Hyperledger Introduction
Hyperledger project is an open-source collaborative effort created to advance cross-industry blockchain technologies by the Linux foundation.

 Hyperledger Fabric Introduction
Hyperledger Fabric is a DLT framework, Distributed Ledger Technology for the Business. It is an open-source framework written in GOLANG.
https://github.com/hyperledger/fabric
The listed points make the Fabric is suitable for business application.
1-	Permissioned network
a.	Member identities are known.
b.	Participants have role-based access.
2-	Confidential transactions
a.	Not all transaction should be visible to all.
b.	Members can transact privately.
3-	No cryptocurrency
a.	No incentivization needed.
b.	No Crypto tokens needed for transactions.
4-	Programmable
a.	Smart Contracts.
b.	Business Logix Implementation.
Blockchain Network technologies have the concept of nodes. Nodes connect to each other to form the Blockchain network.
The public Blockchain network such as Ethereum or Bitcoin all nodes are equal.
The Hyperledger Fabric all nodes may not be equal. In the last there a concept of Members and they are legally separate entities that join the blockchain network, these members shared one or more Distributed ledger(s) and each of these members host nodes.
Members = Legally separate entities
Nodes are used for submitting the transactions and manager the state of the ledger within the organization, moreover the node are referred to the communication entities of the blockchain. However, each node assign to identity certificate even the user in the blockchain ecosystem that use an application to communicate with the node must have a certificate. Ledger are managing the state of assets and may represents anything in the real word that can be represented digitally.

The Node initiate the transaction which lead to the execution of the Chain code and the transaction are recorded in the ledger in order also its lead to change the state of the asset in the ledger. Therefore, there is one ledger per channel that responsible for transaction privacy is achieved in the Hyperledger Fabric network.
A Hyperledger Fabric channel is a private “subnet” of communication between two or more specific network members, for the purpose of conducting private and confidential transactions.
Everything that interacts with a blockchain network, including peers, applications, admins, and orderers, acquires their organizational identity from their digital certificate and their Membership Service Provider (MSP) definition.


	                  Manages Asset State

      Transactions


Figure -1.1: Hyperledger Fabric architecture

Nodes are three types: Orderers, Peers and Client and the Peers could be Leader peer or anchor peer.




	Leader peer

	Anchor peer


Figure -1.2: Hyperledger Fabric Node types
Orderer: also known as ‘Ordering Service’ What is ordering? Many distributed blockchains, such as Ethereum and Bitcoin, are not permissioned, which means that any node can participate in the consensus process, wherein transactions are ordered and bundled into blocks. Because of this fact, these systems rely on probabilistic consensus algorithms which eventually guarantee ledger consistency to a high degree of probability, but which are still vulnerable to divergent ledgers (also known as a ledger “fork”), where different participants in the network have a different view of the accepted order of transactions.

Hyperledger Fabric works differently. It features a node called an orderer (it’s also known as an “ordering node”) that does this transaction ordering, which along with other orderer nodes forms an ordering service. Because Fabric’s design relies on deterministic consensus algorithms, any block validated by the peer is guaranteed to be final and correct. Ledgers cannot fork the way they do in many other distributed and permissionless blockchain networks.
A deterministic model does not include elements of randomness. Every time you run the model with the same initial conditions you will get the same results. 
A probabilistic model includes elements of randomness. Every time you run the model, you are likely to get different results, even with the same initial conditions. A probabilistic model is one which incorporates some aspect of random variation. 
Ordering node provide the communication channel for the Fabric network, Implemented with Message-Oriented Middleware. (Creates blocks, Order of transaction)
•	Responsible for consistent ledger state across the blockchain network.
Provide Consensus mechanism and ensures orders of transactions is maintains. 
•	Creates the block and guarantees atomic delivery of the created blocks to the peer network.
SOLO: is a messaging component built in the Orderer node use single node, good for development environment or dev mode since it has only single order instance that could lead to a single point of failure in production, but it is Ok for development.
RAFT: A consensus algorithm built into the Orderer binary like SOLO, it recommended for production with the third option Kafka.
Kafka: Apache Kafka is a framework implementation of a software bus using stream-processing. It is an open-source software platform developed by the Apache Software Foundation written in Scala and Java. it has some challenging that you need to setup a Kafka broker across the multicable member of the organization. Requires Kafka cluster to be managed by the organizations.
 
 
Peers: A blockchain network is comprised primarily of a set of peer nodes (or, simply, peers). Peers are a fundamental element of the network because they host ledgers and smart contracts. Recall that a ledger immutably records all the transactions generated by smart contracts (which in Hyperledger Fabric are contained in a chaincode, more on this later). Smart contracts and ledgers are used to encapsulate the shared processes and shared information in a network, respectively. These aspects of a peer make them a good starting point to understand a Fabric network.  
Each peer instance in the network maintains its own copy of the ledger. A blockchain network is comprised of peer nodes, each of which can hold copies of ledgers and copies of smart contracts. In this example, the network N consists of peers P1, P2 and P3, each of which maintain their own instance of the distributed ledger L1. P1, P2 and P3 use the same chaincode, S1, to access their copy of that distributed ledger.

 
Figure -1.3 Peers network

The peer node maintains the transaction log, State database and the chain code deployed to it.
The peer node exposes a service, and those services are built on GRPC. the services are invoked by client, peers and by Orderer. Peers exchange block data by using  gossip data communication protocol 










         gossip



Figure -1.4 Peers Architecture

Two part of the ledger:
•	Transaction log: Log is immutable that cannot be deleted or changed.
•	State usually using no-SQL database for easy and quick date retrieving Couch DB.
Anchor Peer are known outside Organization, each organization has at least one anchor peers because those peers are discoverable.
Leader Peers are only peers who received blocks from Orderers. Either assign statically or the admin of the organization set the nodes to dynamically elect a leader peer. 
Leadership assignment is at channel level in order to join multiple channels. That receive new block from the Orderer and by gossip dissemination protocol send it to other peers within the organization.
 




Figure -1.5 Peers different types 	
    Submits Txns



Client Node acts on behalf of the end user also named as a submitting-Client since the client submit the transaction request to the network. The transactions are isolated in that channel and available only for the channel members. Also, the Chain Code (Smart Contract) deployed to a Channel Not the network.
There is a special system channel called Ordering System Channel also called as Bootstrap Channel, this channel is automatically created by the time of the network initialization. However, peers may join multiple channels. Each of that channel has its own state database and distributed ledger.
 	

 A	  C   D
 B

Figure -1.4 Organizations C & D sharing the same Channel.

CA Server Fabric CA use PKI public key Infrastructure and it has implementation of the CA know as CA Server. CA Server exposes a services: Identity Registration, Identity Enrollment and Certificate management.

Hyperledger Fabric Software 
We can category the software component of Hyperledger Fabric into three categories. All those configuration are required a YAML file format for configuration.
Core components	Tool / Utilities	Fabric Certification Authority
Peer
Orderer
	configtxgen
configtxlator
cryptogen
discover
idemixgen	fabric-ca-server
fabric-ca-client

Hyperledger Fabric binary
Overview of the Hyperledger Fabric binary, 
Peer binary are used in two ways when its lunch as process it become a node in the network, and it may be tag as Anchor or Leader. Second it might be used as tool to manage network and channels config.
Orderer binary can use built in messaging SOLO and it could be configured to connects to Kafka cluster.
Configtxgen used for management of network and channel configuration.
Three type of artifact that you can manage with Configtxgen:
1-	Genesis Block: Which is the order channel block user by the orderer binary.
2-	Channel Tx: Create channel transaction / when create the application channel.
3-	Anchor Peer Tx: Anchor Peer update Transaction for the organization that part of the network. – Deprecated in Fabric 2.x.
The tool exposes two type of commend:
1-	Output – Requires Configuration in a file in YAML format. Use configtx.yml as an input to the Configtxgen tool that generate the ‘Config Artifact’.
2-	Inspection – outputs configuration as JSON. The above config artifact is binary.
Configtxlator this tool is used to translate between the protobuf (protocol buffer) which is a binary format that used by GRPC and JSON format.
Cryptogen tool it generates the crypto materials for testing.
Fabric-ca-server binary is lunch as server as standalone process that expose a services for managing identity and certificates. 
fabric-ca-client binary is a command line interface tool that used for excuting services on the Fabric-CA-Server, this is the tool is used to managing the identity and certificates. 

Fabric binary Details
Cryptogen
Certification Authorities (CA) manage identities.
Decentralized identity management
 
Utility for generation the crypto material (refer to certificate and key store)
•	Used for test environment setup.
•	Requires the information in YAML format.
 
It is not used in production, the real identities are issued by CA.



 
configtxgen
Configuration artifact types there are three types of artifacts that we can managed.
1-	Genesis Block * The order channel block used by the order binary.
2-	Channel Tx * Create channel transaction when create application channel.
3-	Anchor Peer Tx  Deprecated in Fabric 2.x

Commands
•	Output – Generate the configuration artifacts. Requires the configuration parameters in a file in YAML format.
 
•	Inspection – outputs configuration as JSON because the configuration are not readable and in binary format.
Configtxgen does not have any dependency on Fabric runtime to execute the tool transactions.
The reason: sometimes policies associated with network, the number of signatures is needed to execute a transaction, for example a transaction require organization A and B to sign, then configtxgen generate the transaction and both organizations Admin’s sign that transaction, then one of the admin submit the signed transaction.
 


```
configtxgen version
configtxgen help
```

Orderer Binary 

Development Machine Setup
Deep Dive into the Design, Management & Operation of Hyperledger Fabric Blockchain Networks.
 
Fabric 2.x
•	Major enhancements to Chaincode Lifecycle
•	Introduction of implicit collections
•	State database caching for CouchDB
•	Docker image optimization (using Alpin rather ubuntu)
•	New peer command for chaincode lifecycle
o	Peer lifecycle chaincode --flags
 
 
Setting the Stage
Consortium
	An association of two or more individuals, companies, organization, or government with objective of achieving a common goal.
•	Industry
•	Technology
Fabric uses decentralize administration by way of policy. Policies are created by members of Consortium.
 
My role is Lead engineer.
From this step my work will start to setup the networks:
Phase -01 Setup the network for two organizations: FOODLION, Orderer organization
Phase -02 Add Budget as a member to the network.
Phase -03 on board additional members Stop&Shop. Orderer and three peer organizations.

Cryptogen Tool
A command line utility for generating the crypto material which is refer to crypto store and certificate.
For test environment only.
$> Cryptogen command –flag <args>

