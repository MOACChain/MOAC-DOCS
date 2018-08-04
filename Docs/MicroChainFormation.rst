MicroChain Formation
============

**【前提条件】**

*子链协议合约已经创建*

**【参数】** \* 1.所采用的协议 \*
2.子链的SCS个数[min,max]，选择总节点数的千分比 \* 3.子链刷新周期 \*
4.子链逻辑代码Funccode

**【流程】** \*
1.DAPP部署者在v-node端部署一个全局的子链合约，设置Funccode \*
2.DAPP部署者调用RegisterOpen，允许SCS进来注册。同时调用V-node代码，如果检测到相连的scs符合要求，向scs推送一个enroll
的message \*
3.SCS节点发起一个对子链合约的RegisterAsSCS的调用，来确认参与这个子链 \*
4.DAPP部署者调用RegisterClose,关闭注册，
V-node在执行时，判断条件是否满足。如果满足，v-node向scs推送一个newSubchain
msg。SCS端触发一系列的初始化操作，完成subchain的设置。如果条件不满足，DAPP部署者重新发起这个过程。

.. figure:: https://raw.githubusercontent.com/wiki/moacchain/moac-core/image/reg-flow.png
   :alt: 

