Deploy DApp contracts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Since nuwa 1.0.8, deploy DAPP on MicroChain changed to support multiple contracts.
First, user need to deploy a Dappbase.sol to support all other DAPP contracts.


Requirements
--------------------
One running MicroChain without any DAPP on it.
Please refer to "Create a MicroChain".

Download the control contract dappbase.sol using the latest download link.
dapp1.sol and dapp2.sol are the two 

Deploy the DAPP
------------------------------

DAPP is deployed by using sendTransaction on the MotherChain with the shardingFlag = 3.

Parameters：
::
    from: source account to deploy the DAPP;
	to: MicroChain address;
	gas: no gas needed for this operation, can be set to 0;
	shardingflag: require to set to 0x3，in nuwa 1.0.8 and later, all MicroChain deployment need set to shardingflag;
	via: VNODE proxy address, should be found in vnodeconfig.json;
	
STEP1：Deploy the dappbase.sol on MicroChain 0x1195cd9769692a69220312e95192e0dcb6a4ec09:
::
	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> solfile = 'deorder.sol';
	> contract = fs.readFileSync(solfile, 'utf8');
	> output = solc.compile(contract, 1);                    
	> abi = output.contracts[':DeOrder'].interface;
	> bin = output.contracts[':DeOrder'].bytecode;
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c';  
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction({from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas:0, shardingFlag: "0x3", data: '0x' + bin, nonce: 0, via: via, });
			
Result: 
	If MicroChain is deployed successfully, the Nonce of the source account should change to 1; 
	This can be checked using rpc command get_nonce, or rpcdebug command ScsRPCMethod.GetNonce.
	Please refer to [Interact with the DAPP]().


STEP2：Deploy the dapp1.sol using the same command as Step1;

STEP3：Deploy the dapp2.sol using the same command as Step1;
		

Interact with the DAPP
----------------------

DAPP智能合约的调用也通过主链的sendTransaction发送交易到 proxy vnode 的方式进行。

在多合约版本中，调用dapp方法前，需要先调用dappbase中的registerDapp方法来注册每一个dapp，具体方式如下：

**请注意，与母链调用不同，子链的任何调用需要在data前加上dapp的合约地址！！**

dappbase.sol有个方法 registerDapp(address,address,string)

Parameters：
::
	to: 子链控制合约subchainbase的地址
	nonce：调用monitor的rpc接口ScsRPCMethod.GetNonce获得
	gas: 0 不需要消耗gas费用
	shardingflag： 0x1  表示子链调用操作
	via: 对应 proxy vnode 的收益地址
	data: 调用合约地址 + chain3.sha3("registerDapp(address,address,string)") 取前4个字节 0xb5560a14，加上传值凑足32个字节

registerDapp中，第一个参数是想要注册的dapp的地址（dapp1和dapp2的地址），可以通过RPC getReciipt方法获得部署时contract address；第二个参数是创建dappbase时的from，也就是只有创建dappbase的人才能调用此方法；第三个参数是这个dapp的ABI。
	
Example：
::
	> nonce = 3	
	> data = '0x0xcc0D18E77748AeBe3cC6462be0EF724e391a4aDb5560a140000000000000000000000001105d71a3d23c5bc80fc4b76605d694a0f83bfab00000000000000000000000044c10f4c... ...'			
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { nonce: nonce, from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas:0, shardingFlag:'0x1', data: data, via: via,});
	
验证：
	每次操作成功后，Nonce会自动增加1
	或者直接调用monitor的rpc接口ScsRPCMethod.GetContractInfo获得合约变量的方式进行验证。

以部署dapp1和dapp2为例，需要将这两个业务逻辑合约注册到dappbase中去：

STEP4： 调用dappbase中的registerDapp方法来注册dapp1

STEP5： 调用dappbase中的registerDapp方法来注册dapp2

STEPX： 调用dapp1或dapp2中的业务逻辑
		