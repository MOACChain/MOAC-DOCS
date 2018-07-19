代理Vnode节点（VNODE-PROXY）
----------------------------

在MOAC系统中，代理Vnode节点被用于提供子链调用服务和子链历史数据中转服务的节点。启动代理节点的方法和启动母链矿工节点的方法类似。但是，需要额外在vnodeconfig.json的VnodeBeneficialAddress里设置收益账号。

当提供子链调用合约服务时，需要调用子链时的via字段，设置和VnodeBeneficialAddress相同，否则该节点将不会工作。

在提供子链历史数据服务前，需要先向代理合约注册此节点以便被随机选中进行中转，方法如下：

另附1添加代理Vnode节点的Javascript方法：

.. code:: javascript

    function vnodeRegister (baseAddr,basePasswd, vnodeAddr,vnodeChain)
    {
        chain3.personal.unlockAccount(baseAddr,basePasswd);
        
        sendtx(baseaddr, vnodeChain, '1','0x32434a2e000000000000000000000000' + vnodeAddr //数据1
        +'0000000000000000000000000000000000000000000000000000000000000040'//数据2
        +'0000000000000000000000000000000000000000000000000000000000000013'//数据3
        //1 9 2 . 1 6 8 . 1 0 . 1 6 : 5 0 0 6 2
        +'3139322e3136382e31302e31363a353030363200000000000000000000000000');//数据4
    }

【说明】： \* baseAddr、basePasswd：Dapp用户用来发送交易前账户解锁； \*
vnodeChain：部署VNODE-PROXY合约得到的合约地址； \*
vnodeAddr：vnodeconfig.json的VnodeBeneficialAddress \*
数据1：'0x32434a2e '是VNODE-PROXY合约 中‘register(address vnode, string
link)’通过hash算法Keccak256得到前4个字节，本例押金交1moac；本例带两个参数；
\* 数据2：string数据类型； \* 数据3：string数据长度； \*
数据4：string内容

另附2调用实例：

.. code:: javascript

    vnodeRegister ('0x1b9ce7e......1e0e38913a56cd986786',
    ‘123’,
    '0x1b9ce7e4f156c1a28f......38913a56cd986786',
    '0x979b01d62ae09dfce5......95541bd32580de66')
