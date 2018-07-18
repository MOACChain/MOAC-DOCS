子链SCS节点的选择
=================

*子链的SCS的节点选择通过三个步骤实现：* \*
1.子链设定一个需要选择的SCS节点数范围[min,max]。然后调用子链协议合约的getSelectionTarget()，根据当前的注册的SCS总数，得到一个selection
target。 \*
2.V-node比较子链地址和与自己相连的SCS地址的距离，如果小于selection
target，则通知SCS。 \*
3.SCS得到register的通知，必须主动调用子链的RegisterAsSCS来确定自己参与到该子链。

*通过这样的选择方法，可以实现：* \* 1.选择的过程是随机的 \*
2.SCS的选择根据当前的SCS节点总数自动调整 \*
3.SCS的显示确认保证SCS的liveness

*注：两个地址的距离(hash\_dist)由RangeIndex[]定义的位数(index\_range)来确定。*
|image0|

.. |image0| image:: https://raw.githubusercontent.com/wiki/moacchain/moac-core/image/scschiose.png

