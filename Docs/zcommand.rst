基本指令
^^^^^^^^^

--testnet: 连接到MOAC测试网络；

--rpc: 启用HTTP的RPC服务，以便非本机访问该MOAC节点服务；

--rpcaddr value: 默认是"localhost", 只能本机访问； 可通过设置
为"0.0.0.0", 以便非本机访问该MOAC节点服务,
但现在RPC服务是基于HTTP的，是明文传输，需注意安全问题；

--rpcport value: 默认是"8545", 一般来说不用改，用默认端口即可；

--rpcapi value:
指定RPC要开放的API服务，默认为"chain3,mc,net"，常用的一般还会配置比如personal,admin,debug,miner,txpool,db,shh等，但是因为RPC服务是明文传输，所以，如果使用personal的时候，要注意安全问题；

--rpccorsdomain value:
一般来说，如果你知道要用这个选项的时候，使用"\*"值即可，更详细可自行搜索“CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin
resource sharing）”中相关关键词进一步了解；

--jspath loadScript: 默认值为".", loadScript装载javascript文件的主目录；

示例如下：


./moac attach /xx/xxx/moac.ipc 通过本地ipc接口连接到MOAC节点

./moac attach http://xxx.xxx.xxx.xxx:8545
通过基于HTTP的RPC接口连接到本地或者远程MOAC节点

./moac --testnet 启动MOAC测试节点

./moac --testnet console 启动MOAC测试节点，并启动交互命令行

./moac --testnet --rpc 启动MOAC测试节点，同时启动RPC服务

./moac --testnet --rpc --rpcaddr=0.0.0.0
--rpcapi="db,mc,net,chain3,personal,debug" --rpccorsdomain="\*"
启动MOAC测试节点，非本机可访问，非本机也可以使用personal及debug服务，同时提供跨域资源共享服务；
