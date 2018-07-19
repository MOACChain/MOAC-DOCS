A1、下载SCS代码（或准备一个SCS盒子）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A2、配置SCS配置文件userconfig.json，需要配置的内容如下
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. VnodeServiceCfg：这个SCS需要连接的vnode的ip和port。如架构图，每个SCS需要连接一个MOAC-VNODE来进行通讯，所以如果你没有自己部署一个节点，需要从官方渠道来设置连接一个可用的MOAC-VNODE
2. DataDir：SCS数据目录，以子链地址为文件夹存放子链数据。默认：./scsdata
3. LogPath：SCS日志目录，以天为单位存放日志。默认：./logs
4. Beneficiary：矿工收益账号。为了安全起见，我们建议采用与scsid不同的账号用来获取子链的收益（scsid将会在后面讲到）
5. VnodeChainId：母链ID，当前，测试环境ID是101，正式环境ID是99，请按需设置，不要设置其他ID
6. BondLimit：服务子链的押金上限。每个DAPP用户需要设置押金来为子链服务，矿工可以使用这个上限来选择不为押金高的子链服务。该押金将从register时候的保证金中扣除。（TODO）

A3、启动SCS
~~~~~~~~~~~

参数说明

-  “-p [密码]” scsid的密码，不设置默认密码为 moacscsofflineaccountpwd
-  “-rpcaddr [addr]” SCS开启rpc的ip
-  “-rpcport [port]” SCS开启rpc的port

启动后，SCS将会在当前目录下生成一个keystore目录，并在目录中新建一个账号，这个账号就是scsid，可在第一次启动时使用-p设置密码。如果想换一个scsid，则需要删除keystore中的内容重新启动。

SCS的rpc功能是提供给DAPP用户查询用的（MONITOR），如果是矿工，可以不开启这个功能。

A4、将这个SCS注册到SCS池子中去
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

SCS根据子链矿池智能合约subchainprotocolbase进行注册，并缴纳保证金。在等待一定时间之后（通常是母链50个block），就进入子链矿池，成为该子链的候选SCS节点。

只要性能允许，一个SCS可以参与多个子链的注册。

注册子链协议Javascript方法：（请注意，所有js方法需要和MOAC-VNODE
Console配合使用，之后的例子相同）

.. code:: javascript

    function protocolRegister(baseAddr,basePasswd,protocolAddr,scsAddr)
    {
        chain3.personal.unlockAccount(baseAddr, basePasswd,0);
        sendtx(baseAddr, protocolAddr, '10','0x4420e486000000000000000000000000' + scsAddr);
    }

【说明】：

-  baseAddr、basePasswd：moac主网端存在的一个账户及其密码，用来发送交易前账户解锁；
-  protocolAddr：子链矿池合约subchainprotocolbase地址；
-  scsAddr：scsid地址，放在“…/scsserver/scskeystore”目录下；
-  保证金：这里最低交10个moac，实际根据协议合约的最低保证金要求，保证金交的越多，可以参与的子链数量越多；
-  数据：‘0x4420e486’是子链矿池合约subchainprotocolbase中‘register(address
   scs)’通过hash算法Keccak256得到前4个字节；该函数带一个参数，每个参数占用32个字节，地址20字节，不足32字节则前补12字节00。这里不用修改。

另附1调用实例：

.. code:: javascript

    protocolRegister ('0x1b9ce7e4f1.......e38913a56cd986786',
    ‘123’,
    '0x09f0dfc09b......0b85e5189a7493671',
    'f687272ae00f8cea......37dd9be30329d8cf')//不带0x开头

另附2交易实例：

.. code:: javascript

    function sendtx(src, tgtaddr, amount, strData) {
        chain3.mc.sendTransaction(
            {
                from: src,
                value:chain3.toSha(amount,'mc'),
                to: tgtaddr,
                gas: "9000000",
                gasPrice: chain3.mc.gasPrice,
                data: strData
            });
            
        console.log('sending from:' +   src + ' to:' + tgtaddr  + ' with data:' + strData);
    }

注意：注册时缴纳的保证金，将在SCS被选中做子链的时候临时扣除，用于做假惩罚。

至此，矿工的前期工作全部完成，如果被选中参与子链的运行，将会在子链刷新之后，在Beneficiary中看到收益。
