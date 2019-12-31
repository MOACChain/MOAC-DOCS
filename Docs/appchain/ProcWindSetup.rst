.. _proc-wind-setup:

ProcWind Setup
--------------------

VNODE
=======
MOAC VNODE can be downloaded from: https://github.com/MOACChain/moac-core/releases/

testnet explorer： http://testnet.moac.io/home

Start a VNODE connecting with testnet： 
::
	moac-windows-4.0-amd64.exe --testnet --rpc --rpcapi "chain3,mc,net,db,personal,admin,miner"


Wait until the 运行concole命令 mc.blockNumber 检查是否同步到最新区块

	
SCS
=============

ProcWind 应用链节点  版本来源: https://github.com/MOACChain/moac-core/releases/

userconfig.json配置：
::
	VnodeServiceCfg为代理vnode地址: 192.168.10.209:50062  （对应上面部署的vnode的IP）
	Beneficiary为收益账号
	
启动节点： 
	scsserver-windows-4.0-amd64 --password "123456"   （生成scs keystore的密码）
	
验证： 
::
	scs启动后，并开始从vnode同步块号信息。
	在scskeystore目录内生成的keystore文件中生成scs账号的scskeystore文件。
	
	   
Accounts
========

可以运行console命令:
personal.newAccount() 创建账号； 
mc.accounts查看账号； 

按序号查询余额：mc.getBalance(mc.accounts[0])   

测试环境的公共提币地址：https://faucet.moacchina.com

注意：后续消耗gas的操作都需要执行personal.unlockAccount(mc.accounts[0]) 对应账号进行解锁				

准备账号列表：（示例地址参考后续的命令操作）	
::	
	应用链操作账号：进行创建合约，发起交易等基本操作： 0x87e369172af1e817ebd8d63bcd9f685a513a6736 
	主链vnode收益账号：	0xf103bc1c054babcecd13e7ac1cf34f029647b08c 
	应用链scs收益账号：0xa934198916cd993c73c1aa6e0c0e7b21ce7c735b  0x2e7c076dbf6e61207a0ddb1b942ef7da8fd139f0
	

Node.js software and chain3 lib
================================

				
注意，在windows下，可能还要安装辅助工具 Python 2.X 和 VS2013/2015

1.安装Python 2.X，在这里我安装的是Python 2.7，不能用3及以上版本，安装好了设置环境变量；

2.安装VS2013 或者 VS2015;

3.安装NodeJS及npm工具;

4.安装 chain3 软件库
::
npm install chain3  

验证:  
::
	> chain3 = require('chain3'); 
	> chain3 = new chain3(); 
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545')); 
	> chain3.mc.blockNumber  检查是否获得当前区块 

此外，目前应用链的合约编译仅支持solc 0.4.24版本，需要在工程目录下执行下面命令，更换solc版本
::
npm uninstall solc
npm install solc@0.4.24


部署VNODE节点池合约
==================

请参考 :ref:`VNODE 节点池部署<vnode-pool>` ，并记录VNODE节点池合约的地址，如果加入现成的VNODE节点池，则可以跳过此步骤。


部署SCS节点池合约
=================

请参考 :ref:`SCS 节点池部署<scs-pool>` ，并记录SCS节点池合约的地址，如果加入现成的SCS节点池，则可以跳过此步骤。


部署应用链合约  
=============

根据需要使用的合约类型，确定好合约文件。目前主要有ASM和AST两种类型的ProcWind。最新的合约可以从
 `MOAC 开源地址 <https://github.com/MOACChain/moac-core/tree/master>`__ 处获取。
现在我们可以部署一个应用链合约。

部署ChainBaseASM.sol示例，首先运行Node.js，在node命令行下:
::

	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> input = {'': fs.readFileSync('ChainBaseASM.sol', 'utf8'), 'SubChainProtocolBase.sol': fs.readFileSync('SubChainProtocolBase.sol', 'utf8')};
	> output = solc.compile({sources: input}, 1);			
	> abi = output.contracts[':SubChainBase'].interface;
	> bin = output.contracts[':SubChainBase'].bytecode;
	> proto = '0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac' ;    // 应用链节点池合约 
	> vnodeProtocolBaseAddr = '0x22f141dcc59850707708bc90e256318a5fe0b928' ;       // Vnode节点池合约 
	> min = 1 ;			// 应用链需要SCS的最小数量，当前需要从如下值中选择：1，3，5，7
	> max = 11;		// 应用链需要SCS的最大数量，当前需要从如下值中选择：11，21，31，51，99
	> thousandth = 1 ;			// 千分之几，控制选择scs的概率，对于大型应用链节点池才有效
	> flushRound = 40 ;     	// 应用链刷新周期  单位是主链block生成对应数量的时间，当前的取值范围是40-99
	> SubChainBaseContract = chain3.mc.contract(JSON.parse(abi));  
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> SubChainBase = SubChainBaseContract.new( proto, vnodeProtocolBaseAddr, min, max, thousandth, flushRound,{ from: chain3.mc.accounts[0],  data: '0x' + bin,  gas:'9000000'} , function (e, contract){console.log('Contract address: ' + contract.address + ' transactionHash: ' + contract.transactionHash); });
	
		
部署完毕后, 获得应用链合约地址  0x1195cd9769692a69220312e95192e0dcb6a4ec09
		

	
应用链开放注册
=============

首先应用链合约需要最终提供gas费给scs，需要给应用链控制合约发送一定量的moac，调用合约里的函数addFund
::	
	根据ABI chain3.sha3("addFund()") = 0xa2f09dfa891d1ba530cdf00c7c12ddd9f6e625e5368fff9cdf23c9dc0ad433b1
		取前4个字节 0xa2f09dfa 
	> amount = 20;
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0xa2f09dfa'});

可以通过查询余额进行验证  
::		
	> chain3.mc.getBalance('0x1195cd9769692a69220312e95192e0dcb6a4ec09')
		
然后调用  调用合约里的函数registerOpen 开放注册 (按应用链节点池合约中SCS注册先后排序进行选取)
::
	根据ABI chain3.sha3("registerOpen()") = 0x5defc56ce78f178d760a165a5528a8e8974797e616a493970df1c0918c13a175
		取前4个字节 0x5defc56c 
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0x5defc56c'});				

	
验证：  等待scs注册 (vnode 一个 flush周期后 ) ， 可不断访问应用链合约的 nodeCount，等待3个scs注册完成
::
	> SubChainBase.nodeCount()
	> chain3.mc.getStorageAt(subchainaddr,0x0e)  // 注意nodeCount变量在合约中变量定义的位置（16进制）

应用链关闭注册
=============

等到两个scs都注册完毕后，即注册SCS数目大于等于应用链要求的最小数目时，调用应用链合约里的函数 registerClose关闭注册：

::
	根据ABI chain3.sha3("registerClose()") = 0x69f3576fc10c82561bd84b0045ee48d80d59a866174f2513fdef43d65702bf70
		取前4个字节 0x69f3576f 
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0x69f3576f'});
			
验证：  SCS自身完成初始化并开始应用链运行，可观察scs的concole界面，scs开始出块即成功完成部署应用链。

SCS join/exit AppChain
======================

After the AppChain started, SCS can join and 节点或者去除SCS节点，也可以加入监听节点，具体可以参考：

:ref:`SCS join AppChain <scs-join-appchain>` 

:ref:`SCS exit AppChain <scs-exit-appchain>` 

:ref:`SCS work as monitor <scs-monitor>` 

Shut Down
=============

The AppChain is shut down when the AppChain account has no mc balance to pay the SCS miners. The final state of the AppChain will be kept at the last flush to the MotherChain. The AppChain will be resumed after enough mc is paid to the AppChain account.

The AppChain can also be closed if the owner of the AppChain called the 'close' method in the AppChain contract.

Example:
::	
	ABI chain3.sha3("close()") = 0x43d726d69bfad97630bc12e80b1a43c44fecfddf089a314709482b2b0132f662
	Use the first 4 bytes: 0x43d726d6 
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0x43d726d6'});

After send out the request, the AppChain will be closed after the next flush is over. The mining fee and the deposit of the SCS will also be paid after the flush. The AppChain will be be resumed after this method is called. 


::		
	> chain3.mc.getBalance('0x1195cd9769692a69220312e95192e0dcb6a4ec09')
