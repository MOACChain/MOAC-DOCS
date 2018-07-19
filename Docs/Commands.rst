Commands
========

常用命令：
----------

::

    --testnet: connect to MOAC testnet (networkid = 101)；

    --rpc: 启用HTTP的RPC服务，以便非本机访问该MOAC节点服务；

    --rpcaddr value: 默认是"localhost", 只能本机访问； 可通过设置 为"0.0.0.0", 以便非本机访问该MOAC节点服务, 但现在RPC服务是基于HTTP的，是明文传输，需注意安全问题；

    --rpcport value: 默认是"8545",  一般来说不用改，用默认端口即可；

    --rpcapi value: 指定RPC要开放的API服务，默认为"chain3,mc,net"，常用的一般还会配置比如personal,admin,debug,miner,txpool,db,shh等，但是因为RPC服务是明文传输，所以，如果使用personal的时候，要注意安全问题；

    --rpccorsdomain value: 一般来说，如果你知道要用这个选项的时候，使用""值即可，更详细可自行搜索“CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）”中相关关键词进一步了解；

    --jspath loadScript: 默认值为".", loadScript装载javascript文件的主目录；

示例如下：
~~~~~~~~~~

启动MOAC节点并连接到mainnet(networkid = 99)

::

    ./moac      

通过本地ipc接口连接到MOAC mainnet节点，并启动命令行

::

    ./moac attach

启动MOAC节点并连接到testnet(networkid = 101)

::

    ./moac --testnet                      

启动MOAC测试节点，并启动交互命令行

::

    ./moac --testnet console    

通过本地ipc接口连接到MOAC节点

::

    ./moac attach /xx/xxx/moac.ipc

通过基于HTTP的RPC接口连接到本地或者远程MOAC节点

::

    ./moac attach http://xxx.xxx.xxx.xxx:8545       

启动MOAC测试节点，同时启动RPC服务

::

    ./moac --testnet --rpc                  

启动MOAC测试节点，非本机可访问，非本机也可以使用personal及debug服务，同时提供跨域资源共享服务

::

    ./moac --testnet --rpc --rpcaddr=0.0.0.0 --rpcapi="db,mc,net,chain3,personal,debug" --rpccorsdomain=""                  

全部命令行参数
--------------

在命令行下，输入：

::

    $ ./moac help

或者

::

    $ ./moac --help

NAME:
^^^^^

::

    moac - the MOAC-core command line interface

    Copyright 2017 The MOAC Authors

USAGE:

::

    moac [options] command [command options] [arguments...]

VERSION:

0.8.3-release-f84b7d91

COMMANDS:
^^^^^^^^^

::

        account     Manage accounts
        attach      Start an interactive JavaScript environment (connect to node)
        bug         opens a window to report a bug on the moac repo
        console     Start an interactive JavaScript environment
        dump        Dump a specific block from storage
        dumpconfig  Show configuration values
        export      Export blockchain into file
        import      Import a blockchain file
        init        Bootstrap and initialize a new genesis block
        js          Execute the specified JavaScript files
        license     Display license information
        makecache   Generate ethash verification cache (for testing)
        makedag     Generate ethash mining DAG (for testing)
        monitor     Monitor and visualize node metrics
        removedb    Remove blockchain and state databases
        version     Print version numbers
        wallet      Manage MoacNode presale wallets
        help, h     Shows a list of commands or help for one command

MOAC CORE OPTIONS:
^^^^^^^^^^^^^^^^^^

::

       --config value                            TOML configuration file
       --datadir "/Users/zpli/Library/MoacNode"  Data directory for the databases and keystore
       --keystore                                Directory for the keystore (default = inside the datadir)
       --nousb                                   Disables monitoring for and managing USB hardware wallets
       --networkid value                         Network identifier (integer, 99=mainnet, 101=testnet, 100=devnet) (default: 99)
       --testnet                                 MOAC test network: pre-configured proof-of-work test network
       --dev                                     Developer mode: pre-configured private network with several debugging flags
       --syncmode "fast"                         Blockchain sync mode ("fast", "full", or "light")
       --mcstats value                           Reporting URL of a mcstats service (nodename:secret@host:port)
       --identity value                          Custom node name
       --lightserv value                         Maximum percentage of time allowed for serving LES requests (0-90) (default: 0)
       --lightpeers value                        Maximum number of LES client peers (default: 20)
       --lightkdf                                Reduce key-derivation RAM & CPU usage at some expense of KDF strength

ETHASH OPTIONS:
^^^^^^^^^^^^^^^

::

       --ethash.cachedir                      Directory to store the ethash verification caches (default = inside the datadir)
       --ethash.cachesinmem value             Number of recent ethash caches to keep in memory (16MB each) (default: 2)
       --ethash.cachesondisk value            Number of recent ethash caches to keep on disk (16MB each) (default: 3)
       --ethash.dagdir "/Users/zpli/.ethash"  Directory to store the ethash mining DAGs (default = inside home folder)
       --ethash.dagsinmem value               Number of recent ethash mining DAGs to keep in memory (1+GB each) (default: 1)
       --ethash.dagsondisk value              Number of recent ethash mining DAGs to keep on disk (1+GB each) (default: 2)

TRANSACTION POOL OPTIONS:
^^^^^^^^^^^^^^^^^^^^^^^^^

::

       --txpool.nolocals            Disables price exemptions for locally submitted transactions
       --txpool.journal value       Disk journal for local transaction to survive node restarts (default: "transactions.rlp")
       --txpool.rejournal value     Time interval to regenerate the local transaction journal (default: 1h0m0s)
       --txpool.pricelimit value    Minimum gas price limit to enforce for acceptance into the pool (default: 1)
       --txpool.pricebump value     Price bump percentage to replace an already existing transaction (default: 10)
       --txpool.accountslots value  Minimum number of executable transaction slots guaranteed per account (default: 16)
       --txpool.globalslots value   Maximum number of executable transaction slots for all accounts (default: 40960)
       --txpool.accountqueue value  Maximum number of non-executable transaction slots permitted per account (default: 64)
       --txpool.globalqueue value   Maximum number of non-executable transaction slots for all accounts (default: 1024)
       --txpool.lifetime value      Maximum amount of time non-executable transaction are queued (default: 3h0m0s)

PERFORMANCE TUNING OPTIONS:
^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

       --cache value            Megabytes of memory allocated to internal caching (min 16MB / database forced) (default: 128)
       --trie-cache-gens value  Number of trie node generations to keep in memory (default: 120)

ACCOUNT OPTIONS:
^^^^^^^^^^^^^^^^

::

       --unlock value    Comma separated list of accounts to unlock
       --password value  Password file to use for non-inteactive password input

API AND CONSOLE OPTIONS:
^^^^^^^^^^^^^^^^^^^^^^^^

::

       --rpc                  Enable the HTTP-RPC server
       --rpcaddr value        HTTP-RPC server listening interface (default: "localhost")
       --rpcport value        HTTP-RPC server listening port (default: 8545)
       --rpcapi value         API's offered over the HTTP-RPC interface
       --ws                   Enable the WS-RPC server
       --wsaddr value         WS-RPC server listening interface (default: "localhost")
       --wsport value         WS-RPC server listening port (default: 8546)
       --wsapi value          API's offered over the WS-RPC interface
       --wsorigins value      Origins from which to accept websockets requests
       --ipcdisable           Disable the IPC-RPC server
       --ipcpath              Filename for IPC socket/pipe within the datadir (explicit paths escape it)
       --rpccorsdomain value  Comma separated list of domains from which to accept cross origin requests (browser enforced)
       --jspath loadScript    JavaScript root path for loadScript (default: ".")
       --exec value           Execute JavaScript statement
       --preload value        Comma separated list of JavaScript files to preload into the console

NETWORKING OPTIONS:
^^^^^^^^^^^^^^^^^^^

::

       --bootnodes value     Comma separated enode URLs for P2P discovery bootstrap (set v4+v5 instead for light servers)
       --bootnodesv4 value   Comma separated enode URLs for P2P v4 discovery bootstrap (light server, full nodes)
       --bootnodesv5 value   Comma separated enode URLs for P2P v5 discovery bootstrap (light server, light nodes)
       --port value          Network listening port (default: 30333)
       --maxpeers value      Maximum number of network peers (network disabled if set to 0) (default: 25)
       --maxpendpeers value  Maximum number of pending connection attempts (defaults used if set to 0) (default: 0)
       --nat value           NAT port mapping mechanism (any|none|upnp|pmp|extip:<IP>) (default: "any")
       --nodiscover          Disables the peer discovery mechanism (manual peer addition)
       --v5disc              Enables the experimental RLPx V5 (Topic Discovery) mechanism
       --netrestrict value   Restricts network communication to the given IP networks (CIDR masks)
       --nodekey value       P2P node key file
       --nodekeyhex value    P2P node key as hex (for testing)

MINER OPTIONS:
^^^^^^^^^^^^^^

::

     --mine                    Enable mining
     --minerthreads value      Number of CPU threads to use for mining (default: 8)
     --moacbase value          Public address for block mining rewards (default = first account created) (default: "0")
     --targetgaslimit value    Target gas limit sets the artificial target gas floor for the blocks to mine (default: 9000000)
      --gasprice "18000000000"  Minimal gas price to accept for mining a transactions
     --extradata value         Block extra data set by the miner (default = client version)

GAS PRICE ORACLE OPTIONS:
^^^^^^^^^^^^^^^^^^^^^^^^^

::

       --gpoblocks value      Number of recent blocks to check for gas prices (default: 10)
       --gpopercentile value  Suggested gas price is the given percentile of a set of recent transaction gas prices (default: 50)

VIRTUAL MACHINE OPTIONS:
^^^^^^^^^^^^^^^^^^^^^^^^

::

       --vmdebug  Record information useful for VM and contract debugging
      

LOGGING AND DEBUGGING OPTIONS:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

       --metrics                 Enable metrics collection and reporting
       --fakepow                 Disables proof-of-work verification
       --nocompaction            Disables db compaction after import
       --verbosity value         Logging verbosity: 0=silent, 1=error, 2=warn, 3=info, 4=debug, 5=detail (default: 3)
       --vmodule value           Per-module verbosity: comma-separated list of <pattern>=<level> (e.g. mc/=5,p2p=4)
       --backtrace value         Request a stack trace at a specific logging statement (e.g. "block.go:271")
       --debug                   Prepends log messages with call-site location (file and line number)
       --pprof                   Enable the pprof HTTP server
       --pprofaddr value         pprof HTTP server listening interface (default: "127.0.0.1")
       --pprofport value         pprof HTTP server listening port (default: 6060)
       --memprofilerate value    Turn on memory profiling with the given rate (default: 524288)
       --blockprofilerate value  Turn on block profiling with the given rate (default: 0)
       --cpuprofile value        Write CPU profile to the given file
       --trace value             Write execution trace to the given file

MISC OPTIONS:
^^^^^^^^^^^^^

::

       --fast      Enable fast syncing through state downloads
       --light     Enable light client mode
       --help, -h  show help
      

COPYRIGHT:
^^^^^^^^^^

::

       Copyright 2017-2018 MOAC Authors
