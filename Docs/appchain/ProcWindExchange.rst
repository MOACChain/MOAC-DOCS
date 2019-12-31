.. _proc-wind-as:

ProcWind 跨链指南
----------------

ProcWind 应用链支持有币应用链，并且提供母链货币和应用链原生币之间的兑换。

当前，有两种类型的应用链货币交互，简称为

母链MOAC和应用链原生币交互
========================

这个是最基础的一种货币兑换。使用者可以在主链上充值MOAC，然后最早在下一个flush周期在应用链上获取应用链原生币。同理，使用者可以提出应用链原生币，并最早在下一个flush周期获得主链MOAC。

对应release中的ASM版本包

应用链部署准备


参考应用链部署章节，完成部署应用链的准备工作。
::
	应用链操作账号: 0x87e369172af1e817ebd8d63bcd9f685a513a6736
	vnode节点池合约地址: 0x22f141dcc59850707708bc90e256318a5fe0b928
	vnode代理地址: 0xf103bc1c054babcecd13e7ac1cf34f029647b08c    192.168.10.209:50062
	应用链节点池合约地址: 0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac
	scs0: 0x075447a6df1fde4f39243bc67a945312ff36c193  确保启动并加入应用链节点池
	scs1: 0x7932f827c90c5f06c0177f642a07edfa73ee3044  确保启动并加入应用链节点池
	scs2 兼 monitor: 0xa5966600efb221097ce6a8ba1dc6eb1d5b43ef83  确保启动并加入应用链节点池
	


subchainbase 部署
====================	

注意这个应用链合约在官方基础上进行了修改，对应v1目录下的SubChainBase.sol（带tokensupply和exchangerate参数）。
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
	> proto = '0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac' ;    // 应用链节点池合约 
	> vnodeProtocolBaseAddr = '0x22f141dcc59850707708bc90e256318a5fe0b928' ;       // Vnode节点池合约 
	> min = 1 ;			// 应用链需要SCS的最小数量，当前需要从如下值中选择：1，3，5，7
	> max = 11 ;		// 应用链需要SCS的最大数量，当前需要从如下值中选择：11，21，31，51，99
	> thousandth = 1 ;			// 千分之几
	> flushRound = 40 ;     	// 应用链刷新周期  单位是主链block生成对应数量的时间（10秒一个block）
	> tokensupply = 1000;    // 应用链原生币总额
	> exchangerate = 100;		// 与moac兑换比例
	> SubChainBaseContract = chain3.mc.contract(JSON.parse(abi));  
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> SubChainBase = SubChainBaseContract.new( proto, vnodeProtocolBaseAddr, min, max, thousandth, flushRound, tokensupply, exchangerate { from: chain3.mc.accounts[0],  data: '0x' + bin,  gas:'9000000'} , function (e, contract){console.log('Contract address: ' + contract.address + ' transactionHash: ' + contract.transactionHash); });
	
部署完毕后, 获得应用链合约地址  0xe9463e215315d6f1e5387a161868d7d0a4db89e8

验证：  
 | 访问应用链合约的 BALANCE 与 tokensupply 对应
 | 应该是10的21次方 : 即 tokensupply * 10的18次方(应用链原生币decimals) 
 
调用示例:  
::	
	> subchainaddr = '0xe9463e215315d6f1e5387a161868d7d0a4db89e8';
	> SubChainBase = SubChainBaseContract.at(subchainaddr);
	> SubChainBase.BALANCE()
	
调用monitor的方法 ScsRPCMethod.GetBalance 查询对应部署账号地址的余额，应该也等于10的21次方			
		
	
dappbase合约部署
================	

应用链开放注册后，待scs开始出块即成功完成部署应用链，方法请参见"应用链的部署方法"。
按照多合约部署步骤，需要首先部署dappbase合约，方法请参见"应用链业务逻辑的部署"。

# 注意部署dappbase合约的value为 1000(tokensupply) * 10的18次方(应用链原生币decimals) 

部署示例:
::
	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> solfile = 'DappBase.sol';
	> contract = fs.readFileSync(solfile, 'utf8');
	> output = solc.compile(contract, 1);                    
	> abi = output.contracts[':DappBase'].interface;
	> bin = output.contracts[':DappBase'].bytecode;
	> amount = chain3.toSha(1000,'mc') 
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c'; 
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction({from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: subchainaddr, gas:0, gasPrice: 0, shardingFlag: "0x3", data: '0x' + bin, nonce:0, via: via });

	
验证: 
 | 调用monitor的方法 ScsRPCMethod.GetNonce  Nonce值应该是1  
 | 调用monitor的方法 ScsRPCMethod.GetBalance 查询对应dappbase合约地址的余额，应该等于10的21次方 
 | 调用monitor的方法 ScsRPCMethod.GetBalance 查询对应部署账号地址的余额，应该等于0
 | 调用monitor的方法 ScsRPCMethod.GetReceipt 传入对应Nonce，从contractAddress字段内容获得合约地址


		
dapp 充值
================		
	
调用 subchainbase 的 buyMintToken方法充值， 用户账号为发出sendTransaction的账号 数量为sendTransaction的amount参数
 

调用示例：
::
	根据ABI chain3.sha3("buyMintToken()") = 0x6bbded701cd78dee9626653dc2b2e76d3163cc5a6f81ac3b8e69da6a057824cb
		取前4个字节 0x6bbded70
	> amount = 1;
	> subchainaddr = '0xe9463e215315d6f1e5387a161868d7d0a4db89e8';
	> chain3.personal.unlockAccount(chain3.mc.accounts[1], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[1], value: chain3.toSha(amount,'mc'), to: subchainaddr, gas:"2000000", gasPrice: chain3.mc.gasPrice, data: '0x6bbded70'});
			
验证：  
 | 检查账号的moac是否减少:    > chain3.mc.getBalance(chain3.mc.accounts[1])
 | 检查应用链的token是否增加:  调用monitor的方法 ScsRPCMethod.GetBalance 获得应用链chain3.mc.accounts[1]地址对应token
 | 检查应用链dappbase合约地址的原生币是否减少:  调用monitor的方法 ScsRPCMethod.GetBalance


dapp 提币
================	

**请注意data前需要加上dappbase合约地址**			

调用 dappbase合约 的 redeemFromMicroChain方法，用户账号为发出sendTransaction的账号 数量为sendTransaction的amount参数
 | redeemFromMicroChain方法将用户账号和对应token数量加入推送结构体redeem，等待一轮flush后生效


调用示例：
::
	根据ABI chain3.sha3("redeemFromMicroChain()") = 0x89739c5bf1ef36273bf0e7aeb59ffe71213a58e1f01965e75662cb21b03abb13
	取前4个字节 0x89739c5b
	调用dapp合约方法，需要再data前加入dappaddr
	> nonce = 1	  // 调用ScsRPCMethod.GetNonce获得
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> dappbassaddr = dappbase合约地址
	> via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c';
	> amount = 10  // 对应应用链原生币  10 * 18次方    即0.1 moac
	> chain3.personal.unlockAccount(chain3.mc.accounts[1], '123456');
	> chain3.mc.sendTransaction( { nonce: nonce, from: chain3.mc.accounts[1], value:chain3.toSha(amount,'mc'), to: subchainaddr, gas:0, shardingFlag:'0x1', data: dappbassaddr + '89739c5b', via: via,});
	
	
验证：  
 | 检查账号的moac是否增加:    > chain3.mc.getBalance(chain3.mc.accounts[1])
 | 检查应用链的token是否减少:  调用monitor的方法 ScsRPCMethod.GetBalance 获得应用链token
 | 检查应用链dappbase合约地址的原生币是否增加:  调用monitor的方法 ScsRPCMethod.GetBalance

 
 
母链ERC20和应用链原生币交互
-------------------------
这是非常通用的一种货币兑换。使用者可以使用预先已经部署好的ERC20，或者当场部署一个主链ERC20，和应用链的原生币进行兑换。

对应release中的AST版本包


应用链部署准备
================

参考应用链部署章节，完成部署应用链的准备工作。
::
	应用链操作账号: 0x87e369172af1e817ebd8d63bcd9f685a513a6736
	vnode节点池合约地址: 0x22f141dcc59850707708bc90e256318a5fe0b928
	vnode代理地址: 0xf103bc1c054babcecd13e7ac1cf34f029647b08c    192.168.10.209:50062
	应用链节点池合约地址: 0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac
	scs0: 	0xd81043d85c9c959d2925958c54c1a49c7bfd1fc8  确保启动并加入应用链节点池
	scs1: 	0xe767059d768fcef12e527fab63fda68cc13e24b3  确保启动并加入应用链节点池
	scs2 兼 monitor: 	0x0964e5d73d6a40f2fc707aa3e1361028a34923f0 确保启动并加入应用链节点池
	
	
erc20 部署
================	

默认一个标准的erc20合约，通过allowance，transferFrom，balanceOf，transfer等标准的方法支持货币的转移。

参考官方示例的erc20合约erc20.sol，默认decimals为2，totalSupply为10000乘以10的2次方。
调用示例：
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

验证： 调用合约balanceOf方法查询部署者的余额，应该是1000000
::
	> contractInstance = erc20Contract.at('0x5042086887a86151945d2c2bb60628addf49d48c')
	> contractInstance.balanceOf.call('0x87e369172af1e817ebd8d63bcd9f685a513a6736')
	

subchainbase 部署
=====================	

注意这个应用链合约在官方基础上进行了修改，增加了erc20合约地址和兑换比例的参数
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
	> proto = '0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac' ;    // 应用链节点池合约 
	> vnodeProtocolBaseAddr = '0x22f141dcc59850707708bc90e256318a5fe0b928' ;       // Vnode节点池合约 
	> ercAddr = '0x5042086887a86151945d2c2bb60628addf49d48c';     // erc20合约地址
	> ercRate = 10;    // 兑换比率
	> min = 1 ;			// 应用链需要SCS的最小数量，当前需要从如下值中选择：1，3，5，7
	> max = 11 ;		// 应用链需要SCS的最大数量，当前需要从如下值中选择：11，21，31，51，99
	> thousandth = 1 ;			// 千分之几
	> flushRound = 40 ;     	// 应用链刷新周期  单位是主链block生成对应数量的时间（10秒一个block）
	> SubChainBaseContract = chain3.mc.contract(JSON.parse(abi));  
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> SubChainBase = SubChainBaseContract.new( proto, vnodeProtocolBaseAddr, ercAddr, ercRate, min, max, thousandth, flushRound,{ from: chain3.mc.accounts[0],  data: '0x' + bin,  gas:'9000000'} , function (e, contract){console.log('Contract address: ' + contract.address + ' transactionHash: ' + contract.transactionHash); });
	
部署完毕后, 获得应用链合约地址  0xb877bf4e4cc94fd9168313e00047b77217760930



验证：  
 | 访问应用链合约的 BALANCE 与 ERC20的 totalsupply 对应
 | 应该是10的23次方 : 即 1000000(ERC20的totalsupply) * 10(兑换比率) * 10的18次方(应用链原生币decimals) / 10的2次方(ERC20的decimals)
 
调用示例: 
::
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> SubChainBase = SubChainBaseContract.at(subchainaddr);
	> SubChainBase.BALANCE()
	
调用monitor的方法 ScsRPCMethod.GetBalance 查询对应部署账号地址的余额，应该等于10的23次方			

	
dappbase合约部署
================	

应用链开放注册后，待scs开始出块即成功完成部署应用链，方法请参见"应用链的部署方法"。
按照多合约部署步骤，需要首先部署dappbase合约，方法请参见"应用链业务逻辑的部署"。

注意部署dappbase合约的value为 ERC20的totalsupply * 10(兑换比率) * 10的18次方(应用链原生币decimals) / 10的2次方(ERC20的decimals)

部署示例:
::
	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> solfile = 'DappBase.sol';
	> contract = fs.readFileSync(solfile, 'utf8');
	> output = solc.compile(contract, 1);                    
	> abi = output.contracts[':DappBase'].interface;
	> bin = output.contracts[':DappBase'].bytecode;
	> amount = chain3.toSha(100000,'mc') 
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c'; 
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction({from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: subchainaddr, gas:0, gasPrice: 0, shardingFlag: "0x3", data: '0x' + bin, nonce:0, via: via });

	
验证: 
 | 调用monitor的方法 ScsRPCMethod.GetBalance 查询对应dappbase合约地址的余额，应该等于10的23次方 
 | 调用monitor的方法 ScsRPCMethod.GetBalance 查询对应部署账号地址的余额，应该等于0
 | 调用monitor的方法 ScsRPCMethod.GetReceipt 传入对应Nonce，从contractAddress字段内容获得合约地址
		
dapp 充值
================		
	
调用 subchainbase 的 buyMintToken方法充值， 用户账号为发出sendTransaction的账号 ，参数分别为应用链合约地址和erc20的数量。
注意：buyMintToken方法首先调用erc20合约的allowance检查授权，再调用transferFrom方法将token从用户账号地址转到合约地址
所以要先调用erc20的approve方法授权对应的erc20给subchainbase合约地址。

调用示例：
::
	> amount = 200 
	> data = erc20.approve.getData(subchainaddr, amount);
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value: 0, to: erc20.address, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: data});
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> SubChainBase = SubChainBaseContract.at(subchainaddr);
	> data = SubChainBase.buyMintToken.getData(amount)
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value: 0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: data});
			
验证：  
 | 检查账号的erc20 token是否减少200:    调用erc20合约的balanceOf方法
 | 检查应用链对应账号的原生币是否增加20000000000000000000:  调用monitor的方法 ScsRPCMethod.GetBalance
 | 检查应用链dappbase合约地址的原生币是否减少20000000000000000000:  调用monitor的方法 ScsRPCMethod.GetBalance

dapp 提币
================					

**请注意data前需要加上dappbase合约地址**

调用 dappbase合约 的 redeemFromMicroChain方法，用户账号为发出sendTransaction的账号 数量为sendTransaction的amount参数
 | redeemFromMicroChain方法将用户账号和对应token数量加入推送结构体redeem，等待一轮flush后，自动会调用应用链合约的redeemFromMicroChain方法
 
 | 调用erc20合约的transfer给用户账号转对应的token数量

调用示例：
::
	根据ABI chain3.sha3("redeemFromMicroChain()") = 0x89739c5bf1ef36273bf0e7aeb59ffe71213a58e1f01965e75662cb21b03abb13
	取前4个字节 89739c5b
	调用dapp方法，需要再data前加入dappaddr
	> nonce = 5	  // 调用ScsRPCMethod.GetNonce获得
	> subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
	> dappbassaddr = dappbase合约地址
	> via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c';
	> amount = chain3.toSha(10,'mc')    //   * 10的2次方(ERC20的decimals) / 10(兑换比率)   100 即为对应erc20数量
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { nonce: nonce, from: chain3.mc.accounts[0], value:amount, to: subchainaddr, gas:0, shardingFlag:'0x1', data: dappbassaddr + '89739c5b', via: via,});
	
验证：  
 | 检查账号的erc20 token是否增加100:    调用erc20合约的balanceOf方法
 | 等待一轮flush后，检查应用链对应账号的原生币是否减少10000000000000000000:  调用monitor的方法 ScsRPCMethod.GetBalance


ATO方式
----------------------
TODO