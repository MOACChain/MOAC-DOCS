Atomic Swap of MicroChain tokens
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

MOAC MicroChain can have their own tokens and these tokens can be swapped with MotherChain native token moac or other ERC20 token.

当前，墨客提供三种类型的母子链货币交互，以下一一介绍

Atomic Swap of moac (ASM)
------------------------
这个是最基础的一种货币兑换。使用者可以在主链上充值MOAC，然后最早在下一个flush周期在子链上获取子链原生币。同理，使用者可以提出子链原生币，并最早在下一个flush周期获得主链MOAC。

子链部署准备
================

参考子链部署章节，完成部署子链的准备工作。
::
	子链操作账号: 0x87e369172af1e817ebd8d63bcd9f685a513a6736
	vnode矿池合约地址: 0x22f141dcc59850707708bc90e256318a5fe0b928
	vnode代理地址: 0xf103bc1c054babcecd13e7ac1cf34f029647b08c    192.168.10.209:50062
	子链矿池合约地址: 0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac
	scs0: 0x075447a6df1fde4f39243bc67a945312ff36c193  确保启动并加入子链矿池
	scs1: 0x7932f827c90c5f06c0177f642a07edfa73ee3044  确保启动并加入子链矿池
	scs monitor: 0xa5966600efb221097ce6a8ba1dc6eb1d5b43ef83
	

subchainbase 部署
====================	

注意这个子链合约在官方基础上进行了修改，增加了setToken和MintToken相关的若干方法。
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
	> max = 11 ;		// 子链需要SCS的最大数量，当前需要从如下值中选择：11，21，31，51，99
	> thousandth = 1 ;			// 千分之几
	> flushRound = 40 ;     	// 子链刷新周期  单位是主链block生成对应数量的时间（10秒一个block）
	> SubChainBaseContract = chain3.mc.contract(JSON.parse(abi));  
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> SubChainBase = SubChainBaseContract.new( proto, vnodeProtocolBaseAddr, min, max, thousandth, flushRound,{ from: chain3.mc.accounts[0],  data: '0x' + bin,  gas:'9000000'} , function (e, contract){console.log('Contract address: ' + contract.address + ' transactionHash: ' + contract.transactionHash); });
	
部署完毕后, 获得子链合约地址  0xe9463e215315d6f1e5387a161868d7d0a4db89e8

			
子链注册scs
================	


子链开放注册

调用合约里的函数addFund
::	
	根据ABI chain3.sha3("addFund()") = 0xa2f09dfa891d1ba530cdf00c7c12ddd9f6e625e5368fff9cdf23c9dc0ad433b1
		取前4个字节 0xa2f09dfa 
	> amount = 20;
	> subchainaddr = '0xe9463e215315d6f1e5387a161868d7d0a4db89e8';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0xa2f09dfa'});

可以通过查询余额进行验证  
::		
	> chain3.mc.getBalance('0xe9463e215315d6f1e5387a161868d7d0a4db89e8')
		
然后调用  调用合约里的函数registerOpen 开放注册 (按子链矿池合约中SCS注册先后排序进行选取)
::
	根据ABI chain3.sha3("registerOpen()") = 0x5defc56ce78f178d760a165a5528a8e8974797e616a493970df1c0918c13a175
		取前4个字节 0x5defc56c 
	> subchainaddr = '0xe9463e215315d6f1e5387a161868d7d0a4db89e8';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0x5defc56c'});				

	
验证：  

访问子链合约的 nodeCount
::
	> chain3.mc.getStorageAt(subchainaddr,0x0e)  // 注意nodeCount变量在合约中变量定义的位置（16进制）



等到两个scs都注册完毕后，即注册SCS数目大于等于子链要求的最小数目时，调用子链合约里的函数 registerClose关闭注册
::
	根据ABI chain3.sha3("registerClose()") = 0x69f3576fc10c82561bd84b0045ee48d80d59a866174f2513fdef43d65702bf70
		取前4个字节 0x69f3576f 
	> subchainaddr = '0xe9463e215315d6f1e5387a161868d7d0a4db89e8';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0x69f3576f'});
			
验证：  

访问子链合约的 registerFlag 为 0
::
	> chain3.mc.getStorageAt(subchainaddr,0x14)	// 注意registerFlag变量在合约中变量定义的位置（16进制）
	
同时观察scs的concole界面，scs开始出块即成功完成部署子链。
	
dapp合约部署
================	
按照多合约部署步骤，需要首先部署dappbase合约，方法请参见"子链业务逻辑的部署"。
验证: 
 | 合约部署成功后，Nonce值应该是1  

部署 dapp合约，dechat是一个官方示例，注意合约有两个参数，分别是版主的账号和开发者的账号。

部署示例:
::
	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> solfile = 'dechat1.0.5.sol';
	> contract = fs.readFileSync(solfile, 'utf8');
	> output = solc.compile(contract, 1);                    
	> abi = output.contracts[':DeChat'].interface;
	> bin = output.contracts[':DeChat'].bytecode;	
	> bin += '000000000000000000000000' + '87e369172af1e817ebd8d63bcd9f685a513a6736';  // 版主的账号
	> bin += '000000000000000000000000' + '87e369172af1e817ebd8d63bcd9f685a513a6736';  // 开发者的账号
	> amount = chain3.toSha(100000000,'mc') // 需要在子链上发行的原生币的数量
	> subchainaddr = '0xe9463e215315d6f1e5387a161868d7d0a4db89e8';
	> via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c'; 
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction({from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas:0, shardingFlag: "0x3", data: '0x' + bin, nonce: 0, via: via, });
			
验证: 
 | 合约部署成功后，Nonce值应该是2


 部署完dapp合约，需要将合约注册入dappbase。


		
dapp 充值
================		
	
调用 subchainbase 的 buyMintToken方法充值， 用户账号为发出sendTransaction的账号 数量为sendTransaction的amount参数
 | buyMintToken方法首先调用 DirectExchangeToke对象 的 buyMintToken 方法  按交易记录的amount 扣除DirectExchangeToke合约本身的token数量，增加用户账号对应token的数量
 
 | 然后调用 transfer 给DirectExchangeToken合约地址 转 对应的moac 
 
 | 再调用 DirectExchangeToke对象 的 requestEnterMicrochain方法 （传subchainbase地址和对应token数量），减去用户账号地址的对应token数量， 增加DirectExchangeToke合约本身的token总量
 
 | 最后将用户账号和对应token数量，加入推送结构体发至子链，等待一轮flush后，充值会进入到子链

Example：
::
	根据ABI chain3.sha3("buyMintToken()") = 0x6bbded701cd78dee9626653dc2b2e76d3163cc5a6f81ac3b8e69da6a057824cb
		取前4个字节 0x6bbded70
	> amount = 100;
	> nonce = 1	  // 调用ScsRPCMethod.GetNonce获得
	> subchainaddr = '0xe9463e215315d6f1e5387a161868d7d0a4db89e8';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { nonce: nonce, from: chain3.mc.accounts[0], value: chain3.toSha(amount,'mc'), to: subchainaddr, gas:0, shardingFlag:'0x1',  data: '0x6bbded70', via: via});
			
验证：  
 | 检查账号的moac是否减少:    > chain3.mc.getBalance(chain3.mc.accounts[0])
 | 检查子链的token是否增加:  调用monitor的方法 ScsRPCMethod.GetBalance 获得子链token


dapp 提币
================	

**请注意data前需要加上dappbase合约地址**			

调用 dappbase合约 的 redeemFromMicroChain方法，用户账号为发出sendTransaction的账号 数量为sendTransaction的amount参数
 | redeemFromMicroChain方法将用户账号和对应token数量加入推送结构体redeem，等待一轮flush后，自动会调用子链合约的redeemFromMicroChain方法
 
 | 然后子链合约自动调用DirectExchangeToke对象的 redeemFromMicroChain 方法，扣除DirectExchangeToke合约本身的token数量，增加用户账号对应token的数量 
 
 | 最后自动调用subchainbase的sellMintToken 方法，自动调用DirectExchangeToke对象的 sellMintToken  兑换moac

Example：
::
	根据ABI chain3.sha3("redeemFromMicroChain()") = 0x89739c5bf1ef36273bf0e7aeb59ffe71213a58e1f01965e75662cb21b03abb13
	取前4个字节 0x89739c5b
	调用dapp合约方法，需要再data前加入dappaddr
	> nonce = 1	  // 调用ScsRPCMethod.GetNonce获得
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { nonce: nonce, from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas:0, shardingFlag:'0x1', data: dappaddr + '0x89739c5b', via: via,});
	
	
验证：  
 | 检查账号的moac是否增加:    > chain3.mc.getBalance(chain3.mc.accounts[0])
 | 检查子链的token是否减少:  调用monitor的方法 ScsRPCMethod.GetBalance 获得子链token

 
 
Atomic Swap of Token (AST)
-------------------------
The user can swap the MicroChain token with ERC20 token in the MotherChain.


子链部署准备
================

参考子链部署章节，完成部署子链的准备工作。
::
	子链操作账号: 0x87e369172af1e817ebd8d63bcd9f685a513a6736
	vnode矿池合约地址: 0x22f141dcc59850707708bc90e256318a5fe0b928
	vnode代理地址: 0xf103bc1c054babcecd13e7ac1cf34f029647b08c    192.168.10.209:50062
	子链矿池合约地址: 0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac
	scs0: 	0xd81043d85c9c959d2925958c54c1a49c7bfd1fc8  确保启动并加入子链矿池
	scs1: 	0xe767059d768fcef12e527fab63fda68cc13e24b3  确保启动并加入子链矿池
	scs monitor: 	0x0964e5d73d6a40f2fc707aa3e1361028a34923f0
	
	
erc20 部署
================	

默认一个标准的erc20合约，通过allowance，transferFrom，balanceOf，transfer等标准的方法支持货币的转移。

参考官方示例的erc20合约erc20.sol，默认decimals为6，totalSupply为100000000乘以10的6次方。
Example：
::
	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> solfile = 'erc20.sol';
	> contract = fs.readFileSync(solfile, 'utf8');
	> output = solc.compile(contract, 1);            
	> abi = output.contracts[':TestCoin'].interface;
	> bin = output.contracts[':TestCoin'].bytecode;
	> erc20Contract = chain3.mc.contract(JSON.parse(abi));  
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> dtoken = erc20Contract.new( { from: chain3.mc.accounts[0],  data: '0x' + bin,  gas:'9000000'} , function (e, contract){console.log('Contract address: ' + contract.address + ' transactionHash: ' + contract.transactionHash); });

部署完毕后, 获得erc20合约地址  0x5042086887a86151945d2c2bb60628addf49d48c

验证： 调用合约balanceOf方法查询部署者的余额，应该是10的14次方

	> contractInstance = erc20Contract.at('0x5042086887a86151945d2c2bb60628addf49d48c')
	> contractInstance.balanceOf.call('0x87e369172af1e817ebd8d63bcd9f685a513a6736')
	

subchainbase 部署
=====================	

注意这个子链合约在官方基础上进行了修改，增加了erc20合约地址和兑换比例的参数
部署SubChainBase.sol示例:
::
	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> input = {'': fs.readFileSync('SubChainBase.sol', 'utf8'), 'SubChainProtocolBase.sol':fs.readFileSync('SubChainProtocolBase.sol', 'utf8')};
	> output = solc.compile({sources: input}, 1);			
	> abi = output.contracts[':SubChainBase'].interface;
	> bin = output.contracts[':SubChainBase'].bytecode;
	> proto = '0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac' ;    // 子链矿池合约 
	> vnodeProtocolBaseAddr = '0x22f141dcc59850707708bc90e256318a5fe0b928' ;       // Vnode矿池合约 
	> ercAddr = '0x5042086887a86151945d2c2bb60628addf49d48c';     // erc20合约地址
	> ercRate = 100;    // 兑换比率
	> min = 1 ;			// 子链需要SCS的最小数量，当前需要从如下值中选择：1，3，5，7
	> max = 11 ;		// 子链需要SCS的最大数量，当前需要从如下值中选择：11，21，31，51，99
	> thousandth = 1 ;			// 千分之几
	> flushRound = 40 ;     	// 子链刷新周期  单位是主链block生成对应数量的时间（10秒一个block）
	> SubChainBaseContract = chain3.mc.contract(JSON.parse(abi));  
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> SubChainBase = SubChainBaseContract.new( proto, vnodeProtocolBaseAddr, ercAddr, ercRate, min, max, thousandth, flushRound,{ from: chain3.mc.accounts[0],  data: '0x' + bin,  gas:'9000000'} , function (e, contract){console.log('Contract address: ' + contract.address + ' transactionHash: ' + contract.transactionHash); });
	
部署完毕后, 获得子链合约地址  0xb877bf4e4cc94fd9168313e00047b77217760930



验证：  

访问子链合约的 BALANCE 为 ERC20的 totalsupply 
::	
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> chain3.mc.getStorageAt(subchainaddr,0x30)   // 注意BALANCE变量在合约中变量定义的位置（16进制） 1ed09bead87c0378d8e6400000000
	应该是10的34次方  (14 + 2 + 18)

			
子链注册scs
================	


子链开放注册

调用合约里的函数addFund
::	
	根据ABI chain3.sha3("addFund()") = 0xa2f09dfa891d1ba530cdf00c7c12ddd9f6e625e5368fff9cdf23c9dc0ad433b1
		取前4个字节 0xa2f09dfa 
	> amount = 20;
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0xa2f09dfa'});

可以通过查询余额进行验证  
::		
	> chain3.mc.getBalance('0xb877bf4e4cc94fd9168313e00047b77217760930')
		
然后调用  调用合约里的函数registerOpen 开放注册 (按子链矿池合约中SCS注册先后排序进行选取)
::
	根据ABI chain3.sha3("registerOpen()") = 0x5defc56ce78f178d760a165a5528a8e8974797e616a493970df1c0918c13a175
		取前4个字节 0x5defc56c 
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0x5defc56c'});				

验证：  

访问子链合约的 nodeCount
::
	> chain3.mc.getStorageAt(subchainaddr,0x0e)  // 注意nodeCount变量在合约中变量定义的位置（16进制）


等到两个scs都注册完毕后，即注册SCS数目大于等于子链要求的最小数目时，调用子链合约里的函数 registerClose关闭注册
::
	根据ABI chain3.sha3("registerClose()") = 0x69f3576fc10c82561bd84b0045ee48d80d59a866174f2513fdef43d65702bf70
		取前4个字节 0x69f3576f 
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: '0x69f3576f'});
			
验证：  

访问子链合约的 registerFlag 为 0
::
	> chain3.mc.getStorageAt(subchainaddr,0x14)	// 注意registerFlag变量在合约中变量定义的位置（16进制）
	
同时观察scs的concole界面，scs开始出块即成功完成部署子链。
	
dapp合约部署
================	
按照多合约部署步骤，需要首先部署dappbase合约，方法请参见"子链业务逻辑的部署"。
验证: 
 | 合约部署成功后，Nonce值应该是1 

部署 dapp合约，dechat是一个官方示例，注意合约有两个参数，分别是版主的账号和开发者的账号

部署示例:
::
	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> solfile = 'dechat1.0.5.sol';
	> contract = fs.readFileSync(solfile, 'utf8');
	> output = solc.compile(contract, 1);                    
	> abi = output.contracts[':DeChat'].interface;
	> bin = output.contracts[':DeChat'].bytecode;	
	> bin += '000000000000000000000000' + '87e369172af1e817ebd8d63bcd9f685a513a6736';  // 版主的账号
	> bin += '000000000000000000000000' + '87e369172af1e817ebd8d63bcd9f685a513a6736';  // 开发者的账号
	> amount = chain3.toSha(100000000,'mc') // dapp合约地址的token数量，与erc20的amount一致
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c'; 
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction({from: chain3.mc.accounts[0], value:amount, to: subchainaddr, gas:0, shardingFlag: "0x3", data: '0x' + bin, nonce: 0, via: via, });

	
验证: 
 | 在 scs 的_logs目录下搜索日志文件，查找"created contract address"，找到dapp合约地址:6ab296062d8a147297851719682fb5ffe081f1d3
 | 调用monitor的方法 ScsRPCMethod.GetBalance 查询对应dapp合约地址的余额，应该等于erc20总量。
		
dapp 充值
================		
	
调用 subchainbase 的 buyMintToken方法充值， 用户账号为发出sendTransaction的账号 ，参数分别为子链合约地址和token数量。
 | buyMintToken方法首先调用erc20合约的allowance检查授权，并调用transferFrom方法将token从用户账号地址转到合约地址
 
 | 然后自动调用requestEnterMicrochain方法，将用户账号和对应token数量，加入推送结构体发至子链，等待一轮flush后，充值会进入到子链

Example：
::
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> data = contractInstance.buyMintToken.getData(subchainaddr, 100)
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value: 0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: data});
			
验证：  
 | 检查账号的erc20 token是否减少:    调用erc20合约的balanceOf方法
 | 检查子链的token是否增加:  调用monitor的方法 ScsRPCMethod.GetBalance 获得子链token


dapp 提币
================					

**请注意data前需要加上dappbase合约地址**

调用 dappbase合约 的 redeemFromMicroChain方法，用户账号为发出sendTransaction的账号 数量为sendTransaction的amount参数
 | redeemFromMicroChain方法将用户账号和对应token数量加入推送结构体redeem，等待一轮flush后，自动会调用子链合约的redeemFromMicroChain方法
 
 | 调用erc20合约的transfer给用户账号转对应的token数量

Example：
::
	根据ABI chain3.sha3("redeemFromMicroChain()") = 0x89739c5bf1ef36273bf0e7aeb59ffe71213a58e1f01965e75662cb21b03abb13
	取前4个字节 0x89739c5b
	调用dapp方法，需要再data前加入dappaddr
	> nonce = 1	  // 调用ScsRPCMethod.GetNonce获得
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { nonce: nonce, from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas:0, shardingFlag:'0x1', data: dappaddr + '0x89739c5b', via: via,});
	
	
验证：  
 | 检查账号的erc20 token是否增加:    调用erc20合约的balanceOf方法
 | 检查子链的token是否减少:  调用monitor的方法 ScsRPCMethod.GetBalance 获得子链token

