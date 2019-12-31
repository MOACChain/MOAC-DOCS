.. _scs-setup:

SCS start guide
^^^^^^^^^^^^^^^^^^

Configurations
-----------------

Official MOAC SCS can be downloaded from the `release
link <https://github.com/MOACChain/moac-core/releases>`__，

Before start the SCS client, make sure to modify the userconfig.json.
The contents of the userconfig.json is the following:

1. VnodeServiceCfg：The VNODE IP and port for the SCS to connect. Each SCS
   needs to connect a VNODE to communicate with the BaseChain. You can
   setup a local VNODE or use a trust VNODE from other sources. Default value is localhost：50062.
2. DataDir：SCS data directory that holding the MicroChain data. Default
   is "./scsdata".
3. LogPath：SCS log directory, default is "./_logs".
4. Beneficiary：Account holds the MicroChain mining rewards. Please
   don't use the SCS account for this. We suggest you create this
   account separatly and don't put the keystore file on SCS.
5. VnodeChainId：network id with the BaseChain. Testnet = 101 and
   mainnet = 99. If you have a custome network, you need to make sure
   the vnode connect with has the same network id.
6. LogLevel：Logging verbosity, 0=silent, 1=error, 2=warn, 3=info, 4=debug (default: 4)
7. Capability: desposit limit for one AppChain. Since each MicroChain
   requires some desposit to join, you can set this number and only join
   the MicroChain with deposit requirement less than this limit.
8. ReconnectInterval: If the connection is lost with vnode, SCS will try
   to connect with the vnode again. This is the number of seconds
   between each connection with vnode.
9. BondLimit: SCS can choose the AppChain it wants to join. A top limit of the mc deposit amount to join one AppChain. If an AppChain requires more deposit than this limit, SCS won't join the AppChain as a validator(miner). Default value is 2, unit is mc.
10. ReWardMin: Minimum reward number for the SCS to join one AppChain. If the AppChain rewards less than this value, SCS won't join the AppChain as a validator(miner). Default values is 0.0001, unit is mc.

Installation guide
-----------------

Debian/Ubuntu/CentOS
~~~~~~~~~~~~~~~~~~~~

1. 使用 tar 来解压缩，可以看到scs目录下有三个文件：
README          - 说明文件;
scsserver       - 可执行程序;
userconfig.json - 配置文件；

在当前目录下运行：
 ./scsserver

如果要看帮助信息, 使用help参数:
./scsserver --help

SCS OPTIONS:
  --datadir "/Users/zpli/go/src/github.com/innowells/releases/nuwa1.0.11/mac/scs/scsdata"  Data directory for the microchain databases
  --password value                                                                         password of the SCS keystore (default: "moacscsofflineaccountpwd")
  
API OPTIONS:
  --rpc                  Enable the HTTP JSON-RPC server
  --rpcdebug             Enable the HTTP-RPC Debug server
  --rpcaddr value        HTTP-RPC server listening interface (default: "127.0.0.1")
  --rpcport value        HTTP-RPC server listening port (default: 8548)
  --rpcapi value         API's offered over the HTTP-RPC interface
  --rpccorsdomain value  Comma separated list of domains from which to accept cross origin requests (browser enforced)
  --userconfig value     userconfig [file path]
  --verbosity value      sets the verbosity level, override loglevel in userconfig.json (default: 2)
  --help, -h             show help
  --version, -v          print the version


WINDOWS
-------

解压文件，应该可以看到目录内的三个文件：

scsserver.exe

打开命令（cmd）终端，转到SCS解压目录，在命令行中执行：

::

    D:\scs>scsserver.exe --help

其它操作参考 Debian/Ubuntu/CentOS。




