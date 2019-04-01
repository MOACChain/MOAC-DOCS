Create a MicroChain
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Vnode Pool
----------------------

首先部署vnode矿池合约，VnodeProtocolBase，如果加入现成的vnode矿池，则可以忽略此步骤。

加入矿池的代理Vnode节点被用于提供子链调用服务和子链历史数据中转服务的节点。

以下为nodejs部署示例：最低保证金为 2 moac 
::	
	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> solfile = 'VnodeProtocolBase.sol';
	> contract = fs.readFileSync(solfile, 'utf8');
	> output = solc.compile(contract, 1);   
	> abi = output.contracts[':VnodeProtocolBase'].interface;
	> bin = output.contracts[':VnodeProtocolBase'].bytecode;
	> VnodeProtocolBaseContract = chain3.mc.contract(JSON.parse(abi));
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> VnodeProtocolBase = VnodeProtocolBaseContract.new( 2, { from: chain3.mc.accounts[0],  data: '0x' + bin,  gas: '5000000'});
	> chain3.mc.getTransactionReceipt(VnodeProtocolBase.transactionHash).contractAddress

部署完毕后，获得 vnode矿池合约地址  0x22f141dcc59850707708bc90e256318a5fe0b928	
	
注意：gas 不要设置太大， 不然会触发错误 exceeds block gas limit undefined
		
vnode 设置代理并加入矿池
------------------------

修改vnode目录配置文件vnodeconfig.json: VnodeBeneficialAddress里设置收益账号：  0xf103bc1c054babcecd13e7ac1cf34f029647b08c

这个账号也作为vnode的address，矿池中对应这个vnode的唯一编号

调用vnode矿池合约register方法加入矿池  

Parameters:
::
	from： 子链测试账号    
	value：押金，必须大于矿池合约的设置值  
	to: vnode矿池合约地址  
	data: register(address,string) 
	
关于data传递调用register参数说明:	
::
	根据ABI chain3.sha3("register(address,string)") = 0x32434a2e90725ed590daff07a244305001c58c49f7bef73ce5e7249acf69f561 
		取前4个字节 0x32434a2e  
	第一个参数address 传  vnodeconfig.json的VnodeBeneficialAddress  （前面补24个0， 凑足32个字节）  
		000000000000000000000000f103bc1c054babcecd13e7ac1cf34f029647b08c
	第二个参数string传 vnode提供给子链的调用地址 192.168.10.209:50062   
	端口号对应vnodeconfig.json的VnodeServiceCfg
		string数据类型 + string数据长度 + string内容
		string数据类型： 0000000000000000000000000000000000000000000000000000000000000040
		string内容： data = new Buffer('192.168.10.209:50062', 'utf8').toString('hex'); 
			后面补0， 凑足32个字节   3139322e3136382e31302e3230393a3530303632000000000000000000000000
		string数据长度:	3139322e3136382e31302e3230393a3530303632 	20字节
			0000000000000000000000000000000000000000000000000000000000000014
		data = '0x32434a2e000000000000000000000000f103bc1c054babcecd13e7ac1cf34f029647b08c000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000143139322e3136382e31302e3230393a3530303632000000000000000000000000'		

Example:
::
	> amount = chain3.toSha(5,'mc')
	> data = '0x32434a2e000000000000000000000000f103bc1c054babcecd13e7ac1cf34f029647b08c000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000143139322e3136382e31302e3230393a3530303632000000000000000000000000';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction({ from: chain3.mc.accounts[0], value:amount, to: '0x22f141dcc59850707708bc90e256318a5fe0b928', gas: "5000000", gasPrice: chain3.mc.gasPrice, data: data });

验证： 访问Vnode矿池合约的vnodeCount
::
	> chain3.mc.getStorageAt("0x22f141dcc59850707708bc90e256318a5fe0b928",0x02)   // 注意vnodeCount变量在合约中变量定义的位置（16进制）
	

SCS Pool
----------------------
		
目前针对不同的共识协议，可以创建对应的子链矿池，接受对应SCS的注册，并缴纳保证金，进入矿池后，成为子链的候选节点

如果加入现成的子链矿池，则可以忽略此步骤
		
部署SubChainProtocolBase.sol示例:    共识:POR  最低保证金: 2moac 
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
	> subchainprotocolbase = subchainprotocolbaseContract.new( "POR",  2, { from: chain3.mc.accounts[0],  data: '0x' + bin,  gas: '5000000'});
	> chain3.mc.getTransactionReceipt(subchainprotocolbase.transactionHash).contractAddress
	
部署完毕后，获得子链矿池合约地址  0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac


Start SCSs 
----------------------

这里我们设置两个scs节点

确认 userconfig.json配置
::
	VnodeServiceCfg为代理vnode地址: 192.168.10.209:50062
	Beneficiary为收益账号: 
		0xa934198916cd993c73c1aa6e0c0e7b21ce7c735b 
		0x2e7c076dbf6e61207a0ddb1b942ef7da8fd139f0
		
分别通过命令启动  scsserver-windows-4.0-amd64 --password "123456"   （生成scs keystore的密码）
		
然后在生成的keystore文件中分别获得 scs 地址  
::
	d4057328a35f34507dbcd295d43ed0cccf9c368a 
	0x3e21ba36b396936c6cc9adc3674655b912e5fa54

最后给scs转入moac以支付必要的交易费用
::		
	> amount = 20;
	> scsaddr = '0xd4057328a35f34507dbcd295d43ed0cccf9c368a';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: scsaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: ''});
	> scsaddr = '0x3e21ba36b396936c6cc9adc3674655b912e5fa54';
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: scsaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: ''});
	
可以通过查询余额进行验证  
::		
	> chain3.mc.getBalance('0xd4057328a35f34507dbcd295d43ed0cccf9c368a')
	> chain3.mc.getBalance('0x3e21ba36b396936c6cc9adc3674655b912e5fa54')
	
将scs加入子链矿池
----------------------

调用子链矿池合约register方法加入矿池
			
Parameters:
::
	from： 子链测试账号    
	value：押金，必须大于矿池合约的设置值  
	to: 子链矿池合约地址  
	data: register(address) 
	
关于data传递调用register参数说明:	
::	
	根据ABI chain3.sha3("register(address)") = 0x4420e4869750c98a56ac621854d2d00e598698ac87193cdfcbb6ed1164e9cbcd 
		取前4个字节 0x4420e486  
	参数address传scs 地址    d4057328a35f34507dbcd295d43ed0cccf9c368a  （前面补24个0， 凑足32个字节）  
		000000000000000000000000d4057328a35f34507dbcd295d43ed0cccf9c368a
	data = '0x4420e486000000000000000000000000d4057328a35f34507dbcd295d43ed0cccf9c368a'		

Example:
::
	> amount = chain3.toSha(5,'mc')
	> data = '0x4420e486000000000000000000000000d4057328a35f34507dbcd295d43ed0cccf9c368a';
	> chain3.mc.sendTransaction({ from: chain3.mc.accounts[0], value:amount, to: '0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac', gas: "5000000", gasPrice: chain3.mc.gasPrice, data: data });
	
验证： 访问子链矿池合约的scsCount
::		
	> chain3.mc.getStorageAt("0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac",0x02)	// 注意scsCount变量在合约中变量定义的位置（16进制）

同上将另一个scs（0x3e21ba36b396936c6cc9adc3674655b912e5fa54）也加入子链矿池


部署子链合约  
----------------------

现在我们可以部署一个子链合约，并准备将两个scs

部署SubChainBase.sol示例:
::
	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> input = {'': fs.readFileSync('SubChainBase.sol', 'utf8'), 'SubChainProtocolBase.sol': fs.readFileSync('SubChainProtocolBase.sol', 'utf8')};
	> output = solc.compile({sources: input}, 1);			
	> abi = output.contracts[':SubChainBase'].interface;
	> bin = output.contracts[':SubChainBase'].bytecode;
	> proto = '0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac' ;    // 子链矿池合约 
	> vnodeProtocolBaseAddr = '0x22f141dcc59850707708bc90e256318a5fe0b928' ;       // Vnode矿池合约 
	> min = 1 ;			// 子链需要SCS的最小数量，当前需要从如下值中选择：1，3，5，7
	> max = 11;		// 子链需要SCS的最大数量，当前需要从如下值中选择：11，21，31，51，99
	> thousandth = 1 ;			// 千分之几
	> flushRound = 40 ;     	// 子链刷新周期  单位是主链block生成对应数量的时间，当前的取值范围是40-99
	> SubChainBaseContract = chain3.mc.contract(JSON.parse(abi));  
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> SubChainBase = SubChainBaseContract.new( proto, vnodeProtocolBaseAddr, min, max, thousandth, flushRound,{ from: chain3.mc.accounts[0],  data: '0x' + bin,  gas:'9000000'} , function (e, contract){console.log('Contract address: ' + contract.address + ' transactionHash: ' + contract.transactionHash); });
	
		
部署完毕后, 获得子链合约地址  0x1195cd9769692a69220312e95192e0dcb6a4ec09
		

	
子链开放注册
----------------------

首先子链合约需要最终提供gas费给scs，需要给子链控制合约发送一定量的moac，调用合约里的函数addFund
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
		
然后调用  调用合约里的函数registerOpen 开放注册 (按子链矿池合约中SCS注册先后排序进行选取)
::
	根据ABI chain3.sha3("registerOpen()") = 0x5defc56ce78f178d760a165a5528a8e8974797e616a493970df1c0918c13a175
		取前4个字节 0x5defc56c 
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0x5defc56c'});				

	
验证：  访问子链合约的 registerFlag 为 1 ， 等待scs注册 (vnode 一个 flush周期后 ) ， 访问子链合约的 nodeCount
	> chain3.mc.getStorageAt(subchainaddr,0x14)  // 注意registerFlag变量在合约中变量定义的位置（16进制）
	> chain3.mc.getStorageAt(subchainaddr,0x0e)  // 注意nodeCount变量在合约中变量定义的位置（16进制）

子链关闭注册
----------------------

等到两个scs都注册完毕后，即注册SCS数目大于等于子链要求的最小数目时，调用子链合约里的函数 registerClose关闭注册
::
	根据ABI chain3.sha3("registerClose()") = 0x69f3576fc10c82561bd84b0045ee48d80d59a866174f2513fdef43d65702bf70
		取前4个字节 0x69f3576f 
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0x69f3576f'});
			
验证：  访问子链合约的 registerFlag 为 0
	> chain3.mc.getStorageAt(subchainaddr,0x14)	 // 注意registerFlag变量在合约中变量定义的位置（16进制）

SCS自身完成初始化并开始子链运行，可观察scs的concole界面，scs开始出块即成功完成部署子链。

