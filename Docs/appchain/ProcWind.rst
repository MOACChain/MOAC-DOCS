.. _proc-wind:

ProcWind
--------------

ProcWind consensus
====================

ProcWind是采用PoS共识，支持多合约部署的应用链节点。
目前ProcWind也支持两种原子跨链交换，可以完成母链原生通证或者ERC20通证和应用链原生通证之间的互换。

采取股权证明共识的ProcWind，依赖网络中的验证节点来检验交易，而不像严格的工作量证明(PoW)那样需要处理大量数据。在股权证明共识中，下一个区块的创建者是根据诸如持币量或币龄等因素（即股份）的随机算法来选择的。

股权证明系统的优点在于它可以完全扩展到企业量级的交易、能效高并支持多种交易。随着网络中的节点数量增加，其验证能力也会同步提升，在无需不断地访问母链的情况下，允许DApp应用链开展小额交易。

应用链的验证过程由应用链客户端（SCS）完成，SCS节点随机组合，支持动态增减。

应用链支持分片，每个分片都能独立完成业务逻辑。

应用链本身是以智能合约的方式部署到MOAC平台上，其共识方式、节点组成和业务逻辑都在应用链合约中定义。

* AppChainProtocolBas contract: defined the consensus protocols in the AppChain. The SCS needs to agree the protocol to join the SCS pool and then be selected as part of the AppChain;
* AppChainBase：用于应用链控制逻辑，应用链生成前和生成后的一系列控制逻辑;
* DappBase）：用于控制合约在应用链上的部署，一条应用链可以部署多个合约；目前有两类控制合约，一类是仅允许应用链的部署帐号，即拥有者（owner）在应用链上部署合约，而另一类则没有这个限制;
* 应用链DAPP智能合约：用于部署应用链业务逻辑的合约，每个应用链可以部署多个DAPP合约;


ProcWind SCS
================

SCS is the client software to form ProceWind AppChain. Each ProcWind AppChain needs to have at least 1 SCS to run. 客户端，原称智能合约服务器 Smart Contract Server(SCS) 是支持应用链运行的节点软件。
SCS通过VNODE代理节点接入MOAC母链，每个运行的SCS可以支持多条应用链，也可以动态接入不同VNODE。
目前有两种应用链客户端，分别支持对应的应用链

当前，按在应用链中的功能分，有如下几种SCS节点类型：
* 参与业务逻辑的SCS
* 用于业务监控的SCS
* 准备参与业务逻辑的SCS

节点的操作可以参考：

:doc:`Setup`

ProcWind token
====================

ProcWind can have its own token. The total amount of token needs to be set in the AppChain contract before deployed on the BaseChain. 
More details can be seen in:
:ref:`Setup ProcWind AppChain <proc-wind-setup>` 


ProcWind cross chain
====================

应用链通证可以和母链的原生货币或者ERC20代币直接进行兑换，
具体做法可以参考：

:doc:`ProcWindExchange`

ProcWind setup
====================

To setup ProcWind AppChain, user needs to have certain resources, and deploy the ProcWind AppChain contract. 

There are two type of ProcWind AppChain:ASM and AST.
The content of the contracts can be downloaded from 
`MOAC ProcWind contracts <https://github.com/MOACChain/moac-core/tree/master/procwind>`__

In the ASM contract, the constructor is as the following:
:: 
    function ChainBaseASM(
    address proto, 
    address vnodeProtocolBaseAddr, 
    uint min, 
    uint max, 
    uint thousandth, 
    uint flushRound, 
    uint256 tokensupply, 
    uint256 exchangerate)

* address proto - SCS pool base contract address on the BaseChain;
* address vnodeProtocolBaseAddr - Vnode pool contract address;
* uint min - Min number of SCS required by the ProcWind AppChain, need to choose one of [1，3，5，7];
* uint max - Max number of SCS required by the ProcWind AppChain, need to choose one of [11，21，31，51，99;
* uint thousandth - Ratio to choose the SCS，suggested value = 1;
* uint flushRound - Number of blocks on BaseChain between two flush action, range should be 40-99;  
* uint256 tokensupply - AppChain token amount；
* uint256 exchangerate - Exchange ratio between moac and AppChain token;

注意，这里输入参数tokensupply和应用链的BALANCE相对映，
BALANCE = tokensupply * 1e18
例如，tokensupply = 1000，结果的BALANCE应该是10的21次方。

AST的合约构建函数为：
:: 
    function ChainBaseAST(
    address proto, 
    address vnodeProtocolBaseAddr, 
    address ercAddr,  
    uint ercRate,
    uint min, 
    uint max, 
    uint thousandth, 
    uint flushRound)

其中的参数含义为：

* address proto - SCS节点池地址；
* address vnodeProtocolBaseAddr - Vnode节点池合约地址；
* address ercAddr - 基础链ERC20合约地址；
* uint ercRate - 应用链原生货币和基础链ERC20 token的兑换比例；
* uint min - 应用链需要SCS的最小数量，需要从如下值中选择：1，3，5，7；
* uint max - 应用链需要SCS的最大数量，需要从如下值中选择：11，21，31，51，99
* uint thousandth - 控制选择scs的概率，建议设为1，对于大型应用链节点池才有效；
* uint flushRound - 应用链刷新周期  单位是主链block生成对应数量的时间，当前的取值范围是40-99；
* uint256 tokensupply - 应用链的原生货币数量；
* uint256 exchangerate - 应用链原生货币和母链moac的兑换比例；

注意，AST应用链的BALANCE是由ERC20 token里面totalSupply相对映，
BALANCE = tokenSupply * ERCRate * (10 ** (ERCDecimals));

用户可以根据需要调试输入参数，之后的应用链部署步骤请参考：

More details can be seen in:

:doc:`ProcWindSetup`

ProcWind Dapp development
==========================

DAPP can be developed on the ProcWind AppChain，
More details can be seen in:

:ref:`ProcWind Dapp development <proc-wind-dapps>` 


