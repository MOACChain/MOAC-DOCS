MicroChain VNODE-PROXY
-----------------------

In MOAC system，VNODE-PROXY(VP) is used to provide MicroChain register
and data services. VP is a VNODE that can get rewards from MicroChains
without mining. You just need to set the VnodeBeneficialAddress in the
vnodeconfig.json.

To provide MicroChain register service, VP need to publish its
VnodeBeneficialAddress to the MicroChain users.

To provide data service to MicroChains, VP need to register in the Proxy
contract to be selected:

The function call to register vnode is:

.. code:: javascript

    function vnodeRegister (baseAddr,basePasswd, vnodeAddr,vnodeChain)
    {
        chain3.personal.unlockAccount(baseaddr,basename);
        sendtx(baseaddr, vnodeChain, '1','0x32434a2e000000000000000000000000' + vnodeAddr //data1
        +'0000000000000000000000000000000000000000000000000000000000000040'//data2
        +'0000000000000000000000000000000000000000000000000000000000000013'//data3
        //1 9 2 . 1 6 8 . 1 0 . 1 6 : 5 0 0 6 2
        +'3139322e3136382e31302e31363a353030363200000000000000000000000000');//data4
    }

Commenets:

-  baseAddr、basePasswd：Dapp Developer need to use the password to
   unlock the account;
-  vnodeChain：VNODE-PROXY contract address;
-  vnodeAddr：VnodeBeneficialAddress in vnodeconfig.json;
-  data1：'0x32434a2e000000000000000000000000' is the call to contract
   VNODE-PROXY function ‘register(address vnode, string link)’, '1'
   means 1 moac as deposit;
-  data2：string data type；
-  data3：string data length；
-  data4：string content;

Commands to call register vnode:

.. code:: javascript

    vnodeRegister ('0x1b9ce7e......1e0e38913a56cd986786',
    ‘123’,
    '0x1b9ce7e4f156c1a28f......38913a56cd986786',
    '0x979b01d62ae09dfce5......95541bd32580de66')
