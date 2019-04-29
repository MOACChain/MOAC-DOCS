
MOAC token
********************************************************************************

What is MOAC token?
================================================================================

MOAC(mc) is the name of the native token used within MOAC network, just like ether within Ethereum. It is used to pay for
computation within the EVM and run MicroChains. This is done indirectly by purchasing gas for mc as explained in :ref:`gas-and-mc`.

Denominations
--------------------------------------------------------

MOAC has a metric system of denominations used as units of mc. Each denomination has its own unique name. The smallest denomination aka *base unit* of mc is called Sha. Below is a list of the named denominations and
their value in Sha. Following a common pattern, mc also designates a unit (of 1e+18 or one quintillion Sha) of the currency. 


+-------------------------+-----------+-------------------------------------------+
| Unit                    | Sha Value | Sha                                       |
+=========================+===========+===========================================+
| **sha**                 | 1 sha     | 1                                         |
+-------------------------+-----------+-------------------------------------------+
| **Ksha (femtomc)**      | 1e3 sha   | 1,000                                     |
+-------------------------+-----------+-------------------------------------------+
| **Msha (picomc)**       | 1e6 sha   | 1,000,000                                 |
+-------------------------+-----------+-------------------------------------------+
| **Gsha (xiao)**         | 1e9 sha   | 1,000,000,000                             |
+-------------------------+-----------+-------------------------------------------+
| **micromc (sand)**      | 1e12 sha  | 1,000,000,000,000                         |
+-------------------------+-----------+-------------------------------------------+
| **millimc**             | 1e15 sha  | 1,000,000,000,000,000                     |
+-------------------------+-----------+-------------------------------------------+
| **mc (moac)**           | 1e18 sha  | 1,000,000,000,000,000,000                 |
+-------------------------+-----------+-------------------------------------------+


MOAC supply
=========================

The initial supply of MOAC coin is 150 million. The MOAC foundation holds about 53 million coins and the rest is in circulation. The value of MOAC coin ﬂuctuates daily, and can be found at CoinMarketcap.com (https://coinmarketcap.com/currencies/moac).
The MOAC coins first issued as ERC20 token on Ethereum in July 2017. After MOAC mainnet was successfully launched in April 2018, all the MOAC coins on the Ethereum were exchanged to MOAC native token with a 1:1 ratio. The ERC20 contract of MOAC was closed in 2018 and there is no ERC20 token of MOAC on Ethereum platform anymore. 

Six million additional coins per year will be generated during mining, further increasing the supply in circulation. After 4 years, the production will halve to 3 million, then will halve again the following 4 years. The total supply will be 210 million coins by 2058.

Current supply can be found at:
* http://explorer.moac.io/home


Getting mc
================================================================================

Please see this page `here <https://coinmarketcap.com/currencies/moac/#markets>`_.

Sending mc
================================================================================

The `MOAC Wallet  <https://www.moacwalletonline.com/>`_  supports sending mc via a graphical interface.

MOAC can also be transferred using the **MOAC console**.

.. code-block:: console

    > var sender = mc.accounts[0];
    > var des = mc.accounts[1];
    > var amount = chain3.toSha(0.01, "mc")
    > mc.sendTransaction({from:sender, to:des, value: amount})


MOAC network, like Ethereum, use mc as a cryptofuel, commonly referred to as "gas". Beyond transaction fees, gas is a central part of every network request and requires the sender to pay for the computing resources consumed. The gas cost is dynamically calculated, based on the volume and complexity of the request and multiplied by the current gas price. Its value as a cryptofuel has the effect of increasing the stability and long-term  demand for mc and MOAC as a whole. 

.. _gas-and-mc:

Gas and mc
=============================


Gas is supposed to be the constant cost of network resources/utilisation. You want the real cost of sending a transaction to always be the same, so you can't really expect Gas to be issued, currencies in general are volatile.

So instead, we issue mc whose value is supposed to vary, but also implement a Gas Price in terms of MOAC. If the price of mc goes up, the Gas Price in terms of mc should go down to keep the real cost of Gas the same.

Gas has multiple associated terms with it: Gas Prices, Gas Cost, Gas Limit, and Gas Fees. The principle behind Gas is to have a stable value for how much a transaction or computation costs on the Ethereum network.

* Gas Cost is a static value for how much a computation costs in terms of Gas, and the intent is that the real value of the Gas never changes, so this cost should always stay stable over time.
* Gas Price is how much Gas costs in terms of another currency or token like MOAC. To stabilise the value of gas, the Gas Price is a floating value such that if the cost of tokens or currency fluctuates, the Gas Price changes to keep the same real value. The Gas Price is set by the equilibrium price of how much users are willing to spend, and how much processing nodes are willing to accept.
* Gas Limit is the maximum amount of Gas that can be used per block, it is considered the maximum computational load, transaction volume, or block size of a block, and miners can slowly change this value over time.
* Gas Fee is effectively the amount of Gas needed to be paid to run a particular transaction or program (called a contract). The Gas Fees of a block can be used to imply the computational load, transaction volume, or size of a block. The gas fees are paid to the miners (or bonded contractors in PoS).
