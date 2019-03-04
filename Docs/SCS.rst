SCS Client
^^^^^^^^^^

Smart Contract Server(SCS) clients are the key components of the MicroChain in MOAC network. 

The most recent client version is v1.0.8, which 
We have some video instructions for developers to setup MicroChains. 

**English tutorials:**

`Youtube <https://www.youtube.com/watch?v=6j3Vl2Un-kQ>`__
`Youku <http://v.youku.com/v_show/id_XMzYyMTQzMTk1Mg==.html?spm=a2h3j.8428770.3416059.1>`__

To connect the SCS client to the MOAC network, user needs to setup the config in the userconfig.json:

    "VnodeServiceCfg": "localhost:50062",
    "DataDir": "./scsdata",
    "LogPath": "./_logs",
    "Beneficiary": "0xD814F2ac2c4cA49b33066582E4e97EBae02F2aB9",
    "VnodechainId": 101,
    "Capability": 10,
    "ReconnectInterval": 5,
    "LogLevel": 4,
    "BondLimit":2,
    "ReWardMin":0.0001
    
The content of the userconfig.json is as follows:

VnodeServiceCfg: the IP of the VNODE the SCS should connect with;


**Other related links**

:doc:`MicroChainFormation`

:doc:`MicroChainSCSMining`

:doc:`MicroChainSCSMonitor`

:doc:`MicroChainVNODEProxy`

:doc:`MicroChainUsers`

:doc:`MicroChainDAPPDeveloper`
