Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

VNODE
----------------------

In MOAC system，VNODE-PROXY(VP) is used to provide MicroChain register and data services. VP is a VNODE that can get rewards from MicroChains without mining. You just need to set the VnodeBeneficialAddress in the vnodeconfig.json.

To provide MicroChain register service, VP need to publish its VnodeBeneficialAddress to the MicroChain users.

To provide data service to MicroChains, VP need to register in the Proxy contract to be selected:

Release packages can be found from: https://github.com/MOACChain/moac-core/releases/

The example is using nuwa 1.0.8 for testnet only.
The testnet explorer is at： http://testnet.moac.io/home

Windows:

Start a VNODE connecting with testnet： 
	moac-windows-4.0-amd64.exe --testnet --rpc --rpcapi "chain3,mc,net,db,personal,admin,miner"

Attach to the JS console： 
::
	moac-windows-4.0-amd64.exe attach \\.\pipe\moac.ipc  

You need to wait until the node is synced with the net you connect.
You can check using the concole command:
::
	> mc.syncing
	   
Accounts with enough moac
--------------------------------	
Under the concole, create an account if you don't have one, make sure you have enough balances:
::
	> personal.newAccount() 
	> mc.accounts
	> mc.getBalance(mc.accounts[0]) 

You can get testnet moac from the faucet：https://faucet.moacchina.com

or contact the team.

You need to have the password for the account and unlock it during the deployment:
::
	> personal.unlockAccount(mc.accounts[0])		

List of accounts that you may needed in the deployment：	
::	
	Creator address：to deploy the MicroChain on the MotherChain： 0x87e369172af1e817ebd8d63bcd9f685a513a6736 
	Vnode Beneficial Address：	0xf103bc1c054babcecd13e7ac1cf34f029647b08c 
	SCS Beneficiary Address for each SCS:0xa934198916cd993c73c1aa6e0c0e7b21ce7c735b  0x2e7c076dbf6e61207a0ddb1b942ef7da8fd139f0
	
chain3 nodejs lib
----------------------	
Setup:
 npm install chain3  

Test program:  
::
	> chain3 = require('chain3'); 
	> chain3 = new chain3(); 
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545')); 
	> chain3.mc.blockNumber  //Check if it matches with the most recent block 
				
			   
SCS
----------------------

To do SCS mining, a SCS miner needs to deposit some MOAC to join a MicroChain mining pool.
When a SCS was chosen by a MicroChain and start mining, it can receive mining rewards from the MicroChain as long as the MicroChain is running. The rewards was given as MOAC and shows up after a flush of the MicroChain. The deposit will be
 deducted if the SCS keeps sending dishonest transactions or lost
 connections very often.Please follow these steps to run a SCS and start your MicroChain mining:

Download SCS program ( Or power up a SCS hardware miner)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The initial SCS program has following files: 
1. scsserver: the executable file for SCS server; 
2. config/userconfig.json - Configuration file used with SCS, need to be customized before start SCS;

Cusomize userconfig.json
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The conten of the userconfig.json is as following:

1. VnodeServiceCfg：The VNODE IP and port SCS connects to. Each SCS
   needs to connect a VNODE to communicate with the MotherChain. You can
   setup a local VNODE or use a truste VNODE from other sources;
2. DataDir：SCS data directory that holding the MicroChain data. Default
   is "./scsdata";
3. LogPath：SCS log directory, default is "./_logs";
4. Beneficiary：Account holds the MicroChain mining rewards. Please
   don't use the SCS account for this. We suggest you create this
   account separatly and don't put the keystore file on SCS.
5. VnodeChainId：network id with the MotherChain. Testnet = 101 and
   mainnet = 99. If you have a custome network, you need to make sure
   the vnode connect with has the same network id.
6. LogLevel：Logging verbosity, 0=silent, 1=error, 2=warn, 3=info, 4=debug (default: 4)
7. Capability: desposit limit for a MicroChain. Since each MicroChain
   requires some desposit to join, you can set this number and only join
   the MicroChain with deposit requirement less than this limit.
8. ReconnectInterval: If the connection is lost with vnode, SCS will try
   to connect with the vnode again. This is the number of seconds
   between each connection with vnode.

Start SCS
~~~~~~~~~

Command options (SCS -h)

::

    -p [psd]           Start SCS with a password for the scsid keystore，default password is "moacscsofflineaccountpwd"
    -rpc               Enable the HTTP-RPC server with JSON-RPC format methods
    -rpcdebug          Enable the HTTP-RPC server with debug format methods       
    -rpcaddr [addr]    HTTP-RPC server listening interface (default: "localhost")
    -rpcport [port]    HTTP-RPC server listening port (default: 8548)

After the first start，SCS generates a keystore with a password (default
or provided by user). The address of this keystore is the scsid. This
address won't receive rewards. If user want to use a different scsid,
should remove the keysore file and restart the SCS.

SCS has a rpc port. Currently the RPC only has monitoring services
for DAPP developers.

Register the SCS into a SCS pool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To participate in the MicroChain, SCS need to register itself into a SCS
pool. A SCS pool is a Global Contract (usually named subchainprotocolbase.sol) deployed on MOAC MotherChain. 
When it registers, the SCS need to pay deposit as required by the
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
-  protocolAddr：The subchainprotocolbase contract address, or the SCS pool address;
-  scsAddr：scsid address, saved in "…/scsserver/scskeystore";
-  deposit: amount of MOAC to send to pools to join MicroChains, no smaller than the deposit limit required by SCS pool contract;
-  data：‘0x4420e486’ is a constant used to call the MicroChain. It was
   from the subchainprotocolbase function ‘register(address scs)’ .
   Don't change it unless you know what you are doing here!
