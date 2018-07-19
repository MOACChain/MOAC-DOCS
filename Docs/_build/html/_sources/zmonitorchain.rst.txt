Monitor模块
-----------

Monitor是一个特殊的SCS节点，它是一种模式，DAPP用户可以通过这个节点来监控自己的子链运行状态和业务数据。

SCS启动时的RPC参数就是为这个模块设定的。

Monitor不参与子链共识，因此只能查看，不能修改数据。

即使子链已经运行，Monitor也能注册加入。

Monitor
SCS启动后，DAPP用户通过调用子链控制合约subchainbase中的registerAsMonitor方法使Monitor
SCS接入子链同步数据。

另附1registerAsMonitor调用方法：

.. code:: javascript

    function subchainRegisterAsMonitor (dappAddr,dappPasswd,subchainAddr,scsAddr)
    {
        chain3.personal.unlockAccount(dappAddr, dappPasswd,0);
        sendtx(dappAddr, subchainAddr, '0', '0x4e592e2f000000000000000000000000' + scsAddr );
    }

【说明】： \* dappAddr、dappPasswd：Dapp用户用来发送交易前账户解锁； \*
subchainAddr：部署子链合约得到的合约地址； \*
scsAddr：scsid地址，放在“…/scsserver/scskeystore”目录下； \*
数据：‘0x4e592e2f’是子链控制合约subchainbase中‘registerAsMonitor(address
monitor)’通过hash算法Keccak256得到前4个字节；该函数带一个参数，每个参数占用32个字节，地址20字节，不足32字节则前补12字节00。这里不用修改。

另附2调用实例：

.. code:: javascrit

    subchainRegisterAsMonitor ('0x1b9ce7e4f156c1......913a56cd986786',
    ‘123’,
    '0x09f0dfc09bee......b85e5189a7493671',
    'f687272ae00f8cea01b8cedf37dd9be30329d8cf')//不带0x开头
