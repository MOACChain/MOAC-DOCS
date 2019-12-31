Introduction
^^^^^^^^^^^^^^^

General information about AppChain
----------------------------------

Using blockchain sharding technology, a AppChain functions as a child blockchain within the platform that operates above the global BaseChain, and is responsible for Smart Contracts management. AppChains also enable high volume transactions using a variety of consensus systems. Consensus system is an agreement system that provides accountability and verification of transactions.

The platform’s advanced layered Multi-blockchain architecture increases overall transaction processing speeds up to 100x faster (TPS) than existing blockchain platforms. Meanwhile, AppChains enhance token concurrency rates up to 10,000 times, for a truly scalable solution.

MOAC is one of the first blockchain solutions to implement an unique AppChain per Smart Contract, providing for efficiency and scalability beyond existing solutions. The MOAC Platform uses AppChains to separate processing tasks and isolate blockchain functions from business logic for each individual smart contract. By providing each Smart Contract with its own unique AppChain, it enables Smart Contracts to use a variety of consensus protocols and results in a wider range of potential business logic use cases.

Developers have the freedom to select the consensus protocol that best fits their use case and determine the number of nodes allocated to a specific Smart Contract. All the states of the Smart Contract are saved inside the local AppChain and can write data to the BaseChain as needed for finality.

AppChains significantly reduce the cost of smart contract operations and allow developers to rapidly test different application and service ideas. MOAC’s AppChains are able to interconnect with all other non- MOAC blockchains using Cross-Chain capabilities. This allows both users and their decentralized applications (DApps) to migrate easily to the MOAC Platform, with no prior blockchain knowledge. It also provides a decentralized file storage solution (IPFS) which is currently missing from other major blockchains.

MOAC needed for a AppChain
-------------------------------

The AppChain requires the participated SCS mining nodes to deposit some MOAC before mining:

1、每个子链节点在第一次启动后，将会有一个唯一的墨客钱包地址，在部署子链前，需要在向这个地址打入至少1个MOAC作为运行费用；

2、每个子链节点注册进入子链矿工池时，需要向矿工池缴纳一定的押金，最小值由子链控制合约设置，最大值不限。子链节点每被选中一次，将会扣除一定数额的押金，当押金被扣完后，该节点将不会再参与新的子链，退出子链时，可以调用方法取回押金；

3、当一个子链节点注册成一个监听节点时，需要缴纳一定的押金；当退出子链时可以取回押金；

4、押金一般不会扣除，但在flush时，如果有企图作弊的节点，将会按照规则踢出子链，并扣除押金，不再返还；

维护子链的MOAC消耗：

首先，调用子链方法不会消耗任何gas，但是，dapp运营方需要向子链控制合约地址打入一定量的MOAC维持子链运行，这部分MOAC将会消耗在给子链矿工费用、子链向母链flush状态，以及母链充提gas返还上。这个维护消耗可以通过调整子链的flush周期来部分改变。


Key points to create a AppChain
---------------------------------

1. The VNODE used to connect with SCSs should have good internet connection and high bandwidth. 
2. The VNODEs should be added as peer with each other to keep the communication well.
3. SCS servers need to have good time synchronization with the internet clock;
4. 
