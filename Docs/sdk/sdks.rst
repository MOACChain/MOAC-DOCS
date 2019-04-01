SDK Methods
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Account
---------------------------

register
=====================

Node.JS Example

Parameters:
::
	pwd：钱包账户密码

Example:
::
	var account = require("moac-api").account;
	var wallet = account.register(pwd);

Results:
::
	wallet：
	{ address: '钱包地址....',
	  privateKey: '私钥....',
	  keyStore: 'keyStore内容...' 
	}
  
log in
=====================

Node.JS Example

Parameters:
::
	addr：钱包地址
	pwd：钱包密码
	keyStore：keyStore

Example:
::
	var account = require("moac-api").account;
	var status = account.login(addr, pwd, keyStore);

Results:
::
	status：0 //登录失败
	status：1 //登录成功
	status：2 //密码错误


VNODE
---------------------------


Create VNODE object
=========================

Node.JS Example

Parameters:
::
	vnodeAddress：主链访问地址 //http://47.106.69.61:8989
	
Example:
::
	var VnodeChain = require("moac-api").vnodeChain;
	var vc = new VnodeChain(vnodeAddress);

Get MotherChain block number
===========================================

Node.JS Example


Example:
::
	var blockNumber = vc.getBlockNumber();

Results:
::
	blockNumber：主链区块高度
	
Get MotherChain block information
====================================

Node.JS Example

Parameters:
::
	hashOrNumber：区块hash或区块高度

Example:
::
	var blockInfo = vc.getBlockInfo(hashOrNumber);

Results:
::
	blockInfo：某一区块信息

获取主链交易详情
=====================================

Node.JS Example

Parameters:
::
	hash：交易hash

Example:
::
	var tradeInfo = vc.getTransactionByHash(hash);

Results:
::
	tradeInfo：交易详情
	
获取合约实例
===========================

Node.JS Example

Parameters:
::
	microChainAddress：子链地址
	versionKey：版本号（默认0.1版本）

Example:
::
	var data = vc.getSubChainBaseInstance(microChainAddress, versionKey);

Results:
::
	data：合约实例
	
获取主链账户余额
=====================================

Node.JS Example

Parameters:
::
	addr：钱包账户地址 
	
Example:
::
	var balance = vc.getBalance(addr);
	
Results:
::
	balance：主链账户余额（单位为moac）

获取主链账户ERC代币余额
=============================================

Node.JS Example

Parameters:
::
	addr：钱包账户地址 
	contractAddress：合约地址
	
Example:
::
	var balance = vc.getErcBalance(addr, contractAddress);
	
Results:
::
	balance：账户ERC代币余额（erc20最小单位）
	
获取主链合约实例
================================

Node.JS Example

Parameters:
::
	abiObj：abi对象
	contractAddress：合约地址
	
Example:
::
	var object = vc.getContractInstance(abiObj, contractAddress);
	
Results:
::
	object：主链合约实例对象
	
获取交易Data
=========================

Parameters:
::
	method：方法 例 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 例['address','uint256']
	paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）

Example:
::
	var data = mc.getData(method,paramTypes,paramValues);

Results:
::
	data：data字符串
	
主链加签交易
=========================

Node.JS Example

Parameters:
::
	from：交易发送人
	to：交易接受者（可以为个人地址，或者主链上的合约地址）
	amount：交易金额
	method：方法 例 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 例['address','uint256']
	paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）
	privateKey：交易发起人私钥字符串
	gasPrice：gas费用（默认为0，如返回错误为gas过低，请在返回的gas基础上加上整数gas重新提交）
	
Example:
::
	vc.sendRawTransaction(from, to, amount, method, paramTypes, paramValues, privateKey, gasPrice).then((hash) => {
		console.log(hash);
	});
	
Results:
::
	hash：交易hash
	
主链MOAC转账
=========================

Parameters:
::
	from：转账人地址
	to：收款人地址
	amount：交易金额（单位为moac）
	privatekey：转账人私钥

Example:
::
	vc.transferMoac(from, to, amount, privatekey).then((hash) => {
		console.log(hash);
	});

Results:
::
	hash：交易hash
	
主链ERC代币转账
==============================

Parameters:
::
	from：转账人地址
	to：收款人地址
	contractAddress：erc代币合约地址
	amount：交易金额（单位为moac）
	privateKey：转账人私钥

Example:
::
	vc.transferErc(from, to, contractAddress, amount, privateKey).then((hash) => {
		console.log(hash);
	});

Results:
::
	hash：交易hash
	
调用主链合约
=========================

Parameters:
::
	method：方法 例 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 例['address','uint256']
	paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）
	contractAddress：合约地址

Example:
::
	var callRes = vc.callContract(method, paramTypes, paramValues, contractAddress);

Results:
::
	callRes：调用合约返回信息
	
ERC20充值
=========================

Parameters:
::
	addr：钱包地址
	privateKey：钱包私钥
	microChainAddress：子链地址
	method：方法 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 ['address','uint256']
	paramValues：paramValues 参数值数组 ['0x.....',10000]（需要传金额的入参为erc20最小单位）

Example:
::
	vc.buyErcMintToken(addr, privateKey, microChainAddress, method, paramTypes, paramValues).then((hash) => {
		console.log(hash);
	});

Results:
::
	hash：交易hash

MOAC充值
=========================

Parameters:
::
	addr：钱包地址
	privateKey：钱包私钥
	microChainAddress：子链地址
	method：方法 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 ['address','uint256']
	paramValues：paramValues 参数值数组 ['0x.....',10000]（金额单位为moac）

Example:
::
	vc.buyMoacMintToken(addr, privateKey, microChainAddress, method, paramTypes, paramValues).then((hash) => {
		console.log(hash);
	});

Results:
::
	hash：交易hash
	
MicroChain/SCS
---------------------------

实例化子链对象
=================================

Node.JS Example

Parameters:
::
	vnodeAddress：主链访问地址 //http://47.106.69.61:8989
	monitorAddress：子链访问地址 //http://47.106.89.22:8546
	microChainAddress：子链地址
	via：子链via

Example:
::
	var MicroChain = require("moac-api").microChain;
	var mc = new MicroChain(vnodeAddress, monitorAddress, microChainAddress, via);

获取子链区块高度
=========================

Node.JS Example

Example:
::
	mc.getBlockNumber().then((blockNumber) => {
		console.log(blockNumber);
	});

Results:
::
	blockNumber：子链区块高度
	
获取某一区间内的多个区块信息
=================================================

Node.JS Example

Parameters:
::
	start：开始高度
	end：结束高度

Example:
::
	mc.getBlocks(start, end).then((blockListInfo) => {
		console.log(blockListInfo);
	});

Results:
::
	blockListInfo：区块信息List
	
获取子链某一区块信息
==========================================

Node.JS Example

Parameters:
::
	blockNumber：区块高度

Example:
::
	mc.getBlock(blockNumber).then((blockInfo) => {
		console.log(blockInfo);
	});

Results:
::
	blockInfo：某一区块信息
	
获取子链交易详情
=========================

Node.JS Example

Parameters:
::
	transactionHash：交易hash

Example:
::
	mc.getTransactionByHash(transactionHash).then((transactionInfo) => {
		console.log(transactionInfo);
	});

Results:
::
	transactionInfo：交易详情
	
获取子链账户余额
=========================

Node.JS Example

Parameters:
::
	addr：钱包地址

Example:
::
	mc.getBalance(addr).then((balance) => {
		console.log(balance);
	});

Results:
::
	data：子链账户余额（erc20最小单位）
	
获取子链详细信息
=========================

Node.JS Example

Example:
::
	mc.getMicroChainInfo().then((microChainInfo) => {
		console.log(microChainInfo);
	});;

Results:
::
	microChainInfo：子链信息
	
获取子链DAPP状态
=========================

Node.JS Example

Example:
::
	mc.getDappState().then((state) => {
		console.log(state);
	});;

Results:
::
	state：1//正常
	state：0//异常

获取Nonce
=========================

Node.JS Example

Parameters:
::
	addr：账户钱包地址

Example:
::
	mc.getNonce(addr).then((nonce) => {
		console.log(nonce);
	});;

Results:
::
	nonce：得到的nonce
	
获取子链DAPP合约实例
============================================

Parameters:
::
	dappContractAddress：dapp合约地址
	dappAbi：dapp合约的Abi对象

Example:
::
	var dapp = getDappInstance(dappContractAddress, dappAbi);

Results:
::
	dapp：dapp实例

获取交易Data
=========================

Parameters:
::
	method：方法 例 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 例['address','uint256']
	paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）

Example:
::
	var data = mc.getData(method,paramTypes,paramValues);

Results:
::
	data：data字符串


子链加签交易
=========================

Node.JS Example

Parameters:
::
	from：发送方的钱包地址
	microChainAddress：子链地址
	amount：交易金额
	dappAddress：dapp地址
	method：方法 例 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 例['address','uint256']
	paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）
	privateKey：发送方钱包私钥

Example:
::
	mc.sendRawTransaction(from, microChainAddress, amount, dappAddress, method, paramTypes, paramValues, privateKey).then((hash) => {
		console.log(hash);
	});

Results:
::
	hash：交易hash
	
子链转账
=========================

Node.JS Example

Parameters:
::
	from：发送方的钱包地址
	to：接收方的钱包地址
	amount：交易金额（erc20最小单位）
	privateKey：钱包私钥
	

Example:
::
	mc.transferCoin(from, to, amount, privateKey).then((hash) => {
		console.log(hash);
	});

Results:
::
	hash：交易hash
	
调用子链合约
=========================

Parameters:
::
	contractAddress：dapp合约地址
	param：例如合约中存在一个无参的方法getDechatInfo，则传入["getDechatInfo"];
     	     存在一个有参的方法getTopicList(uint pageNum, uint pageSize), 则传入["getTopicList", 0, 20]

Example:
::
	mc.callContract(contractAddress, param).then((data) => {
		console.log(data);
	});

Results:
::
	data：调用合约返回信息
	
提币（MOAC）
=========================

Parameters:
::
	addr：钱包地址
	amount：金额（单位为moac）
	privateKey：钱包私钥

Example:
::
	mc.redeemMoacMintToken(addr, amount, privateKey).then((hash) => {
		console.log(hash);
	});

Results:
::
	hash：交易hash

提币（ERC20）
=========================

Parameters:
::
	addr：钱包地址
	amount：金额（erc20最小单位）
	privateKey：钱包私钥

Example:
::
	mc.redeemErcMintToken(addr, amount,privateKey).then((hash) => {
		console.log(hash);
	});

Results:
::
	hash：交易hash


