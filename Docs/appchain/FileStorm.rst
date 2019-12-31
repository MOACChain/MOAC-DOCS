FileStorm
----------------
.. _file-storm:

FileStorm is a decentralized storage platform implemented with IPFS and MOAC. The files are stored on millions of data nodes provided by people around the world in IPFS format in exchange for MOAC. FileStorm is a project that involves three types of people.

Storage Provider Storage providers provides the hardware devices used for storage, such as computers with large volume of storage, or customized storage boxes. FileStorm program needs to be installed on these devices so they can connect to Moac FileStorm microchains as well as IPFS network. Storage providers can gain reward in MOAC by offering the storage service.

Dapp Developers Dapp Developers are the creator of FileStorm microchains. They can create a dedicated microchain used for storage only for their own Dapp. Moac Foundation will also create a public FileStorm microchain. Dapp Developers will have to pay MOAC to deploy and maintain a FileStorm.

Storage End Users Storage end users will use FileStorm through Dapps. The end users do not need MOAC to use FileStorm, but they may have to pay to use the Dapps.

Our user guide has three parts for the three type of users.

Storage Providers
Running FileStorm Subchain needs the following 4 modules:

SCSServer - Moac subchain node program.
redis - Local database to store public and private file hash
IPFS Daemon - IPFS storage server program。
IFPS Monkey - Used by Moac subchain node program to access IPFS
We can download all these modules one by ore, or install them by using Docker. Let's start with installing them one by one.

redis：

Ubuntu
.. code:: bash
    sudo apt update
    sudo apt full-upgrade
    sudo apt install build-essential tcl
    curl -O http://download.redis.io/redis-stable.tar.gz
    tar xzvf redis-stable.tar.gz
    sudo apt install redis-server

CentOs
.. code:: bash
    sudo yum install epel-release
    sudo yum update
    sudo yum install redis

FileStorm 共识
==============

FileStorm共识是在ProcWind共识之上实现的一个dPOS共识。DPOS，Delegated proof of stake，委托权益共识需要选出一组矿工节点来做区块的生产和调度。这些权益所有者需要抵押通证。任何节点如果不正常出块或者不对其他节点的出块做验证，抵押通证将被扣除。这些节点被称之为验证节点，或出块节点。验证节点按顺序轮流出块，每n秒钟一个块，n可设置，现在设置为10。FileStorm的不同之处在于，每个FileStorm子链的区块头上多了两个随机数。这两个随机数是用来做文件验证的。证明每个节点上都存了该存的文件。在FileStorm子链上，节点分成两种：出块节点和存储节点。出块节点负责打包交易，出块，并且产生文件验证随机数。存储节点存储文件，并且对文件进行验证。

出块节点被分到不同的组，每个组是一个存储的基本单位，相当于数据库里的一个Shard(分片)。每个分片里的节点存储空间一样大，存储的文件也是一样。这样一个分片里的多个节点可以互相验证。所有的文件的信息和读写交易都写在区块链上，而文件内容则存在存储节点的IPFS中。

每个存储节点都要定期对本地文件进行验证。初始设定为每40个区块。轮到验证的区块根据两个随机数产生一组文件，然后用第一个随机数找到每个文件中的一个起始位置。从这个起始位置往后数256个字节，得到一组随机的字符串组。然后对这组字符串进行哈希，直到得出一个根哈希值。同时，同分片里的其他节点也要做同样的操作。得到哈希值。被验证节点先将自己的哈希值以交易的方式发到链上。同分片的其他节点收到交易后，跟自己生成的根哈希进行比较，然后将比较结果也以交易的方式发出来。最后进行统计，如果超过半数的节点给出的哈希值跟被验证节点相同。我们认为被验证节点存了所有的文件，我们就会给它奖励。

每个存储节点都会轮流被验证，验证通过得到奖励。文件验证会在每个分片里进行。

用户文件存到链上，会根据一定的算法平均的分配到不同的分片上。

FileStorm 验证节点
====================

验证节点主要安装下面这个程序来产生和验证区块。并且获取区块奖励。

SCSServer - 墨客子链节点程序。

FileStorm 存储节点
存储节点主要安装下面这些程序来对存储的文件进行验证。验证成功即可获取存储收益。

StormCatcher - 文件管理助手，用于对文件读写删除和验证的操作。
IPFS Daemon - 文件以IPFS的方式存储的主要平台。
redis - 本地数据库，用于缓存文件调用的请求。
IPFS Daemon由IPFS源代码生成，没有改动。所以FileStorm子链是一种开放式架构，可以和其他基于IPFS的存储设备兼容。

FileStorm Token
====================

FileStorm Token (FST) is a ERC20 token issued on the MOAC BaseChain. 是发行在墨客区块链上的一个ERC20代币。在FileStorm平台上流通。用户通过支付FST获取平台的使用权，存储提供方提供存储设备得到FST收益。

FileStorm Setup
====================







