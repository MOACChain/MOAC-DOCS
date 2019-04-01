Requirements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

VNODE
----------------------
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
				
			   

