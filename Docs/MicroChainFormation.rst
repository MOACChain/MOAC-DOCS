MicroChain Formation
====================

**Requirements**

To form a MicroChain, user needs to prepare at least three SCS to run the MicroChain and one VNODE to connect to a MOAC netwrk.

MOAC required each SCS has deposit when it registered the MicroChain.

* 1. VNODE connected to MOAC network;
* 2. SCS servers (>=3) with some MOAC deposit;

:doc:`MicroChainVNODEProxy`

There are three types of smart contracts needed for the MicroChain formation

* 1. Smart contract for the VNODE protocol;
* 2. Smart contract for the MicroChain protocol;
* 3. Smart contract for the MicroChain;

.. figure:: https://raw.githubusercontent.com/wiki/moacchain/moac-core/image/reg-flow.png
   :alt: 


*MicroChain select SCSs using the following methods:*

1. In the MicroChain contract, DAPP developer need to choose the range
   of SCSs[min,max] needed for the MicroChain.
2. Vnode compare the distance between the SCS account and the VNODE. If
   its smaller than the selected target, then notify the SCS.
3. SCS received the notice from register, then it need to answer with
   RegisterAsSCS function to confirm to join the MicroChain.

*By using this SCS selection method, we want to make sure:*

1. The selection is random;
2. The selection can adjust with number of SCS in the pool;
3. The selection can be sure the selected SCSs are live in the network.

*Note: The required distance between two address(hash\_dist) is decided
from the value of index\_range RangeIndex[] 。*

.. figure:: https://raw.githubusercontent.com/wiki/moacchain/moac-core/image/scschiose.png
   :alt: 



MOAC MicroChain Formation
-------------------------


The published MOAC release later than nuwa 1.0.8 supporting multiple contracts on the MicroChain.
To form the MicroChain and running, there are 4 major steps:

1. Have at least one VNODE registered in a VNODE POOL contract:

   ::

      A VNODE pool contract to allow VNODE join as proxy of the microchain;
      :doc:`MicroChainVNODEProxy`


2. Have a SCS POOL contract with some SCS servers registered:

   ::

       >personal.newAccount()

   from console prompt, start mining by running

   ::

       >miner.start(1)

3. After vnode started, start scs servers. You need at least 2 scs
   servers to form the microchain.

   from another terminal, change into scs1/bin run the executable

   ::

       $./scs1

   do the same for the other two, if connected with vnode, some info
   should be seen as the following:

   ::

       ......
       INFO [05-23|01:21:56.111] 230:liveinfo: {1285} 
       INFO [05-23|01:22:00.111] 221:liveinfo: {1286} 
       INFO [05-23|01:22:05.112] 205:liveinfo: {1289} 
       INFO [05-23|01:22:08.111] 206:liveinfo: {1290} 
       INFO [05-23|01:22:09.120] 222:liveinfo: {1291} 

   The number in {} is the vnode block number. All 3 scs should have the
   same info.

4. scs servers need some initial funds to work. Currently there are
   fixed address for the test scs servers:

   ::

       scs1="0x027aee503f2e27fee5103df1796daeb6111e204d"
       scs2="0xba3597b4c20f0477c4ebe7cf81c565e556bd52f9"
       scs3="0x1675124b338671dd771a673c7d06606ae1cdd7b0"

   Otherwise you need to send some funds to the accounts. From prompt,
   load script

   ::

       >loadScript("mctest.js")
       >Send(mc.accounts[0], '', scs1, 0.1)
       >Send(mc.accounts[0], '', scs2, 0.1)
       >Send(mc.accounts[0], '', scs3, 0.1)

5. Now start deploy the microchain from vnode:

   ::

       deploy_p1.js: protocol needed for microchain.
       deploy_s1.js: microchain contract.
       test_s1.js: test scripts for microchain.


       > personal.unlockAccount(mc.accounts[0])
       true
       > loadScript("deploy_p1.js")
       null [object Object]

   Testnet deploy:

   ::

       Contract mined! address: 0xeaf8f52588af16aebb26c4ca8fb10d5e0b8b1d70 transactionHash: 0xf029d6168c289fac8322bb1925205c905f31a4cc45dc826f4e3b7200ceda3de5

6. Load the subchain script file:

   ::

       > loadScript("deploy_s1.js")
       null [object Object]
       true
       Contract mined! address: 0x78934339dcb0642bdfd2afb3e028ee40be809280 transactionHash: 0x503fb3377866071a80e4f023c2cab4999876b93fbaa86dec54d73b1d4d8391a7

7. Load the test script.

   ::

       > loadScript("test_s1.js")
               true

8. Register scs servers in the pool:

   ::

       > registertopool(scs1)
       sending from:0xa8863fc8ce3816411378685223c03daae9770ebb to:0x08b95aebd9c3cfbea68631486cc76d7281c15a79 amount:12 with data:0x4420e486000000000000000000000000a4e1e48c7b2b0bd7b2f202e0db0270a9678df266
       undefined
       > registertopool(scs2)
       sending from:0xa8863fc8ce3816411378685223c03daae9770ebb to:0x2287b6c3643aa1d96ca5eb198f660c512bef28d1 amount:12 with data:0x4420e486000000000000000000000000f1f5b7a35dff6400af7ab3ea54e4e637059ef909
       undefined

   You can see if the node is registered:

   ::

       > subchainprotocolbase.scsCount()
           2

   For safety issues, scs servers need to wait for some blocks after
   register in the pool. You can find the info about a scs server by:

   ::

       > subchainprotocolbase.scsList(scs1)
       ["0xecd1e094ee13d0b47b72f5c940c17bd0c7630326", 12000000000000000000, 942, 1.15792089237316195423570985008687907853269984665640564039457584007913129639935e+77]

   The number '942' is the block number this scs server can join
   microchain, usually this is 50 blocks later after the scs registered
   in the pool.

9. Open subchain for scs to join:

When there are more than 2 scs in the pool, you can start open the
microchain for scs server to join You may need to unlock the
mc.accounts[0] for this step.

::

        > personal.unlockAccount(mc.accounts[0])
        true
        > registeropen()
        miner.starsending from:0xa8863fc8ce3816411378685223c03daae9770ebb to:0x26c27eb5585d1e978d4da14f0eb2ee479d733a46 amount:0 with data:0x5defc56c
        undefined

There will be some confirmation tx send from vnode to scs servers. You
can also check to see how many scs servers are selected in the
microchain:

::

        > subchainbase.nodeCount()
        2
        
    When you have enough scs servers, you can close registration and start the microchain:

        > registerclose()
        sending from:0xa8863fc8ce3816411378685223c03daae9770ebb to:0xc3c6e85820d97477172498ce7aed37b0bb22e67e amount:0 with data:0x69f3576f
        

10. Subchain mining started: After these steps, scs server should have
    some info like:

    ::

        INFO [05-23|01:23:01.754] 278:Commit new mining work   number=2 txs=0 elapsed=490.027µs
        INFO [05-23|01:23:01.755] 278:🔨 mined potential block    number=2 hash=0xa67b93923ba3ea9ff43f076c5839aa5ea31291c7cf71db1c80c18af2c1b9be1b
        INFO [05-23|01:23:11.781] 280:
        ###### BLOCK Number: 3 ######
        block.Hash:       0xe0a8c81c8f3898721ea99cd569f0fc545af03ffb18679171cb2eaf79ec6ee672
        block.ParentHash: 0xa67b93923ba3ea9ff43f076c5839aa5ea31291c7cf71db1c80c18af2c1b9be1b
        SubchainAddr:     0xa107434b94c8c2690dbcff298434b91d22f767db
        ##############################

        INFO [05-23|01:23:14.112] 208:liveinfo: {1321} 
