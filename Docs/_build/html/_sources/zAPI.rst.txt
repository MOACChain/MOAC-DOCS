环境安装
^^^^^^^^

nodejs安装
''''''''''

::

    https://nodejs.org/

下载nodejs安装即可, node -v查看安装的版本，npm -v 查看npm版本。
建议对nodejs进行简单的环境配置, 同时下载并安装git， 并通过git
--version查看git版本。

进行chain3库安装
''''''''''''''''

::

    npm install chain3 -g

    注意：某些情况下，Windows下需要用“以管理员身份运行”进入cmd界面，才能使用npm
    install 命令

API使用准备
'''''''''''

.. code:: javascript

    var Chain3 = require('chain3');
    var chain3 = new Chain3();
    chain3.setProvider(new chain3.providers.HttpProvider('http://xxx.xxx.xxx.xxx:xxxx'));

API介绍
'''''''

1. chain3.version.network：查看当前MOAC网络ID
                                             

.. code:: javascript

    var networkId = chain3.version.network;
    console.log('network id:'+ networkId);

2. chain3.setProvider: 设置API访问MOAC节点方式
                                              

.. code:: javascript

    chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));

3. chain3.sha3(string,options)
                              

参数：string：传入的需要使用Keccak-256 SHA3算法进行哈希运算的字符串
options：可选项，如果要解析的是hex格式的十六进制字符串，需要设置为hex

.. code:: javascript

    var hash = chain3.sha3("the string to be hashed");
    console.log(hash);
    var hashOfHash = chain3.sha3(hash,{encoding:'hex'});
    console.log(hashOfHash);

4. chain3.net.peerCount: 返回连接节点已连上的其他moac节点的数量
                                                               

.. code:: javascript

    var peerCount = chain3.net.peerCount;
    console.log(peerCount);

5. chain3.net.listening: 返回连接节点的listen状态
                                                 

.. code:: javascript

    var listenState = chain3.net.listening;
    console.log(listenState);

6. chain3.mc.coinbase: 节点配置的，如果挖矿成功奖励发送到该地址
                                                               

.. code:: javascript

    var nodeCoinbase = chain3.mc.coinbase;
    console.log(nodeCoinbase);

7. chain3.mc.mining: 节点的挖矿状态
                                   

.. code:: javascript

    var miningState = chain3.mc.mining;
    console.log(miningState);  //true or false

8. chain3.mc.accounts: 节点持有的账户列表
                                         

.. code:: javascript

    var nodeAccounts = chain3.mc.accounts;
    console.log(nodeAccounts);

9. chain3.mc.blockNumber: 当前区块号
                                    

.. code:: javascript

    var nowBlockNumber = chain3.mc.blockNumber;
    console.log(nowBlockNumber);

10. chain3.mc.getBlockTransactionCount: 指定区块的交易数量
                                                          

.. code:: javascript

    var transactionCount = chain3.mc.getBlockTransactionCount(96160);
    console.log(transactionCount);

11. chain3.mc.getBalance: 给定地址的余额
                                        

.. code:: javascript

    var balance = chain3.mc.getBalance("0x36eaa71d7383be53cb600743aad08a55222a4915");
    console.log("getBalance1" + balance); //instanceof BigNumber
    console.log("getBalance2" + balance.toString(10));
    //输出结果： getBalance1:3.04527226722e+21  
    //         getBalance2:3045272267220000000000

12. chain3.mc.defaultBlock: 默认块设置
                                      

.. code:: javascript

    var defultBlock = chain3.mc.defaultBlock;
    console.log("defaultBlock" + defultBlock);
    //默认值是latest，即最近刚出的区块
    chain3.mc.defaultBlock = 123;  //传入一个值来覆盖默认配置
    console.log("defaultBlock" + defultBlock);

13. chain3.mc.gasPrice: 当前gas价格
                                   

.. code:: javascript

    var gasPrice = chain3.mc.gasPrice;//该值由最近几个块的gas价格的中值决定
    console.log(gasPrice.toString(10));

14. chain3.mc.estimateGas: 执行一个消息调用或交易，返回使用的gas量
                                                                  

.. code:: javascript

    var result = chain3.mc.estimateGas({
     to :"0xf7ebc6b854a202efe08e91422a44ba2161ed50dc",
     data: '0x23455654'
        //gas: 11,          //可选参数，设定gas
        //gasPrice: 11      //可选参数，设定gasPrice
    });
    console.log('estimateGas  :'+ result);
    //输出结果：gasprice :20000000000
    //        estimateGas :1273

15. chain3.mc.getCode: 获取指定地址的代码（地址合约编译后的字节代码）
                                                                     

.. code:: javascript

    var code  = chain3.mc.getCode("0x0000000000000000000000000000000000000065");//contract address

16. chain3.mc.syncing: 如果正在同步，返回同步对象；否则返回false
                                                                

.. code:: javascript

    var sync = chain3.mc.syncing;
    console.log('syncing  :'+ sync );
    //如果正在同步，返回同步开始区块号、节点当前正在同步的区块号、预估要同步到的区块号

17. chain3.mc.getTransaction: 返回指定交易哈希值的交易
                                                      

.. code:: javascript

    var blockHash = "0xbeca9c6a3a2f7bde119193e802f9506cc0ae58f23aca59f7ac8bf98e4e2242b5";
    var transaction = chain3.mc.getTransaction(blockHash);
    console.log('get transaction:'+ JSON.stringify(transaction));

18. chain3.mc.getBlock: 返回区块号或区块哈希值所对应的区块
                                                          

.. code:: javascript

    var getTheBlock = chain3.mc.getBlock(96190);
    console.log('get the block  :'+ JSON.stringify(getTheBlock ));

//TODO 稍后更新
