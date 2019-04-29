Restful APIs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Admin
---------------------------

auth
=====================

API authorization

Request to access the token of calling other APIs.

Method: auth

Parameters:
::
	account:  The account to be authorized
	pwd:  The account password
	
Example：
::
	POST: http://139.198.126.104:8080/auth
	BODY：account=******&pwd=******

Results	
::	
	{
		"success": true,
		"message": "",
		"data": "token内容"
	}

	
Accounts
---------------------------

register
=====================

Method: register

Account Registration.

Parameters:
::
	pwd:  Account password
	token:  Return token by the auto command
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/account/v1.0/register
	BODY：pwd=********&token=********************************

Results	
::	
	{
		"success": true,
		"message": "",
		"address": Account public address,
		"encode": Encoded Account,
		"keystore": Account keystore,
		"privateKey": Account private key,
	}
	
login
=====================

Account Log in

Method: login

Parameters:
::
	address:  Account public address,
	pwd:  Account password,
	encode:  Encoded Account
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/account/v1.0/login
	BODY：address=0x********&pwd=*****&encode=*******&token=************

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 账户地址
	}	

import
=====================

Account Import

Method: import   将账户通过keystore导入系统

Parameters:
::
	address:  账户地址
	pwd:  账户密码
	keystore:  账户keystore
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/account/v1.0/import
	BODY：address=0x********&pwd=*****&keystore={*******}&token=************

Results	
::	
	{
		"success": true,
		"message": "",
		"address": 账户地址,
		"encode": 账户加密串,
		"privateKey": 账户私钥
	}	
	
MotherChain
---------------------------


getBalance
=====================

Method: getBalance

Get the Account Balance in moac

Parameters:
::
	vnodeip:  vnode server IP address
	vnodeport:  vnode server port
	address:  Account address
	token:  token used to access the API, returned by Auto command.
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getBalance
	BODY：vnodeip=127.0.0.1&vnodeport=8545&address=0x******&token=*****************

Results	
::	
	{
		"success": true,
		"message": "",
		"data": balance of Addcount in moac	
	}
	
getBlockNumber
=====================

Get Block Number

Method: getBlockNumber

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getBlockNumber
	BODY：vnodeip=127.0.0.1&vnodeport=8545&token=***************

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 区块高度
	}	
	
getBlockInfo
=====================

Get Block Information

Method: getBlockInfo

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	block:  区块号或者区块hash
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getBlockInfo
	BODY：vnodeip=127.0.0.1&vnodeport=8545&block=2002326&token=******************

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 区块信息
	}	

getTransactionByHash
=====================

Get Transaction Information

Method: getTransactionByHash

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	hash:  交易hash
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getTransactionByHash
	BODY：vnodeip=127.0.0.1&vnodeport=8545&hash=0x**&token=******************

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 交易明细
	}

getTransactionReceiptByHash
=====================

Get Transaction Receipt

Method: 

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	hash:  交易hash
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getTransactionReceiptByHash
	BODY：vnodeip=127.0.0.1&vnodeport=8545&hash=0x**&token=******************

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 交易详情
	}	
	
sendRawTransaction
=====================

Send a Transaction

Method: 

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	from:  源账号地址
	to:  目标账号地址
	amount:  数量（单位 moac）
	method:  dapp合约方法 比如：buyMintToken(uint256)
	paramtypes:  dapp合约方法对应的参数类型 比如：["uint256"]
	paramvalues:  dapp合约方法对应的参数值   比如：[100000000]
	privatekey:  源账号私钥 （传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	gasprice: 可选参数，默认gasprice为chain3的gasPrice，当交易堵塞时，需要传原交易的110%进行覆盖。
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/sendRawTransaction
	BODY：vnodeip=127.0.0.1&vnodeport=8545&from=0x**&to=0x***&amount=10&method=buyMintToken(uint256)&paramtypes=["uint256"]&paramvalues=[100000000]&privatekey=0x**&token=*******

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	

callContract
=====================

Call a Smart Contract
Method: callContract

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	contractaddress:  合约地址
	method:  dapp合约方法 比如：buyMintToken(uint256)
	paramtypes:  dapp合约方法对应的参数类型 比如：["uint256"]
	paramvalues:  dapp合约方法对应的参数值   比如：[100000000]
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/callContract
	BODY：vnodeip=127.0.0.1&vnodeport=8545&contractaddress=0x*****&method=buyMintToken(uint256)&paramtypes=["uint256"]&paramvalues=[100000000]0x****&token=***************

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 调用合约返回结果
	}		

transferErc
=====================

Transfer ERC20 token

Method: transferErc

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	from:  源账号地址
	to:  目标账号地址
	contractaddress:  erc20合约地址
	amount:  erc20代币数量
	privatekey:  源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/transferErc
	BODY：vnodeip=&vnodeport=&from=0x**&to=0x**&contractaddress=0x**&amount=10&privatekey=0x**&token=*******

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	
	
getErcBalance
=====================

Get the Balance of ERC20 token
Method: getErcBalance

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	address:  账户地址
	contractaddress:  erc20合约地址
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getErcBalance
	BODY：vnodeip=127.0.0.1&vnodeport=8545&address=0x*****&contractaddress=0x**&token=*********

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 余额（最小精度，10进制）
	}	
	
ercApprove
=====================

Approve ERC20 token to MicroChain

This is required before atomic swap of ERC20 token with MicroChain token.

Method: ercApprove

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	address:  账户地址
	amount:  授权erc20数量
	privatekey:  账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	microchainaddress			子链地址
	contractaddress:  erc20合约地址
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/ercApprove
	BODY：vnodeip=127.0.0.1&vnodeport=8545&address=0x*****&amount=***&privatekey=0x***&microchainaddress=0x***&contractaddress=0x**&token=*********

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	


buyErcMintToken
=====================

Atomic Swap the ERC20 token with MicroChain token
This take effects after flush.

Method:    注：前提是erc20对应数量已经授权给子链

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	address:  账户地址
	privatekey:  源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	microchainaddress:  子链地址
	method:  dapp合约方法 默认为：buyMintToken(uint256)
	paramtypes:  dapp合约方法对应的参数类型 默认为：["uint256"]
	paramvalues:  dapp合约方法对应的参数值   比如：[100000000]
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/buyErcMintToken
	BODY：vnodeip=&vnodeport=&address=0x**&privatekey=0x**&microchainaddress=0x**&method=buyMintToken(uint256)&paramtypes=["uint256"]&paramvalues=[100000000]&token=****

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	

buyMoacMintToken
=====================

Atomic Swap moac with MicroChain token 

moac兑换子链原生币

Method: buyMoacMintToken

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	address:  账户地址
	privatekey:  源账号私钥
	pwd： 账户密码
	encode：账户加密串
	microChainaddress:  子链地址
	method:  dapp合约方法 默认为：buyMintToken(uint256)
	paramtypes:  dapp合约方法对应的参数类型 默认为：["uint256"]
	paramvalues:  dapp合约方法对应的参数值   比如：[100000000]
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/buyMoacMintToken
	BODY：vnodeip=&vnodeport=&address=0x**&privatekey=0x**&microChainaddress=0x**&method=buyMintToken(uint256)&paramtypes=["uint256"]&paramvalues=[100000000]&token=****

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}		
	
MicroChain
---------------------------

getBlockNumber
=====================

Get MicroChain Block Number

Method: getBlockNumber

Parameters:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getBlockNumber
	BODY：microip=127.0.0.1&microport=8546&microchainaddress=0x***&token=***********
Results	
::	
	{
		"success": true,
		"message": "",
		"data": 子链区块高度
	}	
	
getDappAddrList
=====================

Get the Dapp addresses on the MicroChain

Method: 

Parameters:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getDappAddrList
	BODY：microip=127.0.0.1&microport=8546&microchainaddress=0x***&token=***********
Results	
::	
	{
		"success": true,
		"message": "",
		"data": 子链dapp地址列表（按合约注册次序）
	}		
	
getBlock
=====================

Get MicroChain Block Information

Method: getBlock

Parameters:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	blocknum:  块号
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getBlock
	BODY：microip=127.0.0.1&microport=8546&microchainaddress=0x***&blocknum=*****&token=***********

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 子链区块信息
	}	
	

getTransactionByHash
=====================

Get the Transaction Information by HASH

Method: 

Parameters:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	hash:  交易hash
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getTransactionByHash
	BODY：microip=127.0.0.1&microport=8546&microchainaddress=0x***&hash=0x**&token=***********

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 子链交易信息
	}	
	
getTransactionReceiptByHash
=====================

获得子链对应Hash的交易明细
Method: getTransactionReceiptByHash

Parameters:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	hash:  交易hash
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getTransactionReceiptByHash
	BODY：microip=127.0.0.1&microport=8546&microchainaddress=0x***&hash=0x**&token=***********

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 子链交易明细
	}	
		

getBalance
=====================
获取子链账户余额
Method: getBalance

Parameters:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	address:  账户地址
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getBalance
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x*****&address=0x*****&token=**************

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 账户余额
	}	

	
transferCoin
=====================
子链原生币转账
Method: transferCoin

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	via:  子链收益账号
	from:  源账户地址
	to:  目标账户地址
	amount:  原生币数量
	privatekey:  源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/transferCoin
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x**&via=0x**&from=0x**&to=0x**&amount=**&privatekey=0x***&token=*****

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	

sendRawTransaction
=====================
子链加签交易  
Method: sendRawTransaction   调用dapp合约涉及修改数据的方法

Parameters:
::
	vnodeip: vnode节点地址
	vnodeport:  vnode节点端口
	microip:  monitor节点地址
	microport:  monitor节点端口
	from: 发送交易账户地址
	microchainaddress:  子链SubChain地址
	via:  子链收益账号
	amount:	 payable对应金额	
	dappaddress:  dapp合约地址
	method:  dapp合约方法 比如：buyMintToken(uint256)
	paramtypes:  dapp合约方法对应的参数类型 比如：["uint256"]
	paramvalues:  dapp合约方法对应的参数值   比如：[100000000]
	privatekey: 源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	token: auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/sendRawTransaction
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&from=0x**&microchainaddress=0x***&via=0x**&amount=**&dappaddress=0x***&method=buyMintToken(uint256)&paramtypes=["uint256"]&paramvalues=[100000000]&privatekey=0x***&token=*****

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 子链交易hash
	}
	
callContract
=====================

子链合约调用 
Method: callContract 针对public方法和变量，不涉及数据修改

Parameters:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	dappaddress:  dapp合约地址
	data:  字符串数组，如合约方法getTopicList(uint pageNum, uint pageSize)，则传入["getTopicList", "0", "20"]
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/callContract
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x*****&dappaddress=0x**&data=&token=********

Results	
::	
	{
		"success": true,
		"message": "",
		"data": 合约返回结果
	}	
	
redeemErcMintToken
=====================

Redeem to ERC20 

Method:      原生币转erc20

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	microipHmonitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	dappbaseaddress:  dappbase合约地址
	via:  子链收益账号
	address:  提币账户地址
	amount:  提取原生币数量
	privatekey:  源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/redeemErcMintToken
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x**&dappbaseaddress=0x**&via=0x**&address=0x**&amount=**&data=****&privatekey=0x**&token=********

Results:
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	
	
redeemMoacMintToken
=====================

Redeem the MicorChain token to MOAC 

Method:      Swap MicroChain token with moac

Parameters:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	microipHmonitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	dappbaseaddress:  dappbase合约地址
	via:  子链收益账号
	address:  提币账户地址
	amount:  提取原生币数量
	privatekey:  源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	token:  auth返回的授权token
	
	
Example：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/redeemMoacMintToken
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x**&dappbaseaddress=0x**&via=0x**&address=0x**&amount=**&data=****&privatekey=0x**&token=********

Results:
::	
	{
		"success": true,
		"message": "",
		"data": transaction hash
	}	