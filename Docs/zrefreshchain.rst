子链刷新过程
============

**【参数】**

*子链刷新的参数在subchainbase.sol中定义。参数列表如下：*

-  1.刷新周期Round数值：定义子链经过多少区块后刷新。假如子链有100个节点，每个节点依次产生block，定义Round数为5，则每过500block
   刷新一次。
-  2.当前刷新id 索引：指定下次刷新的id 在Nodelist中 的索引值
-  3.刷新过期数值expiration：指定的id在block [0,
   2\*expiration]必须完成刷新，否则由下一个id重新发起刷新。
-  4.刷新作假惩罚：如果节点在propose，dispute，或者vote阶段作假，将从预先缴纳的bond中扣除相应的惩罚数额（MOAC），并被踢出NodeList

**【流程】** \* 1.Proposal的格式如下： |image0|

-  2.刷新周期到的时候，SCS节点调用子链合约的createProposal，发起一个刷新请求交易flushTX
-  3.V-node接受到flushTX后，处理相应的逻辑，并将推送消息到相应的scs
   node，告知有新的proposal。
-  4.如果SCS node发现没有问题，它们不需要反应。
-  5.如果SCS node发现这个proposal
   有问题，它可以发起一个新的proposal，并通过TX调用DisbuteProposal，之后触发v-node将推送消息到相应的scs
   node。一旦SCS节点收到消息，那么所有的SCS必须响应，一个SCS只能对其中的一个进行投票（由智能合约保证）。
-  6.最初发起proposal和发起disbute的SCS节点设置timer，在指导的时间内，获得投票的结果。如果得票超过50%，那么这个节点就发起一个TX来调用子链合约的Approval
   function。
-  7.合法的proposal被接受，并被记录到区块链中，错误的proposal和所有对错误的proposal进行投票的人将被扣掉相应的保证金。

.. figure:: https://raw.githubusercontent.com/wiki/moacchain/moac-core/image/flush-flow.png
   :alt: 

*子链刷新过程与SubchainBase合约的调用* |image1|

.. |image0| image:: https://raw.githubusercontent.com/wiki/moacchain/moac-core/image/flush-proposal.png
.. |image1| image:: https://raw.githubusercontent.com/wiki/moacchain/moac-core/image/subchain.png

