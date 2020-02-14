.. _scs-pool:

SCS pool
^^^^^^^^^
		
Deploy the SCS pool contract
------------------------------

To create a SCS pool for the AppChain, user need to deploy the SCSProtocolBase.sol (or called SubChainProtocolBase.sol in old version) on the BaseChain.

Consensus:POR  
Minimum deposit: 2 mc 

First make sure the Node.Js is installed, please refer to https://nodejs.org/en
Example commands under Node.js
::		     
	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> solfile = 'SubChainProtocolBase.sol';
	> contract = fs.readFileSync(solfile, 'utf8');
	> output = solc.compile(contract, 1);                     
	> abi = output.contracts[':SubChainProtocolBase'].interface;
	> bin = output.contracts[':SubChainProtocolBase'].bytecode;
	> subchainprotocolbaseContract = chain3.mc.contract(JSON.parse(abi));
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> subchainprotocolbase = subchainprotocolbaseContract.new( "POR",  2, { 
	from: chain3.mc.accounts[0],  data: '0x' + bin,  gas: '5000000'});
	> chain3.mc.getTransactionReceipt(subchainprotocolbase.transactionHash).contractAddress
	
After successfully deploy, a valid contract address should be returned, such as:  0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac


SCS setup
----------

The example used three SCSs and used the following info in the userconfig.json.
VnodeServiceCfg is the vnode address, different SCSs can use the same VNODE as proxy to BaseChain;
Beneficiary address the SCS used 
::
	VnodeServiceCfg is the vnode address, different SCSs can use the same VNODE as proxy to BaseChain: 192.168.10.209:50062
	Beneficiary is : 
		0xa934198916cd993c73c1aa6e0c0e7b21ce7c735b 
		0x2e7c076dbf6e61207a0ddb1b942ef7da8fd139f0
		0xea1a118e94344be69f02753d1e6f7fe19dda89ac
		
After setup the correct parameters, start the SCS:

scsserver-windows-4.0-amd64 --password "123456"   （default scs keystore password）
		
The address of the SCS can be found in the keystore file under the scskeystore directory.  
::
	0xd4057328a35f34507dbcd295d43ed0cccf9c368a 
	0x3e21ba36b396936c6cc9adc3674655b912e5fa54
	0x03c74ecc8ad9493a6a3d14f4e48d5eb551fe1be5

Transfer some moac to SCS address to enable SCS pay the fee to join the SCS pool and AppChain:
::		
	> amount = 20;
	> scsaddr = '0xd4057328a35f34507dbcd295d43ed0cccf9c368a';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: scsaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: ''});
	> scsaddr = '0x3e21ba36b396936c6cc9adc3674655b912e5fa54';
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: scsaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: ''});
	> scsaddr = '0x03c74ecc8ad9493a6a3d14f4e48d5eb551fe1be5';
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: scsaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: ''});
	
Check the balances on the SCS accounts:

::		
	> chain3.mc.getBalance('0xd4057328a35f34507dbcd295d43ed0cccf9c368a')
	> chain3.mc.getBalance('0x3e21ba36b396936c6cc9adc3674655b912e5fa54')
	> chain3.mc.getBalance('0x03c74ecc8ad9493a6a3d14f4e48d5eb551fe1be5')
	
SCS join SCS pool
----------------------

After the SCS pool contract is created, SCS need to call the register method to join the SCS pool. SCS can only be selected by the AppChain if it joins the SCS pool that the AppChain used.
			
参数:
::
	from： An Account has some moac balance larger than the deposit;    
	value：value in moac, needs to be larger than the deposit value required by the SCS pool contract; 
	to: SCS pool address; 
	data: register(SCS address) 
	
The data is formed by the following method:	
::	
	Method ABI chain3.sha3("register(address)") = 0x4420e4869750c98a56ac621854d2d00e598698ac87193cdfcbb6ed1164e9cbcd 
    Use the first 4 bytes: 0x4420e486  
	The SCS address is passed after the 4 bytes:
	    0xd4057328a35f34507dbcd295d43ed0cccf9c368a
	=>	000000000000000000000000d4057328a35f34507dbcd295d43ed0cccf9c368a  (fill 24 0s to make the 32 bytes）
	data = '0x4420e486000000000000000000000000d4057328a35f34507dbcd295d43ed0cccf9c368a'		

Example:
::
	> amount = chain3.toSha(5,'mc')
	> data = '0x4420e486000000000000000000000000d4057328a35f34507dbcd295d43ed0cccf9c368a';
	> chain3.mc.sendTransaction({ from: chain3.mc.accounts[0], value:amount, to: '0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac', gas: "5000000", gasPrice: chain3.mc.gasPrice, data: data });
	
Call the scs pool contract method scsCount to check the number of SCS in the pool:
::		
	> subchainprotocolbase.scsCount()

Repeat the steps to join the other scs in the pool.

.. _scs-join-appchain:

Add SCS client after AppChain is running
----------------------------------------

If the AppChain contract provides the method 'registerAdd', then the owner of the AppChain can add a SCS address by calling 'registerAdd'. 

First need to make sure the SCS address is already registered in the SCS pool.
Then the AppChain owner calls the 'registerAdd' method using direct call.
When AppChain receives the request, it will choose the SCS address and syncing the blocks automatically.
After one flush round, the SCS will join the AppChain.

registerAdd参数:
::
	nodeToAdd： 当前scs数+需要加入scs数

调用示例:
::	
	> data = subchainbase.registerAdd.getData(20)
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: data});

验证：scs对应日志开始同步区块，合约公共变量nodeCount更新为scs最新数量
::		
	> SubChainBase.nodeCount()
	
.. _scs-exit-appchain:

SCS exit AppChain
-----------------

SCS节点退出应用链有两种方式：

1. 当应用链工作正常时，调用子类合约requestRelease方法请求退出应用链，等待一轮flush后生效。

requestRelease参数:
::
	senderType：	1：scs发起请求       2：收益账号发出请求
	index： 		scs序号（参考ScsRPCMethod.GetSubChainInfo中scs的列表）

调用示例（在NODEJS 交互环境下）:	
::	
	> data = subchainbase.requestRelease.getData(senderType, index)
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: data});
	
验证：等待一轮flush后，关注合约公共变量nodeCount是否变化
::		
	> SubChainBase.nodeCount()

	
2. 当应用链工作不正常时，可以调用子类合约requestReleaseImmediate方法请求立即退出应用链。

requestReleaseImmediate参数:
::
	senderType：	1：scs发起请求       2：收益账号发出请求
	index： 		scs序号（参考ScsRPCMethod.GetSubChainInfo中scs的列表）

调用示例:	
::	
	> data = subchainbase.requestReleaseImmediate.getData(senderType, index)
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: data});
	
验证：合约公共变量nodeCount是否变化
::		
	> SubChainBase.nodeCount()

.. _scs-monitor:

SCS Monitor
------------

SCS can also join the AppChain as a monitor. A monitor doesn't join the consensus procedure in the AppChain but validate the block content but 是一种特殊的应用链SCS节点，其主要可以用于监控应用链的状态和数据。

Monitor不参与应用链的交易共识，只是同步区块数据，提供数据查询

应用链启动的方式与scs区别在于参数不同，主要定义了rpc接口的访问控制
::	
	scsserver-windows-4.0-amd64 --password "123456" --rpcdebug --rpcaddr 0.0.0.0 --rpcport 2345 --rpccorsdomain "*"

应用链运行后，Monitor可以调用应用链控制合约subchainbase中的registerAsMonitor方法进行注册

registerAsMonitor :	
::	
	> data = subchainbase.registerAsMonitor.getData('0xd135afa5c8d96ba11c40cf0b52952d54bce57363','127.0.0.1:2345')   
	

Example:
::
	> subchainbase = SubChainBaseContract.at('0xb877bf4e4cc94fd9168313e00047b77217760930')
	> amount = chain3.toSha(1,'mc')
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> data = subchainbase.registerAsMonitor.getData('0xd135afa5c8d96ba11c40cf0b52952d54bce57363','127.0.0.1:2345')
	> chain3.mc.sendTransaction({ from: chain3.mc.accounts[0], value:amount, to: subchainaddr, gas: "5000000", gasPrice: chain3.mc.gasPrice, data: data });

If the SCS monitor concole outputs the blocks information of the AppChain, it means the AppChain starts successfully. Or you can call the getMonitorInfo Method in the AppChain to check the information:
::
	> subchainbase = SubChainBaseContract.at('0xb877bf4e4cc94fd9168313e00047b77217760930')	
	> subchainbase.getMonitorInfo.call()

