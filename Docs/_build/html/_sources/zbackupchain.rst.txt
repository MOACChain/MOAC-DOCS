Backup模块
----------

Backup其实是一个普通的SCS节点。但Backup不在子链生成时出现，而是在子链运行过程中按照一定规则加入到子链中来。Backup的目的是维持一个子链长久运行。
当前，Backup需要手动加入到队列中，方法如下：

.. code:: javascript

    function testregisterAdd(dappAddr,dappPasswd,subchainAddr)
    {
            chain3.personal.unlockAccount(dappAddr,dappPasswd,0);
            sendtx(dappAddr, subchainAddr, '0','0xbe93f1b30000000000000000000000000000000000000000000000000000000000000020');
    }

【说明】： \* dappAddr、dappPasswd：Dapp用户用来发送交易前账户解锁； \*
subchainAddr：部署子链合约得到的合约地址； \*
数据：'0xbe93f1b3'是子链控制合约subchainbase中‘registerAdd()’通过hash算法Keccak256得到前4个字节；一次性最大添加32个scs作为backup

另附1【参考实例】：

.. code:: javascript

    subchainRegisterAdd('0x1b9ce7e4f156c......8913a56cd986786',
    ‘123’,
    '0x6f5b81365fb1......907fa536cc78749af2c')

通过registerAdd，候选SCS加入Backup，作为Backup的SCS处于运行状态并同步其他SCS节点的区块数据，同步完成后通知子链完成加入；此时可参与子链的相关业务，如：处理交易、出块、刷新等。

在后续迭代版本中，Backup将会是一个自动的模块。
