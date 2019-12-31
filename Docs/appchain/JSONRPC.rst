.. _scs_chain3js01x:

JSON RPC 
--------

`JSON <http://json.org/>` is a lightweight data-interchange format. It
can represent numbers, strings, ordered sequences of values, and
collections of name/value pairs.

`JSON-RPC <http://www.jsonrpc.org/specification>`is a stateless,
light-weight remote procedure call (RPC) protocol. Primarily this
specification defines several data structures and the rules around their
processing. It is transport agnostic in that the concepts can be used
within the same process, over sockets, over HTTP, or in many various
message passing environments. It uses JSON (`RFC
4627 <http://www.ietf.org/rfc/rfc4627.txt>`) as data format.

MOAC JSON-RPC has some compatibility with ETHEREUM JSON-RPC,



SCS JSON-RPC methods
====================


MOAC 平台中的应用链节点 SCS 内置了两种 JSON-RPC 接口来方便用户使用。
一种是 rpc 接口，为普通用户使用。
另一种是 rpcdebug 接口，为开发者使用。
默认 JSON-RPC 的接入为:

+----------+----------+---------------------------+
| Client   | Command  | URL                       |
+==========+==========+===========================+
| Go       | rpc      | http://localhost:8548     |
+----------+----------+---------------------------+
| Go       | rpcdebug | http://localhost:8548/rpc |
+----------+----------+---------------------------+



在SCS节点启动时，可以使用 HTTP JSON-RPC 选项``--rpc`` 命令来开启：

--rpc: 启用HTTP的RPC服务，以便非本机访问该MOAC节点服务；

SCS的JSON-RPC接口默认使用同源的服务方式。如果要通过浏览器来访问 JSON-RPC 接口，需要允许CORS选项，否则访问无法成功：

启用HTTP的RPC服务，以便非本机访问该MOAC节点服务

.. code:: bash

    scsserver --rpc

SCS 默认RPC接口的地址是 localhost，端口号是 8548，可以通过 rpcaddr 和 rpcport 选项进行修改：

.. code:: bash

    scsserver --rpc --rpcaddr <ip> --rpcport <portnumber>

如果要通过浏览器来访问 JSON-RPC 接口，需要允许CORS选项，否则访问无法成功：

.. code:: bash

    scsserver --rpc --rpccorsdomain "http://localhost:8548"

应用链RPCDEBUG接口
=================


在SCS节点启动时，可以使用 HTTP JSON-RPC 选项``--rpc`` 命令来开启：

--rpc: 启用HTTP的RPC服务，以便非本机访问该MOAC节点服务；

SCS的JSON-RPC接口默认使用同源的服务方式。如果要通过浏览器来访问 JSON-RPC 接口，需要允许CORS选项，否则访问无法成功：

启用HTTP的RPC服务，以便非本机访问该MOAC节点服务

.. code:: bash

    scsserver --rpcdebug

SCS 默认RPC接口的地址是 localhost，端口号是 8548/rpc，可以通过 rpcaddr 和 rpcport 选项进行修改：

.. code:: bash

    scsserver --rpcdebug --rpcaddr <ip> --rpcport <portnumber>

如果要通过浏览器来访问 JSON-RPC 接口，需要允许CORS选项，否则访问无法成功：

.. code:: bash

    scsserver --rpcdebug --rpccorsdomain "http://localhost:8548"

根据应用链启动的参数，用户可以访问对应端口，调用接口获取应用链相关数据，注意url中要加入rpc

::url  = "http://127.0.0.1:2345/rpc";

以调用接口GetScsId为列，获得当前的scs编号，即scs keystore文件中的地址。

关于rpcdebug http的接口访问，可以用类似postman之类的工具进行快捷测试
::
  header设置：
    Content-Type = application/json
    Accept = application/json
    
  Body设置：
    {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod\_GetScsId","params":{}}
    
  post返回结果：
    {
      "jsonrpc": "2.0",
      "id": 0,
      "result": "0xd135afa5c8d96ba11c40cf0b52952d54bce57363"
    }
    
同时也可以通过nodejs的方式调用
::
  > request = require('request');
  > url  = "http://127.0.0.1:2345/rpc";  
  > data = {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod\_GetScsId","params":{}};
  > request({ url: url, method: "POST", json: true, body: data, headers: {"Content-Type": 'application/json', "Accept": 'application/json'}}, function(error, response, result) {if (!error && response.statusCode == 200) {console.log(result)}});


默认区块值
=================

以下接口包含默认区块值。如果没有有效区块数值输入，则使用应用链的最高区块高度：

-  :ref:`scs\_getBalance <scs_getbalance>`
-  :ref:`scs\_directCall <scs_directcall>`

When requests are made that act on the state of moac, the last default
block parameter determines the height of the block.

The following options are possible for the defaultBlock parameter:

-  ``HEX String`` - an integer block number
-  ``String "earliest"`` for the earliest/genesis block
-  ``String "latest"`` - for the latest mined block
-  ``String "pending"`` - for the pending state/transactions

Curl command example
====================

The curl options below might return a response where the node complains
about the content type, this is because the --data option sets the
content type to application/x-www-form-urlencoded . If your node does
complain, manually set the header by placing -H "Content-Type:
application/json" at the start of the call.

The examples assume a local MOAC node is running and connected to
testnet (network id = 101). The URL/IP & port combination is
localhost:8545 or '127.0.0.1', which must be the last argument given to
curl.

JSON-RPC Methods
=================

-  rpc

   -  :ref:`scs_directCall <scs_directcall>`
   -  :ref:`scs_getBalance <scs_getbalance>`   
   -  :ref:`scs_getBlock <scs_getblock>`
   -  :ref:`scs_getBlockNumber <scs_getblocknumber>`
   -  :ref:`scs_getDappList <scs_getdapplist>`
   -  :ref:`scs_getDappState <scs-getdappstate>`
   -  :ref:`scs_getAppChainInfo <scs_getmicrochaininfo>`
   -  :ref:`scs_getAppChainList <scs_getmicrochainlist>`
   -  :ref:`scs\_getNonce <scs_getnonce>`
   -  :ref:`scs\_getSCSId <scs_getscsid>`
   -  :ref:`scs\_getTransactionByHash <scs_gettransactionbyhash>`
   -  :ref:`scs\_getTransactionByNonce <scs_gettransactionbynonce>`
   -  :ref:`scs\_getReceiptByHash <scs_getreceiptbyhash>`
   -  :ref:`scs\_getReceiptByNonce <scs_getreceiptbynonce>`
   -  :ref:`scs\_getExchangeByAddress <scs_getexchangebyaddress>`
   -  :ref:`scs\_getExchangeInfo <scs_getexchangeinfo>`
   -  :ref:`scs\_getTxpool <scs_gettxpool>`

-  rpcdebug

   -  :ref:`ScsRPCMethod\_GetNonce <rpcdebug_GetNonce>`
   -  :ref:`ScsRPCMethod\_GetBalance <rpcdebug_GetBalance>`   
   -  :ref:`ScsRPCMethod\_GetBlockNumber <rpcdebug_GetBlockNumber>`
   -  :ref:`ScsRPCMethod\_GetBlock <rpcdebug_GetBlock>`
   -  :ref:`ScsRPCMethod\_GetBlocks <rpcdebug_GetBlocks>`
   -  :ref:`ScsRPCMethod\_GetSubChainInfo <rpcdebug_GetSubChainInfo>`
   -  :ref:`ScsRPCMethod\_GetTxpool <rpcdebug_GetTxpool>`
   -  :ref:`ScsRPCMethod\_GetTxpoolCount <rpcdebug_GetTxpoolCount>`
   -  :ref:`ScsRPCMethod\_GetDappState <rpcdebug_GetDappState>`
   -  :ref:`ScsRPCMethod\_GetDappAddrList <rpcdebug_GetDappAddrList>`
   -  :ref:`ScsRPCMethod\_GetExchangeInfo <rpcdebug_GetExchangeInfo>`
   -  :ref:`ScsRPCMethod\_GetExchangeByAddress <rpcdebug_GetExchangeByAddress>`
   -  :ref:`ScsRPCMethod\_GetGetTransactionByNonce <rpcdebug_GetTransactionByNonce>`
   -  :ref:`ScsRPCMethod\_GetTransactionByHash <rpcdebug_GetTransactionByHash>`
   -  :ref:`ScsRPCMethod\_GetReceiptByNonce <rpcdebug_GetReceiptByNonce>`
   -  :ref:`ScsRPCMethod\_GetReceiptByHash <rpcdebug_GetReceiptByHash>`
   -  :ref:`ScsRPCMethod\_AnyCall <rpcdebug_AnyCall>`

--------------

RPC
'''

.. _scs_directcall:

**scs_directCall**

Executes a new constant call of the AppChain Dapp function without
creating a transaction on the AppChain. This RPC call is used by
API/lib to call AppChain Dapp functions.

*Parameters*


``Object`` - The transaction call object 
- ``from``: ``DATA``, 20 Bytes - (optional) The address the transaction is sent from. 
- ``to``: ``DATA``, 20 Bytes - The address the transaction is directed to. This
parameter is the AppChain address. 
- ``data``: ``DATA`` - (optional) Hash of the method signature and encoded parameters. For details see
`Ethereum Contract ABI <https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI>`

*Returns*


``DATA`` - the return value of executed Dapp constant function call.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_directCall","params":[{see above}],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x"
    }

--------------

**scs\_getBlock**

.. _scs_getblock:

Returns information about a block on the AppChain by block number.

*Parameters*


1. ``String`` - the address of the AppChain that Dapp is on.
2. ``QUANTITY|TAG`` - integer of a block number, or the string
   ``"earliest"`` or ``"latest"``, as in the `default block
   parameter <#the-default-block-parameter>`. Note, scs\_getBlock does
   not support ``"pending"``.

*Returns*


``DATA`` - Data in the block on the AppChain.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getBlock","params":["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8","0x1"],"id":101}' localhost:8548

    // Result
    {"jsonrpc":"2.0","id":101,"result":{"extraData":"0x","hash":"0xc80cbe08bc266b1236f22a8d0b310faae3135961dbef6ad8b6ad4e8cd9537309","number":"0x1","parentHash":"0x0000000000000000000000000000000000000000000000000000000000000000","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0x1a065207da60d8e7a44db2f3b5ed9d3e81052a3059e4108c84701d0bf6a62292","timestamp":"0x0","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"}}

--------------

**scs\_getBlockList**

.. _scs_getblocklist:

Returns information about multiple AppChain blocks by block number.

*Parameters*


1. ``String`` - the address of the AppChain that Dapp is on.
2. ``QUANTITY`` - integer of the start block number.
3. ``QUANTITY`` - integer of the end block number, need to be larger or equal the start block number.

*Returns*


``ARRAY`` - Array of the block infromation on the AppChain.

Example


.. code:: js

    // Request
    curl -X POST --data curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getBlock","params":["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8","0x370", "0x373"],"id":101}' localhost:8548

    // Result
    {"jsonrpc":"2.0","id":101,"result":{"blockList":[{"extraData":"0x","hash":"0x56075838e0fffe6576add14783b957239d4f3c57989bc3a7b7728a3b57eb305a","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x370","parentHash":"0x56352a3a8bd0901608041115817204cbce943606e406d233d7d0359f449bd4c2","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c1c","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"},{"extraData":"0x","hash":"0xbc3f5791ec039cba99c37310a4f30a68030dd2ab79efb47d23fd9ac5343f54e5","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x371","parentHash":"0x56075838e0fffe6576add14783b957239d4f3c57989bc3a7b7728a3b57eb305a","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c3a","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"},{"extraData":"0x","hash":"0x601be17c47cb4684053457d1d5f70a6dbeb853b27cda08d160555f857f2da33b","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x372","parentHash":"0xbc3f5791ec039cba99c37310a4f30a68030dd2ab79efb47d23fd9ac5343f54e5","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c58","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"},{"extraData":"0x","hash":"0x8a0bea649bcdbd2b525690ff485e56d5a83443e9013fcdccd1a0adee56ba4092","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x373","parentHash":"0x601be17c47cb4684053457d1d5f70a6dbeb853b27cda08d160555f857f2da33b","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c76","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"}],"endBlk":"0x373","microchainAddress":"0x7D0CbA876cB9Da5fa310A54d29F4687f5dd93fD7","startBlk":"0x370"}}

--------------

**scs\_getBlockNumber**

.. _scs_getblocknumber:

Returns the number of most recent block .

*Parameters*

1. ``String`` - the address of the AppChain that Dapp is on.

*Returns*

``QUANTITY`` - integer of the current block number the client is on.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getBlockNumber","params":["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x4b7" // 1207
    }
--------------

**scs\_getDappList**

.. _scs_getdapplist:

Returns the Dapp addresses on the AppChain. For nuwa 1.0.8 and later version only, 

*Parameters*

1. ``String`` - the address of the AppChain that has Dapps.

*Returns*

``ARRAY`` - Array of the DAPP addresses on the AppChain.

Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getDappList","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": ["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8","0x7cfd775c7a97aa632846eff35dcf9dbcf502d0f3"]
    }

--------------

**scs\_getDappState**

.. _scs_getdappstate:

Returns the Dapp state on the AppChain.

*Parameters*

1. ``String`` - the address of the AppChain that Dapp is on.

*Returns*

``QUANTITY`` - 0, no DAPP is deployed on the AppChain; 1, DAPP is
deployed on the AppChain.

Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getDappState","params":["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": 1
    }

--------------
**scs\_getMicroChainInfo**

.. _scs_getmicrochaininfo:

Returns the requested AppChain information on the connecting SCS. This information is the same as the information defined in the AppChain contract.

*Parameters*

1. `String` - the address of the AppChain on the SCS.

*Returns*


``Object`` A Micro Chain information object as defined in the AppChain contract.

Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getMicroChainInfo","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": {"balance":"0x0","blockReward":"0x1c6bf52634000","bondLimit":"0xde0b6b3a7640000","owner":"0xa8863fc8Ce3816411378685223C03DAae9770ebB","scsList":["0xECd1e094Ee13d0B47b72F5c940C17bD0c7630326","0x50C15fafb95968132d1a6ee3617E99cCa1FCF059","0x1b65cE1A393FFd5960D2ce11E7fd6fDB9e991945"],"txReward":"0x174876e800","viaReward":"0x9184e72a000"}
    }

--------------
**scs\_getMicroChainList**

.. _scs_getmicrochainlist:

Returns the list of MicroChains on the SCS that is connecting with. 

*Parameters*

None

*Returns*

``Array`` - A list of Micro Chain addresses on the SCS.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getMicroChainList","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": ["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8","0x7cfd775c7a97aa632846eff35dcf9dbcf502d0f3"]
    }

--------------

**scs\_getNonce**

.. _scs_getNonce:

Returns the account nonce on the AppChain. 

*Parameters*


1. ``String`` - the address of the AppChain that Dapp is on.
2. ``String`` - the address of the accountn.

*Returns*


``QUANTITY`` integer of the number of transactions send from this address on the AppChain; 

Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getNonce","params":["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8", "0x7312F4B8A4457a36827f185325Fd6B66a3f8BB8B"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": 1
    }

--------------

**scs\_getSCSId**

.. _scs_getSCSId:

Returns the SCS id.

*Parameters*

None

*Returns*


1. ``String`` - SCS id in the scskeystore directory, used for SCS
identification to send deposit and receive AppChain mining rewards.

Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getSCSId","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8"
    }

--------------

**scs\_getReceiptByHash**

.. _scs_getReceiptByHash:

Returns the receipt of a transaction by transaction hash. Note That the
receipt is not available for pending transactions.

*Parameters*

1. ``String`` - The AppChain address. 
2. ``String`` - The transaction hash.

*Returns*


``Object`` - A transaction receipt object, or null when no receipt was
found:.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getReceiptByHash","params":["0x299afff2da4a57e7e0a0a16bf626f8822b8a3158","0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    }

--------------

**scs\_getReceiptByNonce**

.. _scs_getReceiptByNonce:

Returns the transaction result by address and nonce on the AppChain. Note That the nonce is the nonce on the AppChain. This nonce can be checked using scs_getNonce. 

*Parameters*


1. ``String`` - The AppChain address. 
1. ``String`` - The transaction nonce.
1. ``QUANTITY`` - The nonce of the transaction.

*Returns*

``Object`` - A transaction receipt object, or null when no receipt was
found:.

Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getReceiptByNonce","params":["0x299afff2da4a57e7e0a0a16bf626f8822b8a3158","0xa8863fc8ce3816411378685223c03daae9770ebb", 0],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    }

--------------

**scs\_getTransactionByHash**

.. _scs_gettransactionbyhash:

Returns the receipt of a transaction by transaction hash. Note That the
receipt is not available for pending transactions.

*Parameters*

1. ``String`` - The AppChain address. 
2. ``String`` - The transaction hash.

*Returns*


``Object`` - A transaction object, or null when no transaction was found.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getTransactionByHash","params":["0x299afff2da4a57e7e0a0a16bf626f8822b8a3158","0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    }

--------------

**scs\_getTransactionByNonce**

.. _scs_gettransactionbynonce:

Returns the receipt of a transaction by transaction hash. Note That the
receipt is not available for pending transactions.

*Parameters*

1. ``String`` - The AppChain address. 
1. ``String`` - The transaction nonce.
1. ``QUANTITY`` - The nonce of the transaction.

*Returns*


``Object`` - A transaction receipt object, or null when no receipt was
found:.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getTransactionByNonce","params":["0x299afff2da4a57e7e0a0a16bf626f8822b8a3158","0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    
--------------

**scs\_getExchangeByAddress**

.. _scs_getExchangeByAddress:

Returns the Withdraw/Deposit exchange records between AppChain and MotherChain for a certain address. This command returns both the ongoing exchanges and processed exchanges. To check all the ongoing exchanges, please use scs_getExchangeInfo. 


*Parameters*

1. `String` - The AppChain address.
1. `String` - The address to be checked.
1. `Int` - Index of Deposit records >= 0.
1. `Int` - Number of Deposit records extracted.
1. `Int` - Index of Depositing records >= 0.
1. `Int` - Number of Depositing records extracted.
1. `Int` - Index of Withdraw records >= 0.
1. `Int` - Number of Withdraw records extracted.
1. `Int` - Index of Withdrawing records >= 0.
1. `Int` - Number of Withdrawing records extracted.

*Returns*


``Object`` - A JSON format object contains the token exchange info.

Example


.. code:: js

  // Request
  curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getExchangeByAddress","params":["0x2e4694875de2a7da9c3186a176c52760d58694e4","0xa8863fc8ce3816411378685223c03daae9770ebb", 0,10,0,10,0,10,0,10],"id":100}' localhost:8548

  // Result
  {"jsonrpc":"2.0","id":100,"result":{"DepositRecordCount":2,"DepositRecords":[null,null,{"DepositAmt":"0x18abedda5a37000","Deposittime":"0x5c7f03c4"},{"DepositAmt":"0x2bdbb64bc09000","Deposittime":"0x5c7e8aaa"}],"DepositingRecordCount":0,"DepositingRecords":null,"WithdrawRecordCount":0,"WithdrawRecords":null,"WithdrawingRecordCount":0,"WithdrawingRecords":null,"microchain":"0x2e4694875de2a7da9c3186a176c52760d58694e4","sender":"0xa8863fc8ce3816411378685223c03daae9770ebb"}}


--------------


**scs\_getExchangeInfo**

.. _scs_getExchangeInfo:

Returns the Withdraw/Deposit exchange records between AppChain and MotherChain for a certain address. This command returns both the ongoing exchanges and processed exchanges. To check all the ongoing exchanges, please use scs_getExchangeInfo. 


*Parameters*

1. `String` - The AppChain address.
1. `String` - The transaction hash.
1. `Int` - Index of Depositing records >= 0.
1. `Int` - Number of Depositing records extracted.
1. `Int` - Index of Withdrawing records >= 0.
1. `Int` - Number of Withdrawing records extracted.

*Returns*


``Object`` - A JSON format object contains the token exchange info.

Example


.. code:: js

  // Request
  curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getExchangeInfo","params":["0x2e4694875de2a7da9c3186a176c52760d58694e4",0,10,0,10],"id":101}' 'localhost:8545'

  // Result

  {"jsonrpc":"2.0","id":100,"result":{"DepositingRecordCount":0,"DepositingRecords":null,"WithdrawingRecordCount":0,"WithdrawingRecords":null,"microchain":"0x2e4694875de2a7da9c3186a176c52760d58694e4","scsid":"0x50c15fafb95968132d1a6ee3617e99cca1fcf059"}}

--------------


**scs\_getTxpool**

.. _scs_gettxpool:

Returns the ongoing transactions in the AppChain. 

*Parameters*

1. `String` - The AppChain address.

*Returns*

``Object`` - A JSON format object contains two fields pending and queued. Each of these fields are associative arrays, in which each entry maps an origin-address to a batch of scheduled transactions. These batches themselves are maps associating nonces with actual transactions.

Example


.. code:: js

  // Request
  curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getTxpool","params":["0x2e4694875de2a7da9c3186a176c52760d58694e4"],"id":101}' 'localhost:8545'

  // Result

  {"jsonrpc":"2.0","id":100,"result":{"pending":{},"queued":{}}}
--------------

RPCDEBUG
'''''''''

以下是几个常用的rpcdebug接口及调用body示例（nuwa v1.0.9以上）

***通用类***

此部分接口和应用链区块本身相关，业务逻辑无关。

**GetNonce**

.. _rpcdebug_GetNonce:

Get the nonce，returns the number of transactions sent from an address.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

*Returns*

QUANTITY - integer of the number of transactions send from this address.

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1}


**GetBalance**

.. _rpcdebug_GetBalance:

获得对应账号在应用链中的原生币余额。

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

Returns

QUANTITY - integer of the current balance in AppChain native token with digits.

Example

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetBalance","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1000}


**GetBlock**

.. _rpcdebug_GetBlock:

GetBlock:  获得当前应用链的指定的区块信息

Get the nonce，returns the number of transactions sent from an address.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

*Returns*

QUANTITY - integer of the number of transactions send from this address.

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1}

::
  SubChainAddr: 应用链合约地址
  Sender：查询账号
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetBlock",
      "params":{"number":1000,"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09"}
      }

.. _rpcdebug_GetBlocks:

GetBlocks: 获取某一区间内的区块信息
Get the nonce，returns the number of transactions sent from an address.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

*Returns*

QUANTITY - integer of the number of transactions send from this address.

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1}

::
  SubChainAddr: 应用链合约地址
  Start: 开始block
  End： 结束block
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetBlocks",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
        "Start":10, "End":20}
      }

.. _rpcdebug_GetBlockNumber:

GetBlockNumber：获得当前应用链的区块高度
Get the nonce，returns the number of transactions sent from an address.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

*Returns*

QUANTITY - integer of the number of transactions send from this address.

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1}

::
  SubChainAddr: 应用链合约地址
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetBlockNumber",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09"}
      }

.. _rpcdebug_GetSubChainInfo:

GetSubChainInfo：获得当前应用链的信息
Get the nonce，returns the number of transactions sent from an address.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

*Returns*

QUANTITY - integer of the number of transactions send from this address.

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1}
::
  SubChainAddr: 应用链合约地址
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetSubChainInfo",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09"}
      }

.. _rpcdebug_GetTxpool:

GetTxpool：获得应用链交易池信息
Get the nonce，returns the number of transactions sent from an address.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

*Returns*

QUANTITY - integer of the number of transactions send from this address.

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1}
::
  SubChainAddr: 应用链合约地址
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetTxpool",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09"
       }
      }

.. _rpcdebug_GetTxpoolCount:

GetTxpoolCount：获得应用链交易池中不同类型交易的数量

Get the nonce，returns the number of transactions sent from an address.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

*Returns*

QUANTITY - integer of the number of transactions send from this address.

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1}

::
  SubChainAddr: 应用链合约地址
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetTxpoolCount",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09"
       }
      }

.. _rpcdebug_GetDappState:

GetDappState：获得应用链基础合约合约的状态

Get the nonce，returns the number of transactions sent from an address.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

*Returns*

QUANTITY - integer of the number of transactions send from this address.

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1}

::
  SubChainAddr: 应用链合约地址
  Sender：应用链合约地址创建者地址
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetDappState",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
        "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
       }
      }

.. _rpcdebug_GetDappAddrList:

GetDappAddrList：通过应用链地址获取应用链内所有多合约的地址列表，需要应用链业务逻辑合约调用基础合约registerDapp方法后才能生效，
具体请参见 :ref:`ProcWind 跨链指南<proc-wind-as>` 中的示例

Get the nonce，returns the number of transactions sent from an address.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

*Returns*

QUANTITY - integer of the number of transactions send from this address.

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1}

::
  SubChainAddr: 应用链合约地址
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetDappAddrList",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09"
       }
      }

返回result中，第零位是dappbase的地址，从第一位开始时业务逻辑合约地址

***充提类***

.. _rpcdebug_GetExchangeInfo:

GetExchangeInfo：获得应用链指定数量正在充提的信息

Get the nonce，returns the number of transactions sent from an address.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

*Returns*

QUANTITY - integer of the number of transactions send from this address.

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1}

::
  SubChainAddr: 应用链合约地址
  EnteringRecordIndex： 正在充值记录的起始位置(0)
  EnteringRecordSize: 正在充值记录的长度
  RedeemingRecordIndex： 正在提币记录的起始位置(0)
  RedeemingRecordSize： 正在提币记录的长度
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetExchangeInfo",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
      "EnteringRecordIndex":0, "EnteringRecordSize": 10,
      "RedeemingRecordIndex":0, "RedeemingRecordSize": 10
       }
      }

返回中，XXXRecordCount是指总数量

.. _rpcdebug_GetExchangeByAddress:

GetExchangeByAddress：获得应用链指定账号指定数量的充提信息

Get the nonce，returns the number of transactions sent from an address.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.

*Returns*

QUANTITY - integer of the number of transactions send from this address.

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0","id":101,"result":1}

::
  SubChainAddr: 应用链合约地址
  Sender：需要查询的账号地址
  EnterRecordIndex： 已经充值记录的起始位置(0)
  EnterRecordSize: 已经充值记录的长度
  RedeemRecordIndex： 已经提币记录的起始位置(0)
  RedeemRecordSize： 已经提币记录的长度
  EnteringRecordIndex： 正在充值记录的起始位置(0)
  EnteringRecordSize: 正在充值记录的长度
  RedeemingRecordIndex： 正在提币记录的起始位置(0)
  RedeemingRecordSize： 正在提币记录的长度
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetExchangeByAddress",
      "params":{"Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736",
      "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
      "EnterRecordIndex":0, "EnterRecordSize": 10,
      "RedeemRecordIndex":0, "RedeemRecordSize", 10
      "EnteringRecordIndex":0, "EnteringRecordSize": 10,
      "RedeemingRecordIndex":0, "RedeemingRecordSize", 10
       }
      }

返回中，XXXRecordCount是指总数量

***交易类***

此部分接口和交易相关，当有应用链交易发生（sf>0），即可以在这里查看交易和交易结果

.. _rpcdebug_GetTransactionByNonce:

GetTransactionByNonce: 通过账号和Nonce获取应用链的tx信息

Returns the information about an AppChain transaction requested by sender  and nonce.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.
3. `QUANTITY` - integer of the transaction index of the sender.

*Returns*

Object - A transaction object, or null when no transaction was found.

-  ``transactionHash``: ``DATA``, 32 Bytes - hash of the transaction.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position in the block.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this
   transaction was in.
-  ``blockNumber``: ``QUANTITY`` - block number where this transaction
   was in.

-  ``contractAddress``: ``DATA``, 20 Bytes - The contract address
   created, if the transaction was a contract creation, otherwise
   ``null``.
-  ``logs``: ``Array`` - Array of log objects, which this transaction
   generated.
-  ``logsBloom``: ``DATA``, 256 Bytes - Bloom filter for light clients
   to quickly retrieve related logs.
- ``failed``: ``Boolean`` - ``true`` if the filter was successfully uninstalled,
otherwise ``false``.
-  ``status``: ``QUANTITY`` either ``1`` (success) or ``0`` (failure)

Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetTransactionByNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736", "Nonce": 9,
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0",
  "id":101,
  "result":{contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}}

::
  SubChainAddr: 应用链合约地址
  Sender：查询账号
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetTransactionByNonce",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
        "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"， "Nonce": 9,
        }
      }

.. _rpcdebug_GetTransactionByHash:

GetTransactionByHash: 通过交易HASH获取应用链的交易信息，注意HASH可以用:ref:`GetTransactionByNonce<rpcdebug_GetTransactionByNonce>` 方法获得。

Returns the information about an AppChain transaction requested by transaction hash.

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.
3. `DATA`, 32 Bytes - Hash of a transaction.

*Returns*

Object - A transaction object, or null when no transaction was found.

-  ``transactionHash``: ``DATA``, 32 Bytes - hash of the transaction.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position in the block.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this
   transaction was in.
-  ``blockNumber``: ``QUANTITY`` - block number where this transaction
   was in.

-  ``contractAddress``: ``DATA``, 20 Bytes - The contract address
   created, if the transaction was a contract creation, otherwise
   ``null``.
-  ``logs``: ``Array`` - Array of log objects, which this transaction
   generated.
-  ``logsBloom``: ``DATA``, 256 Bytes - Bloom filter for light clients
   to quickly retrieve related logs.
- ``failed``: ``Boolean`` - ``true`` if the filter was successfully uninstalled,
otherwise ``false``.
-  ``status``: ``QUANTITY`` either ``1`` (success) or ``0`` (failure)


Example:

.. code:: js

  // Request
  curl -X POST --header "Content-Type:application/json" --header "Accept:application/json" --data '{"jsonrpc":"2.0","method":"ScsRPCMethod.GetTransactionByNonce","params":{
     "SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
          "Hash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69", 
         },"id":101}' 127.0.0.1:8548/rpc

  // Result

  {"jsonrpc":"2.0",
  "id":101,
  "result":{contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}}

::
  SubChainAddr: 应用链合约地址
  Hash: 交易hash
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetTransactionByHash",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
        "Hash":"0x87e369172af1e817ebd8d63bcd9f685a513a6736fsne3lkgkvu65kkwlcd"
        }
      }

.. _rpcdebug_GetReceiptByNonce:

GetReceiptByNonce: 通过账号和Nonce获取应用链的tx执行结果

Returns the transaction result by address and nonce on the AppChain. Note That the nonce is the nonce on the AppChain. This nonce can be checked using :ref:`ScsRPCMethod.GetNonce <rpcdebug_GetNonce>` method. 

*Parameters*

1. `String` - SubChainAddr, AppChain address.
2. `String` - Sender, source account address that has the nonce.
3. `QUANTITY` - integer of the transaction index of the sender.

*Returns*

``Object`` - A transaction receipt object, or ``null`` when no receipt
was found:

-  ``transactionHash``: ``DATA``, 32 Bytes - hash of the transaction.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position in the block.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this
   transaction was in.
-  ``blockNumber``: ``QUANTITY`` - block number where this transaction
   was in.

-  ``contractAddress``: ``DATA``, 20 Bytes - The contract address
   created, if the transaction was a contract creation, otherwise
   ``null``.
-  ``logs``: ``Array`` - Array of log objects, which this transaction
   generated.
-  ``logsBloom``: ``DATA``, 256 Bytes - Bloom filter for light clients
   to quickly retrieve related logs.
- ``failed``: ``Boolean`` - ``true`` if the filter was successfully uninstalled,
otherwise ``false``.
-  ``status``: ``QUANTITY`` either ``1`` (success) or ``0`` (failure)


::
  SubChainAddr: 应用链合约地址
  Sender：查询账号
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetReceiptByNonce",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
        "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"， "Nonce":9
        }
      }

注意：如果这是个合约部署的交易，则在contractAddress将会显示合约地址；如果是一个有返回值的方法调用，则在result中显示调用结果

.. _rpcdebug_GetReceiptByHash:

GetReceiptByHash: 通过交易hash获取应用链的tx执行结果，注意HASH可以用:ref:`GetReceiptByNonce<rpcdebug_GetReceiptByNonce>` 方法获得
::
  SubChainAddr: 应用链合约地址
  Sender：查询账号
  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.GetReceiptByHash",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
        "Hash":"0x87e369172af1e817ebd8d63bcd9f685a513a6736fsne3lkgkvu65kkwlcd"
        }
      }

注意：如果这是个合约部署的交易，则在contractAddress将会显示合约地址；如果是一个有返回值的方法调用，则在result中显示调用结果

***业务类***

此部分合约需要指明是哪个业务逻辑合约

**AnyCall**

.. _rpcdebug_AnyCall:

获取dapp合约函数的返回值，**调用此接口前必须将dapp注册入dappbase**

Params： 第一个参数是调用的方法，之后是方法传入参数
  SubChainAddr: 应用链合约地址
  Sender：查询账号
  DappAddr:应用链业务逻辑地址

::

  Body: {"jsonrpc":"2.0","id":0,"method":"ScsRPCMethod.AnyCall",
      "params":{"SubChainAddr":"0x1195cd9769692a69220312e95192e0dcb6a4ec09",
        "DappAddr":"0xcc0D18E77748AeBe3cC6462be0EF724e391a4aD9",
        "Sender":"0x87e369172af1e817ebd8d63bcd9f685a513a6736"， "Params" :["funcA", "param1", param2]
        }
      }
