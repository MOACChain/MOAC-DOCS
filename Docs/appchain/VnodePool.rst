.. _vnode-pool:

VNODE pool
^^^^^^^^^^

VNODE pool is used to connect the AppChain with BaseChain. 
The contract is named VnodeProtocolBase and need to be deployed on the BaseChain.
The VNODE in the pool will provide AppChain data transfer service and can receive rewards from that.

The Node.Js example is as follows:
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

After successfully deployed and retuned with an address, the vnode pool can be accessed as a usual contract on the BaseChain.  
        
VNODE join the pool
------------------------

修改vnode目录配置文件vnodeconfig.json: VnodeBeneficialAddress里设置收益账号：  0xf103bc1c054babcecd13e7ac1cf34f029647b08c

这个账号也作为vnode的address，节点池中对应这个vnode的唯一编号

The VNODE needs to call the VNODE pool contract's register method to join the pool.

::
    Parameters:
    from： A valid account;   
    value：deposit required to join the pool, this value needs to be higher than the value set in the VNODE pool contract;
    to: VNODE pool contract address;  
    data: register(address,string);
    
关于data传递调用register参数说明: 
::
    根据ABI chain3.sha3("register(address,string)") = 0x32434a2e90725ed590daff07a244305001c58c49f7bef73ce5e7249acf69f561 
        取前4个字节 0x32434a2e  
    第一个参数address 传  vnodeconfig.json的VnodeBeneficialAddress  （前面补24个0， 凑足32个字节）  
        000000000000000000000000f103bc1c054babcecd13e7ac1cf34f029647b08c
    第二个参数string传 vnode提供给应用链的调用地址 192.168.10.209:50062   
    端口号对应vnodeconfig.json的VnodeServiceCfg
        string数据类型 + string数据长度 + string内容
        string数据类型： 0000000000000000000000000000000000000000000000000000000000000040
        string内容： data = new Buffer('192.168.10.209:50062', 'utf8').toString('hex'); 
            后面补0， 凑足32个字节   3139322e3136382e31302e3230393a3530303632000000000000000000000000
        string数据长度: 3139322e3136382e31302e3230393a3530303632    20字节
            0000000000000000000000000000000000000000000000000000000000000014
        data = '0x32434a2e000000000000000000000000f103bc1c054babcecd13e7ac1cf34f029647b08c000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000143139322e3136382e31302e3230393a3530303632000000000000000000000000'     
    也可以调用合约对象的getData方法获得data参数
        data = VnodeProtocolBase.register.getData(via, '192.168.10.209:50062')

调用示例:
::
    > amount = chain3.toSha(5,'mc')
    > data = '0x32434a2e000000000000000000000000f103bc1c054babcecd13e7ac1cf34f029647b08c000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000143139322e3136382e31302e3230393a3530303632000000000000000000000000';
    > chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
    > chain3.mc.sendTransaction({ from: chain3.mc.accounts[0], value:amount, to: '0x22f141dcc59850707708bc90e256318a5fe0b928', gas: "5000000", gasPrice: chain3.mc.gasPrice, data: data });

验证： 访问Vnode节点池合约的vnodeCount
::
    > VnodeProtocolBase.vnodeCount()
    > chain3.mc.getStorageAt("0x22f141dcc59850707708bc90e256318a5fe0b928",0x02)   // 注意vnodeCount变量在合约中变量定义的位置（16进制）
