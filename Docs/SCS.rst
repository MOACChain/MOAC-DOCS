SCS Client
^^^^^^^^^^

Smart Contract Server(SCS) clients are the key components of the MicroChain in MOAC network. 



We have some video instructions for developers to setup MicroChains. 

**English tutorials:**

`Youtube <https://www.youtube.com/watch?v=6j3Vl2Un-kQ>`__
`Youku <http://v.youku.com/v_show/id_XMzYyMTQzMTk1Mg==.html?spm=a2h3j.8428770.3416059.1>`__

`SubChainProtocolBase <https://github.com/MOACChain/moac-core/wiki/部署子链协议合约>`__

To initiate ./bin/moac --datadir "/tmp/moac/60/01" --networkid 1975
--nodiscover --verbosity 4

Operatig a Vnode control window ./bin/moac attach /tmp/moac/60/01/moac.ipc

operate SCS1

./scs1/scsserver
./go/src/github.com/innowells/moac-scs/build/bin/scsserver

Start SCS2

::

    .scs2/scsserver
    ./go/src/github.com/innowells/moac-scs/build/bin/scsserver

=============== Test commands in the console ===============
personal.newAccount("123456") miner.start()
personal.unlockAccount(mc.accounts[0], "123456", 0)
scs1="0xA6D018ae6981552Df5C39b5911bF7CA793D0405c"
scs2="0x700A559b7C5c3D5f79df9f7EfdD0bd068CBFad4e"
mc.getBalance(scs1)/1000000000000000000
mc.getBalance(scs2)/1000000000000000000
mc.getBalance(mc.accounts[0])/1000000000000000000

::

    loadScript("deploy_p1.js")
    loadScript("deploy_s1.js")
    loadScript("test_s1.js")

    sendtx(mc.accounts[0], scs1, 200)
    sendtx(mc.accounts[0], scs2, 200)
    mc.getBalance(scs1)/1000000000000000000
    mc.getBalance(scs2)/1000000000000000000

    registertopool(scs1)
    registertopool(scs2)
    subchainprotocolbase.scsCount()
    subchainprotocolbase.scsList(scs1)
    subchainprotocolbase.scsList(scs2)

    miner.stop()
    miner.start(1)
    registeropen()
    subchainbase.nodeCount()
    registerclose()

**Other related links**

:doc:`MicroChainFormation`

:doc:`MicroChainSCSMining`

:doc:`MicroChainSCSMonitor`

:doc:`MicroChainVNODEProxy`

:doc:`MicroChainUsers`

:doc:`MicroChainDAPPDeveloper`
