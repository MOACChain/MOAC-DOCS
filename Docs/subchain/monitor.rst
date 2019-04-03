Monitoring the MicroChain
^^^^^^^^^^^^^^^^^^^^^^^^^^^^


SCS Monitor
----------------------

SCS Monitor is a SCS node monitoring MicroChain status. MicroChain owner
can use this SCS node to monitor MicroChain status and get data from
MicroChain. Only the owner of MicroChain can add monitors.

Users can use RPC settings to get information fron SCS monitor.

SCS monitor can only read data from MicroChain, not submitting any data.
It does not join the consensus and cannot get rewards from MicroChain.

SCS monitor can monitor MicroChain after it starts. The MicroChain user
need to call the "registerAsMonitor" method in MicroChain contract to
register it. 


子链启动的方式与scs区别在于参数不同，主要定义了rpc接口的访问控制
::	
	scsserver-windows-4.0-amd64 --password "123456" --rpcdebug --rpcaddr 0.0.0.0 --rpcport 2345 --rpccorsdomain "*"

子链运行后，Monitor可以调用子链控制合约subchainbase中的registerAsMonitor方法进行注册

调用registerAsMonitor参数说明:	
::	
	根据ABI chain3.sha3("registerAsMonitor(address)") = 0x4e592e2f6fc52405522577d357d824c923989a62e4916d9b689311d8b2a6192c 
		取前4个字节 0x4e592e2f  
	参数address传scs keystore文件中的地址 （前面补24个0， 凑足32个字节）  
	data = '0x4e592e2f000000000000000000000000d135afa5c8d96ba11c40cf0b52952d54bce57363'		
	
	注意registerAsMonitor不同版本参数
	> data = contractInstance.registerAsMonitor.getData('0xd135afa5c8d96ba11c40cf0b52952d54bce57363','127.0.0.1')   
	

Example:
::
	> amount = chain3.toSha(1,'mc')
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> data = '0x4e592e2f000000000000000000000000d135afa5c8d96ba11c40cf0b52952d54bce57363';
	> chain3.mc.sendTransaction({ from: chain3.mc.accounts[0], value:amount, to: subchainaddr, gas: "5000000", gasPrice: chain3.mc.gasPrice, data: data });

验证： 观察SCS monitor concole界面开始同步子链区块，或者调用子链合约的getMonitorInfo方法
::
	> contractInstance = SubChainBaseContract.at('0xb877bf4e4cc94fd9168313e00047b77217760930')
	
	> contractInstance.getMonitorInfo.call()

SCS RPC methods
----------------------

根据子链启动的参数，用户可以访问对应端口，调用接口获取子链相关数据

以调用接口GetScsId为列，获得当前的scs编号，即scs keystore文件中的地址。

关于rpc http的接口访问，可以用类似postman之类的工具进行快捷测试
::
	header设置：
		Content-Type = application/json
		Accept = application/json
		
	Body设置：
		{"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetScsId","params":{}}
		
	post返回结果：
		{
			"jsonrpc": "2.0",
			"id": 0,
			"result": "0xd135afa5c8d96ba11c40cf0b52952d54bce57363"
		}
		
同时也可以通过nodejs的方式调用
::
	> request = require('request');
	> url  = "http://127.0.0.1:2345/rpc";  
	> data = {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetScsId","params":{}};
	> request({ url: url, method: "POST", json: true, body: data, headers: {"Content-Type": 'application/json', "Accept": 'application/json'}}, function(error, response, result) {if (!error && response.statusCode == 200) {console.log(result)}});


以下是几个常用的RPC接口及调用body示例

GetNonce：获得子链的nonce，这是调用子链DAPP合约的必要参数之一，每当子链交易发送后会自动加1
::
	SubChainAddr: 子链合约地址
	Sender：查询账号， 每个账号在子链有不同的nonce
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetNonce",
				"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
					"Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
				 }
		  }
	
GetBlockNumber：获得当前子链的区块高度
::
	SubChainAddr: 子链合约地址
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetBlockNumber",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09"}
		  }
	
GetBlock:  获得当前子链的指定的区块信息
::
	SubChainAddr: 子链合约地址
	Sender：查询账号
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetBlock",
			"params":{"number":1000,"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09"}
		  }

GetSubChainInfo：获得当前子链的信息
::
	SubChainAddr: 子链合约地址
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetSubChainInfo",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09"}
		  }

GetBalance：获得对应账号在子链中的余额
::
	SubChainAddr: 子链合约地址
	Sender：查询账号
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetBalance",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
				"Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
				}
		  }
	
GetDappState：获得子链基础合约合约的状态
::
	SubChainAddr: 子链合约地址
	Sender：子链合约地址创建者地址
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetDappState",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
				"Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
			 }
		  }
	
getContractInfo：获得子链基础合约信息（不推荐）
::
	SubChainAddr: 子链合约地址
	Reqtype:  查询类型 0: 查看合约全部变量 , 1: 查看合约某一个数组变量 , 2: 查看合约某一个mapping变量 , 3: 查看合约某一个结构体变量, 4: 查看合约某一简单类型变量（单倍长度存储的变量）, 5: 查看合约某一变长变量（如string、bytes）
	Storagekey: 十六进制字符串，查询的变量在合约里面的index ，查询全部变量时可以不填
	Position: 十六进制字符串，当Reqtype==1时，Position为数组维度（从0开始）；当Reqtype==2时，Position为mapping下标
	Structformat：针对结构体变量，1：single（简单类型变量单倍长度存储的变量）, 2：list（简单类型数组变量）3：string变长变量（如string、bytes），若结构变量为ContractInfoReq，Structformat = []byte{‘1’,’3’,’3’,’3’}
	
	获取合约 index 1 的 address 对应 Body: 
	{"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetContractInfo",
	"params":{"subChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
		"Request":[{"Reqtype":4,
			  "Storagekey":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1],
			  "Position":[],
			  "Structformat":[]}
			  ]
		}
	}


AnyCall: 获取dapp合约函数的返回值，**调用此接口前必须将dapp注册入dappbase**

Params： 第一个参数是调用的方法，之后是方法传入参数
::
	SubChainAddr: 子链合约地址
	Sender：查询账号
	DappAddr:子链业务逻辑地址
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.AnyCall",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
				"DappAddr":"0xcc0D18E77748AeBe3cC6462be0EF724e391a4aD9",
				"Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"， "Params" :["funcA", "param1", param2]
				}
		  }

GetBlocks: 获取某一区间内的区块信息
::
	SubChainAddr: 子链合约地址
	Start: 开始block
	End： 结束block
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetBlocks",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09"
				"Start":10, "End":20}
		  }

GetTransactionByNonce: 通过账号和Nonce获取子链的tx信息
::
	SubChainAddr: 子链合约地址
	Sender：查询账号
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetTransactionByNonce",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
				"Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"， "Nonce":9,
				}
		  }

GetTransactionByHash: 通过交易hash获取子链的tx信息
::
	SubChainAddr: 子链合约地址
	Hash: 交易hash
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetTransactionByHash",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
				"Hash":"0x87e369172af1e817ebd8d63bcd9f685a513a6736fsne3lkgkvu65kkwlcd"
				}
		  }

GetReceipts: 通过账号和Nonce获取子链的tx执行结果
::
	SubChainAddr: 子链合约地址
	Sender：查询账号
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetReceipts",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
				"Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"， "Nonce":9
				}
		  }

GetDappAddrList: 通过subchainaddr获取子链内所有多合约的地址列表，需要子链业务逻辑合约调用基础合约registerDapp方法后才能生效，具体请参见“母子链货币交互简介”中的示例
::
	SubChainAddr: 子链合约地址
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetDappAddrList",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
				}
		  }

GetExchangeInfo: 获取指定数量的母子链正在充提信息
::
	SubChainAddr: 子链合约地址
	EnteringRecordIndex：充值记录起始位置
	EnteringRecordSize：充值记录获取数量
	RedeemingRecordIndex：提币记录起始位置
	RedeemingRecordSize：提币记录获取数量
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetExchangeInfo",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
			"EnteringRecordIndex": 0, "EnteringRecordSize": 5, 
			"RedeemingRecordIndex": 0, "RedeemingRecordSize", 5}
		  }

返回中，XXXRecordCount是指总数量

GetExchangeByAddress: 获取指定账号指定数量的充提信息
::
	SubChainAddr: 子链合约地址
	EnteringRecordIndex：充值中记录起始位置
	EnteringRecordSize：充值中记录获取数量
	RedeemingRecordIndex：提币中记录起始位置
	RedeemingRecordSize：提币中记录获取数量
	EnterRecordIndex：充值完成记录起始位置
	EnterRecordSize：充值完成记录获取数量
	RedeemRecordIndex：提币完成记录起始位置
	RedeemRecordSize：提币完成记录获取数量
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetExchangeInfo",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
			"EnteringRecordIndex": 0, "EnteringRecordSize": 5, 
			"RedeemingRecordIndex": 0, "RedeemingRecordSize": 5,
			"EnterRecordIndex": 0, "EnterRecordSize": 5, 
			"RedeemRecordIndex": 0, "RedeemRecordSize": 5}
		  }

返回中，XXXRecordCount是指总数量

GetTxpool：获取子链池子信息
::
	SubChainAddr: 子链合约地址
	Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetExchangeInfo",
			"params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09"}
		  }

