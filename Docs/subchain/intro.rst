Introduction
^^^^^^^^^^^^^^^

General information about MicroChain
----------------------

Using blockchain sharding technology, a MicroChain functions as a child blockchain within the platform that operates above the global MotherChain, and is responsible for Smart Contracts management. MicroChains also enable high volume transactions using a variety of consensus systems. Consensus system is an agreement system that provides accountability and verification of transactions.

The platform’s advanced layered Multi-blockchain architecture increases overall transaction processing speeds up to 100x faster (TPS) than existing blockchain platforms. Meanwhile, MicroChains enhance token concurrency rates up to 10,000 times, for a truly scalable solution.

MOAC is one of the first blockchain solutions to implement an unique MicroChain per Smart Contract, providing for efficiency and scalability beyond existing solutions. The MOAC Platform uses MicroChains to separate processing tasks and isolate blockchain functions from business logic for each individual smart contract. By providing each Smart Contract with its own unique MicroChain, it enables Smart Contracts to use a variety of consensus protocols and results in a wider range of potential business logic use cases.

Developers have the freedom to select the consensus protocol that best fits their use case and determine the number of nodes allocated to a specific Smart Contract. All the states of the Smart Contract are saved inside the local MicroChain™ and can write data to the MotherChain™ as needed for finality.

MicroChains significantly reduce the cost of smart contract operations and allow developers to rapidly test different application and service ideas. MOAC’s MicroChains are able to interconnect with all other non- MOAC blockchains using Cross-Chain capabilities. This allows both users and their decentralized applications (DApps) to migrate easily to the MOAC Platform, with no prior blockchain knowledge. It also provides a decentralized file storage solution (IPFS) which is currently missing from other major blockchains.

母链账户地址和子链账户地址是同一体系，可以通用，但在在不同的链上为不同链的货币进行服务，互相之间除非充提否则没有联系。

母链上可以跑多条子链，墨客采用分片技术，随机将scs分配给不同的子链。

子链需要依赖于母链来运行，因此，运行一条子链需要M个母链节点（vnode）和N个子链节点（scs）。

普通子链节点scs可以称作子链矿工，其主要负责子链的出块，是维持一条子链稳定安全运行的根本。

子链节点下的母链节点，需要选取一部分注册成vnode代理的节点，vnode代理节点的作用是用于维护子链稳定运行，vnode代理节点同样有一个代理矿工池

部署子链控制合约时需要指定以上两个池子的地址。

子链合约部署完后，即可调用registeropen来召唤池子里的scs注册；当数量达到预期后，就可以调用registerclose来初始化子链区块，让子链开始运行。

子链需要每隔一段时间发起flush操作，将关键状态写入母链进行背书。除此之外，flush还将完成节点收益分配和有币区块链的母子链充提操作。

可以调用子链控制合约里的registerasmonitor来将一个子链节点注册成一个侦听节点。侦听节点只负责信息查询，不参与普通子链节点的服务，适合dapp用户部署来监控子链运行状态。

如果普通子链节点状态不稳定或弄虚作假，在flush的时候，有几率被没收押金，强制退出这条子链的服务。这时，可以调用子链控制合约的registeradd方法，将scs池子里的其他scs作为备用节点来为这条子链服务。

部署子链方可以调用子链控制合约的close方法关闭这条子链。此时会进入清算状态，待所有子链收益结清后，即关闭子链。请注意，子链业务逻辑清算不包含。

子链节点本身可以调用子链控制合约的withdraw方法来结束为某一子链服务，并得到返还的押金。


MOAC needed for a MicroChain
-------------------------------

A MicroChain requires each SCS mining nodes to deposit some MOAC before mining:

1、每个子链节点在第一次启动后，将会有一个唯一的墨客钱包地址，在部署子链前，需要在向这个地址打入至少1个MOAC作为运行费用；

2、每个子链节点注册进入子链矿工池时，需要向矿工池缴纳一定的押金，最小值由子链控制合约设置，最大值不限。子链节点每被选中一次，将会扣除一定数额的押金，当押金被扣完后，该节点将不会再参与新的子链，退出子链时，可以调用方法取回押金；

3、当一个子链节点注册成一个监听节点时，需要缴纳一定的押金；当退出子链时可以取回押金；

4、押金一般不会扣除，但在flush时，如果有企图作弊的节点，将会按照规则踢出子链，并扣除押金，不再返还；

维护子链的MOAC消耗：

首先，调用子链方法不会消耗任何gas，但是，dapp运营方需要向子链控制合约地址打入一定量的MOAC维持子链运行，这部分MOAC将会消耗在给子链矿工费用、子链向母链flush状态，以及母链充提gas返还上。这个维护消耗可以通过调整子链的flush周期来部分改变。


Key points to create a MicroChain
---------------------------------

1. The VNODE used to connect with SCSs should have good internet connection and high bandwidth. 
2. The VNODEs should be added as peer with each other to keep the communication well.
3. SCS servers need to have good time synchronization with the internet clock;
