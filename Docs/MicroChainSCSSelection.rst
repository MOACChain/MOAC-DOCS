MicroChain SCS Selection
-------------------------


*MicroChain select SCSs using the following methods:*

1. In the MicroChain contract, DAPP developer need to choose the range
   of SCSs[min,max] needed for the MicroChain. min should be no less than 1. max should be 
2. Vnode compare the distance between the SCS account and the VNODE. If
   its smaller than the selected target, then notify the SCS.
3. SCS received the notice from register, then it need to answer with
   RegisterAsSCS function to confirm to join the MicroChain.

*By using this SCS selection method, we want to:*

1. The selection is random;
2. The selection can adjust with number of SCS in the pool;
3. The selection can be sure the selected SCSs are live in the network.

*Note: The required distance between two address(hash\_dist) is decided
from the value of index\_range RangeIndex[] 。*

.. figure:: https://raw.githubusercontent.com/wiki/moacchain/moac-core/image/scschiose.png
   :alt: 

The selection process is executed by calling registerOpen method with MicroChain contract:

RegisterOpen：

首先介绍一下RegisterOpen的主要工作：
* Dapp用户设置子链开放注册；
* 主网广播通知所有的协议合约中的候选SCS；
* SCS收到广播后，参与子链的注册，按注册先后排序，直到注册数目达到子链的上限。

通过开放子链注册，候选SCS内部完成注册，最终成为子链的一员，才有资格参与子链的相关业务。
需要注意的是：SCS参与子链注册是通过SCS地址（我们也称为scsid，放在“./scskeystore”目录下）发送交易到子链完成；因此需要给SCS地址储备一定量的moac，建议1个moac。

另附1示例代码RegisteOpen:
```javascript
function subchainRegisterOpen(dappAddr,dappPasswd,subchainAddr)
{
    chain3.personal.unlockAccount(dappAddr, dappPasswd,0);
    sendtx(dappAddr, subchainAddr, '0','0x5defc56c' );
}
```
【说明】：
* dappAddr、dappPasswd：Dapp用户用来发送交易前账户解锁；
* subchainAddr：部署子链控制合约subchainbase的地址；
* 数据：'0x5defc56c'是子链控制合约subchainbase中‘registerOpen()’通过hash算法Keccak256得到前4个字节，此处不用修改；

另附1调用实例：
```javascript
subchainRegisterOpen('0x1b9ce7e4f15...38913a56cd986786',
‘123’,
'0x6f5b81365fb1d3d...536cc78749af2c')
```

registerOpen后，系统将自动选择符合条件的SCS节点并通知他们进行注册，DAPP用户需要通过如下方法查看已经注册完成的SCS节点数目（nodeCount）：
方法一：刚部署完子链合约可以在Console窗口直接查询
```javascript
> subchainBase.nodeCount()
```
通过子链合约对象subchainBase调用合约成员nodeCount
方法二：根据合约地址，console命令直接调用
```javascript
> mc.getStorageAt(subchainAddr,0x0e)
```
通过合约地址subchainAddr查询合约中第0x0e个public成员变量（即nodeCount）的值，请不要修改此值
当达到子链运行需要的SCS的数量区间后，即可进入RegisterClose步骤
    
### B3、RegisterClose：
首先介绍一下RegisterClose的主要工作：
* Dapp用户设置子链关闭注册；
* 已经注册SCS数目必须不小于子链要求的最小数目，否则子链注册无效；
* 主网广播通知所有的协议合约中候选SCS，包括已经注册的成功的SCS；
* SCS收到广播后，SCS自身完成初始化并开始子链运行。

关闭子链注册后，候选SCS不能再通过subchainRegisterOpen方式注册该子链，已经注册的SCS处于正常运行状态，参与子链的相关业务，如：处理交易、出块、刷新等。

regiserClose Javascript方法：
```javascript
function subchainRegisterClose(dappAddr,dappPasswd,subchainAddr)
{
    chain3.personal.unlockAccount(dappAddr, dappPasswd,0);
    sendtx(dappAddr, subchainAddr, '0','0x69f3576f' );
}
```
【说明】：
* dappAddr、dappPasswd：Dapp用户用来发送交易前账户解锁；
* subchainAddr：部署子链合约得到的合约地址；
* 数据：'0x69f3576f'是子链控制合约subchainbase中‘registerClose()’通过hash算法Keccak256得到前4个字节；

另附1调用实例：
```javascript
subchainRegisterClose('0x1b9ce7e4f15......e0e38913a56cd986786',
‘123’,
'0x6f5b81365fb1d3......6907fa536cc78749af2c')
```

再次建议在close之前先查看有足够多的节点已经注册进来，以便子链启动时有足够多的矿工可以工作。