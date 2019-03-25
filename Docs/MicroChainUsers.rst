MicroChain Users
----------------


There are three types of users in MicroChains:

-  A、 SCS miner：run SCS and join the MicroChain consensus to get
   MicroChain rewards;
-  B、 DAPP developer：develop DAPP and deploy its Smart Contract on the
   MicroChain；
-  C、 VNODE proxy：provide a VNODE-PROXY to MicroChain.

In the MOAC ecosystem, these players have different responsibility and
rewards:

-  A. SCS miner: A SCS miner needs to deposit some MOAC to join a
   MicroChain mining pool. When a SCS was chosen by one MicroChain and
   start mining, it can receive MicroChain mining rewards as long as the
   MicroChain is running. The rewards usually was given as MOAC and
   shows up after a flush of the MicroChain. The deposit will be
   deducted if the SCS keeps sending dishonest transactions or lost
   connections very often.

-  B. DAPP developer: When the DAPP developer deploy his Contract on the
   MicroChain, he need to deposit enough MOAC to pay the SCS miners for
   their work. These MOAC will be distributed to the SCSs after each
   MicroChain block generated, and saved to the MotherChain after
   MicroChain flush.

-  C. VNODE proxy: Need to pay deposit to register as proxy. It can get
   rewards from participting in the MicroChain and from transfer data of
   MicroChains.

For the details of the deposit amount and how to get rewards, please
refer to the smart contracts defined them:

1. subchainprotocolbase
2. subchainbase
3. vnodeprotocolbase

We suggested the users use official pools for our testing, so we can
find bugs and problems in the testnet. In the future, users can form
their own pools and benefit from them.

### B6、DAPP智能合约的查询（RPC模式）
DAPP智能合约查询的前提是使用SCS MONITOR方式接入到子链中，并开启RPC。当前提供的接口如下（go语言实现）：

数据定义：
```go
type Args struct {
    Sender       common.Address      // Dapp创建者
    SubChainAddr common.Address
}
type ArgsData struct {
    Sender       common.Address     // Dapp创建者
    SubChainAddr common.Address
    Func         string                            // eg:"SetData()", "rpcGetData()"
}
```

**RPC接口1：获取SCSID**

接口：func GetScsId(args *Args, reply *common.Address) error
调用方法：
```go
client, err := rpc.DialHTTP("tcp", serverAddress+":"+serverPort)
var scsid common.Address
client.Call("ScsRPCMethod.GetScsId", Args{}, &scsid)
```

**RPC接口2：获取Dapp创建者在子链的nonce**

接口：func GetNonce(args *Args, reply *uint64) error
调用方法：
```go
args := Args{sender, subChainAddr}
var noce uint64//
client.Call("ScsRPCMethod.GetNonce", args, & noce)
```

**RPC接口3：调用Dapp合约某个无参函数并获得返回值**

如下举例，调用GetData()并获取返回值存到replyData
接口：func GetData(argsData *ArgsData, reply *[]byte)
调用方法：
```go
var replyData []byte
argsData := ArgsData{sender, subChainAddr, "GetData()"}
client.Call("ScsRPCMethod.GetData", argsData, &replyData)
```

**RPC接口4：获取Dapp合约状态**

返回值0：未创建；1：已成功创建
接口：func (scs *ScsRPCMethod) GetDappState(args *Args, reply *uint64) error 
调用方法：
```go
args := Args{sender, subChainAddr}
var reply uint64
client.Call("ScsRPCMethod.GetDappState", args, &reply)
```

**RPC接口5：GetContractInfo：**

接口提供子链DAPP智能合约全局变量查询，接口协议使用http。
请求参数：
```go
type ContractInfoReq struct {
    Reqtype      int//查询类型0: 查看合约全部变量 , 1: 查看合约某一个数组变量 , 2: 查看合约某一个mapping变量 , 3: 查看合约某一个结构体变量, 4: 查看合约某一简单类型变量（单倍长度存储的变量）, 5: 查看合约某一变长变量（如string、bytes）
    Key          string//64位定长十六进制字符串，查询的变量在合约里面的index ，查询全部变量时可以不填
    Position     string//64位定长十六进制字符串，当Reqtype==1时，Position为数组里第几个变量（从0开始）；当Reqtype==2时，Position为mapping下标
    Structformat []byte//当出现结构体变量时，此变量描述结构，结构体只允许出现1-single（简单类型变量单倍长度存储的变量）, 2-list（简单类型数组变量）, 3-string变长变量（如string、bytes），若结构变量为ContractInfoReq，Structformat = []byte{‘1’,’3’,’3’,’3’}
}

type GetContractInfoReq struct {
    SubChainAddr common.Address//合约地址
    Request      []ContractInfoReq//查询合约全部变量数据，数据太大，可以选择查询需要的几个变量
}
```
返回参数：
```go
type ContractInfo struct {
    Balance  *big.Int
    Nonce    uint64
    Root     common.Hash
    CodeHash []byte
    Code     []byte
    Storage  map[string]string
}
```
