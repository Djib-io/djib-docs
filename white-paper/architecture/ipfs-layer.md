# IPFS layer

The Interplanetary File System (IPFS) is a protocol and peer-to-peer network for storing and sharing data in a distributed file system.

&#x20;IPFS uses content-addressing to uniquely identify each file in a global namespace connecting all computing devices.&#x20;

Djib is running a cluster of IPFS which is open to community to join by joining to this cluster node owner will be incentivized

![](<../../.gitbook/assets/image (11).png>)

### IPFS Cluster Architecture

The IPFS Cluster software consists of three binary files:

* ipfs-cluster-service runs a Cluster peer (similar to ipfs daemon) using a configuration file and by storing some information on disk.&#x20;
* ipfs-cluster-ctl is used to communicate with a Cluster peer and perform actions such as pinning IPFS CIDs to the Cluster.
* &#x20;ipfs-cluster-follow runs a follower Cluster peer. It can be used as a substitute for ipfs-cluster-service for this specific use case and mixes some service and ctl features.

The Cluster peer communicates with the IPFS daemon using the HTTP API (localhost:5001). The IPFS daemon must be launched and running separately.

Usually, ipfs-cluster-ctl is used on the same machine or server on which ipfs-cluster-service is running. For example, ipfs-cluster-ctl pin add \<hash> will instruct the local Cluster peer to submit a pin to the Cluster. The different peers in the Cluster will then proceed to ask their local IPFS daemons to pin that content. The number of pins across the Cluster will depend on the replication factor set in the Cluster configuration file.

#### The Cluster swarm

IPFS Cluster is a fully distributed application.

ipfs-cluster-service runs a Cluster peer and all peers are equal. Cluster peers form an separate, isolated libp2p \[private] network, which uses the cluster\_secret (a 32-bit hex-encoded passphrase present in the configuration of every peer).

This network does not interact with the main IPFS network, nor with other private IPFS networks and is solely used so that Cluster peers can communicate and operate.

The network uses a number of blocks also used by IPFS (DHT, PubSub, Bitswap…) but, unlike IPFS, does not enjoy public bootstrappers.

This means that Cluster peers will normally need their own bootstrappers (it can be any peer in the Cluster), although sometimes they can rely on mDNS discovery.

This also means that Cluster peers operate separately from IPFS with regards to NAT hole punching, ports etc.

#### The shared state: consensus

All peers in the Cluster maintain a global pinset. Making every peer maintain the same view of the pinset regardless of concurrent pinning operations and on a distributed application layout requires coordination given by what is called a consensus component. Cluster supports two implementations:

* a CRDT-based approach, based on Conflict-Free Replicated Datatypes
* a Raft-based approach, based on a popular log-based consensus algorithm

The relevant details and trade-offs between them are outlined in the Consensus Components section. The choice (which must be performed during initialization and cannot be easily changed), heavily affects the procedures for adding, removing and handling failures in Cluster peers.

The shared state can be inspected with ipfs-cluster-ctl pin ls and is the only piece of information present locally in every peer. Pin status (status) information, or peers information (peers ls) for other than the peer that is running the command, must be obtained at runtime from their respective peers and assembled together.



read more [here](https://cluster.ipfs.io/documentation/deployment/architecture/)
