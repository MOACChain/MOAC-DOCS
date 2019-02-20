MicroChain SCS Mining
---------------------

Please follow these steps to run a SCS and start your MicroChain mining:

A1、Download SCS program ( Or power up a SCS hardware miner)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The initial SCS program has following files: README - A txt file
contains instructions; config/userconfig.json - Configura file used with
SCS, need to be customized before start SCS; scsserver/scsserver -
Executable program of SCS

A2、Cusomize userconfig.json
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The conten of the userconfig.json is as following:

1. VnodeServiceCfg：The VNODE IP and port SCS connects to. Each SCS
   needs to connect a VNODE to communicate with the MotherChain. You can
   setup a local VNODE or use a truste VNODE from other sources;
2. DataDir：SCS data directory that holding the MicroChain data. Default
   is "./scsdata";
3. LogPath：SCS log directory, default is "./logs";
4. Beneficiary：Account holds the MicroChain mining rewards. Please
   don't use the SCS account for this. We suggest you create this
   account separatly and don't put the keystore file on SCS.
5. VnodeChainId：network id with the MotherChain. Testnet = 101 and
   mainnet = 99. If you have a custome network, you need to make sure
   the vnode connect with has the same network id.
6. Capability: desposit limit for a MicroChain. Since each MicroChain
   requires some desposit to join, you can set this number and only join
   the MicroChain with deposit requirement less than this limit.
7. ReconnectInterval: If the connection is lost with vnode, SCS will try
   to connect with the vnode again. This is the number of seconds
   between each connection with vnode.

A3、Start SCS
~~~~~~~~~~~~~

Command options (SCS -h)

::

    -p [psd]           Start SCS with a password for the scsid keystore，default password is "moacscsofflineaccountpwd"
    -rpcaddr [addr]    SCS start with rpc ip
    -rpcport [port]    SCS start with rpc port

After the first start，SCS generates a keystore with a password (default
or provided by user). The address of this keystore is the scsid. This
address won't receive rewards. If user want to use a different scsid,
should remove the keysore file and restart the SCS.

SCS also has a rpc port. Currently the RPC only has monitoring services
for DAPP developers.

A4、Register the SCS into a SCS pool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To participate in the MicroChain, SCS need to register itself into a SCS
pool. When it registers, the SCS need to pay deposit as required by the
SCS pool (defined in subchainprotocolbase.sol). After it is registered,
it need to wait for some blocks (default is 50 blocks) to be chosen by
MicroChains.

One SCS can register multiple MicroChains.

Javascript method to register the SCS: (used under VNODE console)

.. code:: javascript

    function protocolRegister(baseAddr,basePasswd,protocolAddr,scsAddr)
    {
        chain3.personal.unlockAccount(baseAddr, basePasswd,0);
        sendtx(baseAddr, protocolAddr, '10','0x4420e486000000000000000000000000' + scsAddr);
    }

Explainations

-  baseAddr、basePasswd：MOAC VNODE account used to send TX;
-  protocolAddr：The subchainprotocolbase contract address;
-  scsAddr：scsid address, saved in "…/scsserver/scskeystore";
-  deposit: amount of MOAC to send to pools to join MicroChains;
-  data：‘0x4420e486’ is a constant used to call the MicroChain. It was
   from the subchainprotocolbase function ‘register(address scs)’ .
   Don't change it unless you know what you are doing here!

Examples:

register SCS

.. code:: javascript

    protocolRegister ('0x1b9ce7e4f1.......e38913a56cd986786',
    ‘123’,
    '0x09f0dfc09b......0b85e5189a7493671',
    'f687272ae00f8cea......37dd9be30329d8cf')//without prefix '0x'

Send a transaction

.. code:: javascript

    function sendtx(src, tgtaddr, amount, strData) {
        chain3.mc.sendTransaction(
            {
                from: src,
                value:chain3.toSha(amount,'mc'),
                to: tgtaddr,
                gas: "9000000",
                gasPrice: chain3.mc.gasPrice,
                data: strData
            });
            
        console.log('sending from:' +   src + ' to:' + tgtaddr  + ' with data:' + strData);
    }

OK, now SCS miner finished setup and you can sit back and wait for your
rewards. All the rewards can be seen after a MicroChain flushed its data
into the MotherChain and you can see the balances changes in the
Beneficiary account address.

FAQ:

1. What's the deposit for?

The process for a SCS node to join a microChain is: make a safety
deposit and register in the SCS pool. The amount of the deposit is a
parameter that can be set in the microChain protocol. SCS cannot choose
the microChain by itself.

The microChain will choose in the SCS pool to form the microChain
validators. By default, this process is random. The microChain creator
can also change the selection process and only allow specific SCSs to
join. When microChain generate a new block, if a SCS made bad decision,
it will be punished with penalty of the deposit. The microChain will
drop a SCS if it made many bad decisions.

2. How secure are microchains against 51% attack? Or are there different
   security measures applied on microchain level?

   Generally there are two ways to prevent inside attackers in a public
   microChain. First, all the SCS join the SCS pool need to pay some
   deposits and will be kicked out of the microChain if it made enough
   bad decisions. This can cost the attacker more than they can earn in
   a public microChain. Second, the microChain was formed by randomly
   choosing SCSs from the SCS pool. Thus, it is very hard for the
   attacker to get enough SCSs to do the 51% attack (33% for PBFT). For
   a SCSs pool with 100 nodes, the attackers may need 51 nodes to
   perform the 51% attack for a microchip with only 20 nodes.
