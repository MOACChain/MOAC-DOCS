FileStorm User Guide
********************


Introduction
============

FileStorm is a decentralized storage platform implemented with IPFS and
MOAC. The files are stored on millions of data nodes provided by people
around the world in IPFS format in exchange for MOAC. FileStorm is a
project that involves three types of users.

1. Storage Provider: 
   Provides the hardware devices used
   for storage, such as computers with large volume of storage, or
   customized storage boxes. FileStorm program needs to be installed on
   these devices so they can connect to Moac FileStorm microchains as
   well as IPFS network. Storage providers can gain reward in MOAC by
   offering the storage service.

2. Dapp Developers: The creator and owner of FileStorm
   microchains. They can create a dedicated microchain used for storage
   only for their own Dapp. Moac Foundation will also create a public
   FileStorm microchain. Dapp Developers will have to pay MOAC to deploy
   and maintain a FileStorm.

3. Storage End Users:  The users will use FileStorm through
   Dapps. The end users do not need any MOAC to use FileStorm, but they may
   have to pay to use the Dapps.


User Roles
==========

Storage Providers
-----------------

Storage providers are the people who run the FileStore MicroChain as the service.

FileStorm MicroChain needs to install the following packages:

* redis        - Local database, used to save the map between public HASH and private HASH;
* IPFS Daemon  - IPFS platform to save the data files;
* SCSServer    - MOAC MicroChain server;
* stormcatcher - Program calls the IPFS program from MOAC MicroChain server;

These packages can be installed separately or through Docker.


Installation
^^^^^^^^^^^^

redis：

Ubuntu

::

    sudo apt update
    sudo apt full-upgrade
    sudo apt install build-essential tcl
    curl -O http://download.redis.io/redis-stable.tar.gz
    tar xzvf redis-stable.tar.gz
    sudo apt install redis-server

CentOs

::
    sudo yum install epel-release
    sudo yum update
    sudo yum install redis

ipfs:

IPFS can be downloaded from \ `IPFS <https://dist.ipfs.io/#go-ipfs>`__\ 

Ubuntu

::

    wget https://dist.ipfs.io/go-ipfs/v0.4.17/go-ipfs_v0.4.17_linux-amd64.tar.gz
    tar xvfz go-ipfs_v0.4.17_linux-amd64.tar.gz
    sudo mv go-ipfs/ipfs /usr/local/bin/ipfs


::
    sudo yum install epel-release
    sudo yum update
    sudo yum install redis

scsserver:

The newest version can be download from \ `release link <https://github.com/MOACChain/FileStorm/releases>`__\.

scsserver
stormcatcher
userconfig.json
run_filestorm_scs.sh
stop_filestorm_scs.sh

Then you can execute filestorm_install.sh to install filestorm scs.

Configuration
^^^^^^^^^^^^^

Configuration file: userconfig.json

VnodeServiceCfg can be setup using the Vnode/Port information from  
`Node info -
Testnet <https://nodes101.moac.io/>`__\  Protocol
Pool.
Beneficiary is the public wallet address of SCS owner.

The VNODE server need to open the following ports for outside access: 
* 4001;
* 5001;
* 8080;

Running
^^^^^^^

IPFS file system needs to be initialized before the first usage:

::

    ipfs init

Then the user can run FileStorm MicroChain using the start script:

::

    cd scsserver
    ./run_filestorm_scs.sh

The start script calls the four components in the FileStorm:

::

    #!/bin/bash  
    echo "Starting FileStorm SCS--"

    nohup ipfs daemon > ipfs.out 2>&1 &
    echo "IPFS Daemon started."

    nohup redis-server > ipfs.out 2>&1 &
    echo "Redis Server started. "

    nohup ./ipfs_monkey > ipfs.out 2>&1 &
    echo "IPFS Monkey started."

    nohup ./scsserver > scs.out 2>&1 &
    echo "SCS started."

To stop the service, call the following script:

``./stop_filestorm_scs.sh``

The closing script closed the four components:

::

    #!/bin/bash  
    echo "Stoping FileStorm SCS--"
    pkill ipfs_monkey
    pkill redis-server
    pkill ipfs
    pkill scsserver
    echo "FileStorm SCS Stopped."

*To make the SCSs be able to work with MicroChain, 0.5 moac depsit needs to be added to each SCS's address.*

Monitring:

To check if the MOAC MicroChain running, check the log file: 

``tail -f scs.out``

DAPP Developers
^^^^^^^^^^^^^^^

DAPP developers deploy the DAPP on the FileStorm MicroChain to let the Storage Users access the data through the DAPP.

To develop a DAPP on the FileStorm platform, you need to:
1. Run a vnode locally to connect to the MOAC mainnet(or testnet for testing). The newest released version is under: 
   https://github.com/MOACChain/moac-core/releases 
2. Start the vnode:
   To connect with mainnet: ``./moac --rpc --rpccorsdomain "http://wallet.moac.io" console``
   To connect with testnet: ``./moac --testnet --rpc --rpccorsdomain "http://wallet.moac.io" console``
3. Use `MOAC wallet <https://wallet.moac.io>`__\ to deploy the MicroChain;
4.  DeploySubChainBase.sol
5. Find `Node info - Testnet <https://nodes101.moac.io/>`__\ 
   SubChainProtocolBase pool地址和 Vnodeproxy pool地址
6. Use MOAC wallet to deploy the FileStormMicroChain.sol;
7. Register the MicroChain;
8. Check the status of MicroChain with MicroChain explorer.

Storage Users
^^^^^^^^^^^^^

Strage users access the data on the IPFS through DAPP deployed on the FileStorm. 


Example:

The following procedures show how to access a data file on the FileStorm testnet.

1. Setup a local VNODE server. The software can be downloaded from 
   https://github.com/MOACChain/moac-core/releases;
2. Running the VNODE: ``./moac --testnet console``;
3. Setup IPFS \ `download <https://dist.ipfs.io/#go-ipfs>`__\;
  For ubuntu:

   ::

       wget https://dist.ipfs.io/go-ipfs/v0.4.17/go-ipfs_v0.4.17_linux-amd64.tar.gz
       tar xvfz go-ipfs_v0.4.17_linux-amd64.tar.gz
       sudo mv go-ipfs/ipfs /usr/local/bin/ipfs

4. Generate a local text file for uploading:\ ``vi newtestfile.txt``
5. Add the generated file to the IPFS system：\ ``ifps add newtestfile.txt``
6. Convert the hash to HEX code, which can be done using this web tool：https://codebeautify.org/string-hex-converter.
   or using the NODEJS tool: `` npm install --save ethereumjs-abi ``

    ::

    var abi = require('ethereumjs-abi'); var original =
    'QmQNe96LqV5TcRQyBz12iQXPZQjemBqkgnpHki3wmKjtd6'; var encoded =
    abi.simpleEncode('write(string)', original);

    console.log('original', original);  console.log('encoded',
    encoded.toString('hex'));


    7. The HEX code of the HASH should be a HEX code with length 46, total 92 digits. Since the storage of parameters in Solidity only has 32 digits, we used two parameters to store the HEX code of the HASH. The HEX code of the HASH is filled with 0s to make two HEX codes with 64 digits;
    8. Call the three functions to read, write and delete the data on the MicroChain:
    9. from: need to be an unlocked account;
    10. to: DAPP address provided by the DAPP developer or the Storage Provider;
    11. data: Add the HEX code from Step 7 after '2e';
    12. After each successful call, the nonce need to increase by 1 for the next call.
    13. via needs to set as the same value of via in the vnodeproxy.json in the VNODE directory.
        

// write(fileHash) chain3.mc.sendTransaction( { from:
chain3.mc.accounts[0], value:chain3.toSha('0','mc'), to:
subchainbaseaddress, gas: "200000", gasPrice: chain3.mc.gasPrice,
shardingflag: 1, data:
'0xba3835ba00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002e'
nonce: 1, via: chain3.mc.accounts[0] });

// read(fileHash) chain3.mc.sendTransaction( { from: mc.accounts[0],
value:chain3.toSha('0','mc'), to: subchainbaseaddress, gas: "200000",
gasPrice: chain3.mc.gasPrice, shardingflag: 1, data:
'0x616ffe830000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000002e'
nonce: 2, via: mc.accounts[0] });

// remove(fileHash) chain3.mc.sendTransaction( { from: mc.accounts[0],
value:chain3.toSha('0','mc'), to: subchainbaseaddress, gas: "200000",
gasPrice: chain3.mc.gasPrice, shardingflag: 1, data:
'0x80599e4b0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000002e'
nonce: 3, via: mc.accounts[0] }); \`\`\`

Results：

Write：a file was written to FileStorm MicroChain's nodes with a HASH;

Read：A file will be read from FileStorm nodes;
Remove：The data file will be removed from every FileStorm node;
