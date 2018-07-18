`SCS English Version <https://github.com/MOACChain/moac-core/wiki/SCS>`__
-------------------------------------------------------------------------

子链是MOAC区块链中非常重要的一个模块。其主要目的在于分流母链中的业务逻辑，把一些比较繁琐的业务操作放在子链中执行。

子链由SCS节点组成，必须部署在母链Vnode上。

子链支持分片，每个分片都能独立完成业务逻辑。

子链中的节点随机组合，支持动态增减。

同时，在主链上，我们增加了代理的Vnode节点来保证子链的稳定性，这部分我们会在最后介绍。

当前，按功能分，有如下几种SCS节点类型： \* 参与业务逻辑的SCS \*
用于业务监控的SCS \* 准备参与业务逻辑的SCS

下图为MOAC系统的示意图，子链在subchain1方框内 |moac\_subchian|

主要名词解释：
~~~~~~~~~~~~~~

**DAPP用户：** 使用子链DAPP的用户

**DAPP项目方：**
子链DAPP的开发者。在本文档中，DAPP用户和项目方统称DAPP用户（开发者）

**主网挖矿节点：** 当前母链上运行的挖矿节点（MOAC-VNODE）

**主网代理节点：** 母链代理节点，用于提高子链节点的稳定性（VNODE-PROXY）

**子链挖矿节点：** 子链矿工节点，参与子链业务逻辑（SCS）

**子链监控节点：** 只用于查询子链业务状态，不参与共识（MONITOR）

**智能合约：** 当前有这样几种智能合约，本文档只解释与子链相关的智能合约

::

    系统智能合约
        * 主链系统合约
        * 子链矿池智能合约subchainprotocolbase，用于子链SCS节点矿工加入矿工池
        * 代理智能合约vnodeprotocolbase：用于母链vnode提供代理功能，加入代理池
        * 子链控制合约subchainbase：用于子链控制逻辑，子链生成前和生成后的一系列控制逻辑
    子链DAPP智能合约：用于部署子链业务逻辑的合约

DAPP用户部署子链时，只需要关注子链控制合约和子链DAPP智能合约。

子链的基本设计原理
~~~~~~~~~~~~~~~~~~

`部署子链协议合约 <https://github.com/MOACChain/moac-core/wiki/部署子链协议合约>`__

`部署子链合约 <https://github.com/MOACChain/moac-core/wiki/部署子链合约>`__

`选择scs节点 <https://github.com/MOACChain/moac-core/wiki/选择scs节点>`__

`子链创建过程 <https://github.com/MOACChain/moac-core/wiki/子链创建过程>`__

`子链运行流程 <https://github.com/MOACChain/moac-core/wiki/子链运行流程>`__

`子链刷新过程 <https://github.com/MOACChain/moac-core/wiki/子链刷新过程>`__

子链的部署和运行
~~~~~~~~~~~~~~~~

`子链用户类别 <https://github.com/MOACChain/moac-core/wiki/子链用户类别>`__

`子链SCS矿工指南 <https://github.com/MOACChain/moac-core/wiki/SCS矿工指南>`__

`子链DAPP开发者指南 <https://github.com/MOACChain/moac-core/wiki/DAPP开发者指南>`__

`子链VNODE代理指南 <https://github.com/MOACChain/moac-core/wiki/子链VNODE代理指南>`__

.. |moac\_subchian| image:: image/moac_subchain1.png

