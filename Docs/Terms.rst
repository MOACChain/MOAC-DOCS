Terms
^^^^^^

Vnode
----------------------
Verification node (VNODE or V-node), is the application that running a full
MOAC MotherChain node in the MOAC network. It can mine blocks in the
network, transfer moac, perform the POW consensus, and pass MicroChains data in
MOAC network. 

SCS
----------------------
Smart Contract Server(SCS) is used to form MicroChains. It can do MicroChain mining and monitoring. One SCS can form multiple MicroChains.

SubChainProtocolBase
----------------------
A MotherChain contract defines the protocol for the SCSs to register and form a SCS pool.

SCS pool
----------------------
A pool of SCSs with the same protocol to form one type of MicroChain. The protocol is defined in the SubChainProtocolBase.sol. The SCSs need to register itself into the pool by calling the deployed SubChainProtocolBase contract with paying some deposit. A MicroChain contract using the same protocol can pick up the SCSs and form the MicroChain. 

VNODEProtocolBase
----------------------
A MotherChain contract defines the protocol for the VNODEs to register and pass data for MicroChains.

VNODE pool
----------------------
A pool of VNODEs with the same protocol to pass data of the MicroChain. The protocol is defined in the VNODEProtocolBase.sol. The VNODEs need to register itself into the pool.

Subchainbase
----------------------
A MotherChain contract create the MicroChain by using the SCSs in the SCS pool. It requires the input 

MicroChain Monitor
---------------------
SCS Monitor is a SCS node monitoring MicroChain status. MicroChain owner can use this SCS node to monitor MicroChain status and get data from MicroChain. Only the owner of MicroChain can add monitors.

flush
---------
A special operation of MicroChain. Each MicroChain needs to defined the flush period in terms of MotherChain block numbers when it is created. In each flush operation, the status of the MicroChain is written to the MotherChain. In the flush operation, MicroChain will give out the mining rewards to the SCS miners, deposit/withdraw MicroChain tokens, and other transactions that may change the status in the MotherChain. 

Dappbase
--------------------
A MicroChain contract controls the Dapps on the MicroChain. It is available in the release of nuwa 1.0.8 and later. 