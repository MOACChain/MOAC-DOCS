JSON-RPC
========


.. raw:: html

   <!-- START doctoc generated TOC please keep comment here to allow auto update -->

.. raw:: html

   <!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

-  `JSON RPC API <#json-rpc-api>`__
-  `JavaScript API <#javascript-api>`__
-  `JSON-RPC Endpoint <#json-rpc-endpoint>`__

   -  `Go <#go>`__
   -  `C++ <#c>`__
   -  `Python <#python>`__

-  `JSON-RPC support <#json-rpc-support>`__
-  `HEX value encoding <#hex-value-encoding>`__
-  `The default block parameter <#the-default-block-parameter>`__
-  `Curl Examples Explained <#curl-examples-explained>`__
-  `JSON-RPC methods <#json-rpc-methods>`__
-  `JSON RPC API Reference <#json-rpc-api-reference>`__

   -  `chain3\_clientVersion <#web3_clientversion>`__

   -  `chain3\_sha3 <#web3_sha3>`__

   -  `net\_version <#net_version>`__

   -  `net\_listening <#net_listening>`__

   -  `net\_peerCount <#net_peercount>`__

   -  `mc\_protocolVersion <#eth_protocolversion>`__

   -  `mc\_syncing <#eth_syncing>`__

   -  `mc\_coinbase <#eth_coinbase>`__

   -  `mc\_mining <#eth_mining>`__

   -  `mc\_hashrate <#eth_hashrate>`__

   -  `mc\_gasPrice <#eth_gasprice>`__

   -  `mc\_accounts <#eth_accounts>`__

   -  `mc\_blockNumber <#eth_blocknumber>`__
   
   -  `mc\_getBalance <#eth_getbalance>`__

   -  `mc\_getStorageAt <#eth_getstorageat>`__

   -  `mc\_getTransactionCount <#eth_gettransactioncount>`__

   -  `mc\_getBlockTransactionCountByHash <#eth_getblocktransactioncountbyhash>`__

   -  `mc\_getBlockTransactionCountByNumber <#eth_getblocktransactioncountbynumber>`__

   -  `mc\_getUncleCountByBlockHash <#eth_getunclecountbyblockhash>`__

   -  `mc\_getUncleCountByBlockNumber <#eth_getunclecountbyblocknumber>`__

   -  `mc\_getCode <#eth_getcode>`__
   
   -  `mc\_sign <#eth_sign>`__

   -  `mc\_sendTransaction <#eth_sendtransaction>`__

   -  `mc\_sendRawTransaction <#eth_sendrawtransaction>`__
   
   -  `mc\_call <#eth_call>`__
   
   -  `mc\_estimateGas <#eth_estimategas>`__

   -  `mc\_getBlockByHash <#eth_getblockbyhash>`__

   -  `mc\_getBlockByNumber <#eth_getblockbynumber>`__

   -  `mc\_getTransactionByHash <#eth_gettransactionbyhash>`__

   -  `mc\_getTransactionByBlockHashAndIndex <#eth_gettransactionbyblockhashandindex>`__

   -  `mc\_getTransactionByBlockNumberAndIndex <#eth_gettransactionbyblocknumberandindex>`__

   -  `mc\_getTransactionReceipt <#eth_gettransactionreceipt>`__

   -  `mc\_getUncleByBlockHashAndIndex <#eth_getunclebyblockhashandindex>`__

   -  `mc\_getUncleByBlockNumberAndIndex <#eth_getunclebyblocknumberandindex>`__

   -  `mc\_getCompilers <#eth_getcompilers>`__

   -  `mc\_compileSolidity <#eth_compilesolidity>`__

   -  `mc\_compileLLL <#eth_compilelll>`__

   -  `mc\_compileSerpent <#eth_compileserpent>`__

   -  `mc\_newFilter <#eth_newfilter>`__

   -  `mc\_newBlockFilter <#eth_newblockfilter>`__

   -  `mc\_newPendingTransactionFilter <#eth_newpendingtransactionfilter>`__

   -  `mc\_uninstallFilter <#eth_uninstallfilter>`__

   -  `mc\_getFilterChanges <#eth_getfilterchanges>`__

   -  `mc\_getFilterLogs <#eth_getfilterlogs>`__

   -  `mc\_getLogs <#eth_getlogs>`__

   -  `mc\_getWork <#eth_getwork>`__

   -  `mc\_submitWork <#eth_submitwork>`__
   
   -  `mc\_submitHashrate <#eth_submithashrate>`__

   -  `db\_putString <#db_putstring>`__

   -  `db\_getString <#db_getstring>`__

   -  `db\_putHex <#db_puthex>`__

   -  `db\_getHex <#db_gethex>`__

   -  `shh\_version <#shh_version>`__

   -  `shh\_post <#shh_post>`__

   -  `shh\_newIdentity <#shh_newidentity>`__

   -  `shh\_hasIdentity <#shh_hasidentity>`__

   -  `shh\_newGroup <#shh_newgroup>`__

   -  `shh\_addToGroup <#shh_addtogroup>`__

   -  `shh\_newFilter <#shh_newfilter>`__

   -  `shh\_uninstallFilter <#shh_uninstallfilter>`__

   -  `shh\_getFilterChanges <#shh_getfilterchanges>`__

   -  `shh\_getMessages <#shh_getmessages>`__
   
  

.. raw:: html

   <!-- END doctoc generated TOC please keep comment here to allow auto update -->

JSON RPC API
------------

`JSON <http://json.org/>`__ is a lightweight data-interchange format. It
can represent numbers, strings, ordered sequences of values, and
collections of name/value pairs.

`JSON-RPC <http://www.jsonrpc.org/specification>`__ is a stateless,
light-weight remote procedure call (RPC) protocol. Primarily this
specification defines several data structures and the rules around their
processing. It is transport agnostic in that the concepts can be used
within the same process, over sockets, over HTTP, or in many various
message passing environments. It uses JSON (`RFC
4627 <http://www.ietf.org/rfc/rfc4627.txt>`__) as data format.

Geth 1.4 has experimental pub/sub support. See
`this <https://github.com/ethereum/go-ethereum/wiki/RPC-PUB-SUB>`__ page
for more information.

Parity 1.6 has experimental pub/sub support See
`this <https://github.com/paritytech/parity/wiki/JSONRPC-Eth-Pub-Sub-Module>`__
for more information.

JavaScript API
--------------

To talk to an ethereum node from inside a JavaScript application use the
`web3.js <https://github.com/ethereum/web3.js>`__ library, which gives a
convenient interface for the RPC methods. See the `JavaScript
API <https://github.com/ethereum/wiki/wiki/JavaScript-API>`__ for more.

JSON-RPC Endpoint
-----------------

Default JSON-RPC endpoints:

+----------+-------------------------+
| Client   | URL                     |
+==========+=========================+
| C++      | http://localhost:8545   |
+----------+-------------------------+
| Go       | http://localhost:8545   |
+----------+-------------------------+
| Py       | http://localhost:4000   |
+----------+-------------------------+
| Parity   | http://localhost:8545   |
+----------+-------------------------+

Go
~~

You can start the HTTP JSON-RPC with the ``--rpc`` flag

.. code:: bash

    geth --rpc

change the default port (8545) and listing address (localhost) with:

.. code:: bash

    geth --rpc --rpcaddr <ip> --rpcport <portnumber>

If accessing the RPC from a browser, CORS will need to be enabled with
the appropriate domain set. Otherwise, JavaScript calls are limit by the
same-origin policy and requests will fail:

.. code:: bash

    geth --rpc --rpccorsdomain "http://localhost:3000"

The JSON RPC can also be started from the `geth
console <https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console>`__
using the ``admin.startRPC(addr, port)`` command.

C++
~~~

You can start it by running ``eth`` application with ``-j`` option:

.. code:: bash

    ./eth -j

You can also specify JSON-RPC port (default is 8545):

.. code:: bash

    ./eth -j --json-rpc-port 8079

Python
~~~~~~

In python the JSONRPC server is currently started by default and listens
on ``127.0.0.1:4000``

You can change the port and listen address by giving a config option.

``pyethapp -c jsonrpc.listen_port=4002 -c jsonrpc.listen_host=127.0.0.2 run``

JSON-RPC support
----------------

+------------------+----------------+---------------+---------------+----------+
|                  | cpp-ethereum   | go-ethereum   | py-ethereum   | parity   |
+==================+================+===============+===============+==========+
| JSON-RPC 1.0     | ✓              |               |               |          |
+------------------+----------------+---------------+---------------+----------+
| JSON-RPC 2.0     | ✓              | ✓             | ✓             | ✓        |
+------------------+----------------+---------------+---------------+----------+
| Batch requests   | ✓              | ✓             | ✓             | ✓        |
+------------------+----------------+---------------+---------------+----------+
| HTTP             | ✓              | ✓             | ✓             | ✓        |
+------------------+----------------+---------------+---------------+----------+
| IPC              | ✓              | ✓             |               | ✓        |
+------------------+----------------+---------------+---------------+----------+
| WS               |                | ✓             |               | ✓        |
+------------------+----------------+---------------+---------------+----------+

HEX value encoding
------------------

At present there are two key datatypes that are passed over JSON:
unformatted byte arrays and quantities. Both are passed with a hex
encoding, however with different requirements to formatting:

When encoding **QUANTITIES** (integers, numbers): encode as hex, prefix
with "0x", the most compact representation (slight exception: zero
should be represented as "0x0"). Examples: - 0x41 (65 in decimal) -
0x400 (1024 in decimal) - WRONG: 0x (should always have at least one
digit - zero is "0x0") - WRONG: 0x0400 (no leading zeroes allowed) -
WRONG: ff (must be prefixed 0x)

When encoding **UNFORMATTED DATA** (byte arrays, account addresses,
hashes, bytecode arrays): encode as hex, prefix with "0x", two hex
digits per byte. Examples: - 0x41 (size 1, "A") - 0x004200 (size 3,
":raw-latex:`\0`B:raw-latex:`\0`") - 0x (size 0, "") - WRONG: 0xf0f0f
(must be even number of digits) - WRONG: 004200 (must be prefixed 0x)

Currently
`cpp-ethereum <https://github.com/ethereum/cpp-ethereum>`__,\ `go-ethereum <https://github.com/ethereum/go-ethereum>`__,
and `parity <https://github.com/paritytech/parity>`__ provide JSON-RPC
communication over http and IPC (unix socket Linux and OSX/named pipes
on Windows). Version 1.4 of go-ethereum and version 1.6 of Parity
onwards have websocket support.

The default block parameter
---------------------------

The following methods have an extra default block parameter:

-  `eth\_getBalance <#eth_getbalance>`__
-  `eth\_getCode <#eth_getcode>`__
-  `eth\_getTransactionCount <#eth_gettransactioncount>`__
-  `eth\_getStorageAt <#eth_getstorageat>`__
-  `eth\_call <#eth_call>`__

When requests are made that act on the state of ethereum, the last
default block parameter determines the height of the block.

The following options are possible for the defaultBlock parameter:

-  ``HEX String`` - an integer block number
-  ``String "earliest"`` for the earliest/genesis block
-  ``String "latest"`` - for the latest mined block
-  ``String "pending"`` - for the pending state/transactions

Curl Examples Explained
-----------------------

The curl options below might return a response where the node complains
about the content type, this is because the --data option sets the
content type to application/x-www-form-urlencoded . If your node does
complain, manually set the header by placing -H "Content-Type:
application/json" at the start of the call.

The examples also do not include the URL/IP & port combination which
must be the last argument given to curl e.x. 127.0.0.1:8545

JSON-RPC methods
----------------

-  `chain3\_clientVersion <#web3_clientversion>`__
-  `chain3\_sha3 <#web3_sha3>`__
-  `net\_version <#net_version>`__
-  `net\_peerCount <#net_peercount>`__
-  `net\_listening <#net_listening>`__
-  `mc\_protocolVersion <#eth_protocolversion>`__
-  `mc\_syncing <#eth_syncing>`__
-  `mc\_coinbase <#eth_coinbase>`__
-  `mc\_mining <#eth_mining>`__
-  `mc\_hashrate <#eth_hashrate>`__
-  `mc\_gasPrice <#eth_gasprice>`__
-  `mc\_accounts <#eth_accounts>`__
-  `mc\_blockNumber <#eth_blocknumber>`__
-  `mc\_getBalance <#eth_getbalance>`__
-  `mc\_getStorageAt <#eth_getstorageat>`__
-  `mc\_getTransactionCount <#eth_gettransactioncount>`__
-  `mc\_getBlockTransactionCountByHash <#eth_getblocktransactioncountbyhash>`__
-  `mc\_getBlockTransactionCountByNumber <#eth_getblocktransactioncountbynumber>`__
-  `mc\_getUncleCountByBlockHash <#eth_getunclecountbyblockhash>`__
-  `mc\_getUncleCountByBlockNumber <#eth_getunclecountbyblocknumber>`__
-  `mc\_getCode <#eth_getcode>`__
-  `mc\_sign <#eth_sign>`__
-  `mc\_sendTransaction <#eth_sendtransaction>`__
-  `mc\_sendRawTransaction <#eth_sendrawtransaction>`__
-  `mc\_call <#eth_call>`__
-  `mc\_estimateGas <#eth_estimategas>`__
-  `mc\_getBlockByHash <#eth_getblockbyhash>`__
-  `mc\_getBlockByNumber <#eth_getblockbynumber>`__
-  `mc\_getTransactionByHash <#eth_gettransactionbyhash>`__
-  `mc\_getTransactionByBlockHashAndIndex <#eth_gettransactionbyblockhashandindex>`__
-  `mc\_getTransactionByBlockNumberAndIndex <#eth_gettransactionbyblocknumberandindex>`__
-  `mc\_getTransactionReceipt <#eth_gettransactionreceipt>`__
-  `mc\_getUncleByBlockHashAndIndex <#eth_getunclebyblockhashandindex>`__
-  `mc\_getUncleByBlockNumberAndIndex <#eth_getunclebyblocknumberandindex>`__
-  `mc\_getCompilers <#eth_getcompilers>`__
-  `mc\_compileLLL <#eth_compilelll>`__
-  `mc\_compileSolidity <#eth_compilesolidity>`__
-  `mc\_compileSerpent <#eth_compileserpent>`__
-  `mc\_newFilter <#eth_newfilter>`__
-  `mc\_newBlockFilter <#eth_newblockfilter>`__
-  `mc\_newPendingTransactionFilter <#eth_newpendingtransactionfilter>`__
-  `mc\_uninstallFilter <#eth_uninstallfilter>`__
-  `mc\_getFilterChanges <#eth_getfilterchanges>`__
-  `mc\_getFilterLogs <#eth_getfilterlogs>`__
-  `mc\_getLogs <#eth_getlogs>`__
-  `mc\_getWork <#eth_getwork>`__
-  `mc\_submitWork <#eth_submitwork>`__
-  `mc\_submitHashrate <#eth_submithashrate>`__
-  `db\_putString <#db_putstring>`__
-  `db\_getString <#db_getstring>`__
-  `db\_putHex <#db_puthex>`__
-  `db\_getHex <#db_gethex>`__
-  `shh\_post <#shh_post>`__
-  `shh\_version <#shh_version>`__
-  `shh\_newIdentity <#shh_newidentity>`__
-  `shh\_hasIdentity <#shh_hasidentity>`__
-  `shh\_newGroup <#shh_newgroup>`__
-  `shh\_addToGroup <#shh_addtogroup>`__
-  `shh\_newFilter <#shh_newfilter>`__
-  `shh\_uninstallFilter <#shh_uninstallfilter>`__
-  `shh\_getFilterChanges <#shh_getfilterchanges>`__
-  `shh\_getMessages <#shh_getmessages>`__

JSON RPC API Reference
----------------------

--------------

web3\_clientVersion
~~~~~~~~~~~~~~~~~~~

Returns the current client version.

Parameters
''''''''''

none

Returns
'''''''

``String`` - The current client version

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":67}'

    // Result
    {
      "id":67,
      "jsonrpc":"2.0",
      "result": "Mist/v0.9.3/darwin/go1.4.1"
    }

--------------

web3\_sha3
~~~~~~~~~~

Returns Keccak-256 (*not* the standardized SHA3-256) of the given data.

Parameters
''''''''''

1. ``DATA`` - the data to convert into a SHA3 hash

.. code:: js

    params: [
      "0x68656c6c6f20776f726c64"
    ]

Returns
'''''''

``DATA`` - The SHA3 result of the given string.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"web3_sha3","params":["0x68656c6c6f20776f726c64"],"id":64}'

    // Result
    {
      "id":64,
      "jsonrpc": "2.0",
      "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
    }

--------------

net\_version
~~~~~~~~~~~~

Returns the current network id.

Parameters
''''''''''

none

Returns
'''''''

``String`` - The current network id. - ``"1"``: Ethereum Mainnet -
``"2"``: Morden Testnet (deprecated) - ``"3"``: Ropsten Testnet -
``"4"``: Rinkeby Testnet - ``"42"``: Kovan Testnet

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"net_version","params":[],"id":67}'

    // Result
    {
      "id":67,
      "jsonrpc": "2.0",
      "result": "3"
    }

--------------

net\_listening
~~~~~~~~~~~~~~

Returns ``true`` if client is actively listening for network
connections.

Parameters
''''''''''

none

Returns
'''''''

``Boolean`` - ``true`` when listening, otherwise ``false``.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"net_listening","params":[],"id":67}'

    // Result
    {
      "id":67,
      "jsonrpc":"2.0",
      "result":true
    }

--------------

net\_peerCount
~~~~~~~~~~~~~~

Returns number of peers currently connected to the client.

Parameters
''''''''''

none

Returns
'''''''

``QUANTITY`` - integer of the number of connected peers.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":74}'

    // Result
    {
      "id":74,
      "jsonrpc": "2.0",
      "result": "0x2" // 2
    }

--------------

eth\_protocolVersion
~~~~~~~~~~~~~~~~~~~~

Returns the current ethereum protocol version.

Parameters
''''''''''

none

Returns
'''''''

``String`` - The current ethereum protocol version

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_protocolVersion","params":[],"id":67}'

    // Result
    {
      "id":67,
      "jsonrpc": "2.0",
      "result": "54"
    }

--------------

eth\_syncing
~~~~~~~~~~~~

Returns an object with data about the sync status or ``false``.

Parameters
''''''''''

none

Returns
'''''''

``Object|Boolean``, An object with sync status data or ``FALSE``, when
not syncing: - ``startingBlock``: ``QUANTITY`` - The block at which the
import started (will only be reset, after the sync reached his head) -
``currentBlock``: ``QUANTITY`` - The current block, same as
eth\_blockNumber - ``highestBlock``: ``QUANTITY`` - The estimated
highest block

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_syncing","params":[],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": {
        startingBlock: '0x384',
        currentBlock: '0x386',
        highestBlock: '0x454'
      }
    }
    // Or when not syncing
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": false
    }

--------------

eth\_coinbase
~~~~~~~~~~~~~

Returns the client coinbase address.

Parameters
''''''''''

none

Returns
'''''''

``DATA``, 20 bytes - the current coinbase address.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_coinbase","params":[],"id":64}'

    // Result
    {
      "id":64,
      "jsonrpc": "2.0",
      "result": "0x407d73d8a49eeb85d32cf465507dd71d507100c1"
    }

--------------

eth\_mining
~~~~~~~~~~~

Returns ``true`` if client is actively mining new blocks.

Parameters
''''''''''

none

Returns
'''''''

``Boolean`` - returns ``true`` of the client is mining, otherwise
``false``.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_mining","params":[],"id":71}'

    // Result
    {
      "id":71,
      "jsonrpc": "2.0",
      "result": true
    }

--------------

eth\_hashrate
~~~~~~~~~~~~~

Returns the number of hashes per second that the node is mining with.

Parameters
''''''''''

none

Returns
'''''''

``QUANTITY`` - number of hashes per second.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_hashrate","params":[],"id":71}'

    // Result
    {
      "id":71,
      "jsonrpc": "2.0",
      "result": "0x38a"
    }

--------------

eth\_gasPrice
~~~~~~~~~~~~~

Returns the current price per gas in wei.

Parameters
''''''''''

none

Returns
'''''''

``QUANTITY`` - integer of the current gas price in wei.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":73}'

    // Result
    {
      "id":73,
      "jsonrpc": "2.0",
      "result": "0x09184e72a000" // 10000000000000
    }

--------------

eth\_accounts
~~~~~~~~~~~~~

Returns a list of addresses owned by client.

Parameters
''''''''''

none

Returns
'''''''

``Array of DATA``, 20 Bytes - addresses owned by the client.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]
    }

--------------

eth\_blockNumber
~~~~~~~~~~~~~~~~

Returns the number of most recent block.

Parameters
''''''''''

none

Returns
'''''''

``QUANTITY`` - integer of the current block number the client is on.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":83}'

    // Result
    {
      "id":83,
      "jsonrpc": "2.0",
      "result": "0x4b7" // 1207
    }

--------------

eth\_getBalance
~~~~~~~~~~~~~~~

Returns the balance of the account of given address.

Parameters
''''''''''

1. ``DATA``, 20 Bytes - address to check for balance.
2. ``QUANTITY|TAG`` - integer block number, or the string ``"latest"``,
   ``"earliest"`` or ``"pending"``, see the `default block
   parameter <#the-default-block-parameter>`__

.. code:: js

    params: [
       '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
       'latest'
    ]

Returns
'''''''

``QUANTITY`` - integer of the current balance in wei.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0x407d73d8a49eeb85d32cf465507dd71d507100c1", "latest"],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0x0234c8a3397aab58" // 158972490234375000
    }

--------------

eth\_getStorageAt
~~~~~~~~~~~~~~~~~

Returns the value from a storage position at a given address.

Parameters
''''''''''

1. ``DATA``, 20 Bytes - address of the storage.
2. ``QUANTITY`` - integer of the position in the storage.
3. ``QUANTITY|TAG`` - integer block number, or the string ``"latest"``,
   ``"earliest"`` or ``"pending"``, see the `default block
   parameter <#the-default-block-parameter>`__

Returns
'''''''

``DATA`` - the value at this storage position.

Example
'''''''

Calculating the correct position depends on the storage to retrieve.
Consider the following contract deployed at
``0x295a70b2de5e3953354a6a8344e616ed314d7251`` by address
``0x391694e7e0b0cce554cb130d723a9d27458f9298``.

::

    contract Storage {
        uint pos0;
        mapping(address => uint) pos1;
        
        function Storage() {
            pos0 = 1234;
            pos1[msg.sender] = 5678;
        }
    }

Retrieving the value of pos0 is straight forward:

.. code:: js

    curl -X POST --data '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x0", "latest"], "id": 1}' localhost:8545

    {"jsonrpc":"2.0","id":1,"result":"0x00000000000000000000000000000000000000000000000000000000000004d2"}

Retrieving an element of the map is harder. The position of an element
in the map is calculated with:

.. code:: js

    keccack(LeftPad32(key, 0), LeftPad32(map position, 0))

This means to retrieve the storage on
pos1["0x391694e7e0b0cce554cb130d723a9d27458f9298"] we need to calculate
the position with:

.. code:: js

    keccak(decodeHex("000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"))

The geth console which comes with the web3 library can be used to make
the calculation:

.. code:: js

    > var key = "000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"
    undefined
    > web3.sha3(key, {"encoding": "hex"})
    "0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9"

Now to fetch the storage:

.. code:: js

    curl -X POST --data '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9", "latest"], "id": 1}' localhost:8545

    {"jsonrpc":"2.0","id":1,"result":"0x000000000000000000000000000000000000000000000000000000000000162e"}

--------------

eth\_getTransactionCount
~~~~~~~~~~~~~~~~~~~~~~~~

Returns the number of transactions *sent* from an address.

Parameters
''''''''''

1. ``DATA``, 20 Bytes - address.
2. ``QUANTITY|TAG`` - integer block number, or the string ``"latest"``,
   ``"earliest"`` or ``"pending"``, see the `default block
   parameter <#the-default-block-parameter>`__

.. code:: js

    params: [
       '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
       'latest' // state at the latest block
    ]

Returns
'''''''

``QUANTITY`` - integer of the number of transactions send from this
address.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionCount","params":["0x407d73d8a49eeb85d32cf465507dd71d507100c1","latest"],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0x1" // 1
    }

--------------

eth\_getBlockTransactionCountByHash
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the number of transactions in a block from a block matching the
given block hash.

Parameters
''''''''''

1. ``DATA``, 32 Bytes - hash of a block

.. code:: js

    params: [
       '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
    ]

Returns
'''''''

``QUANTITY`` - integer of the number of transactions in this block.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0xb" // 11
    }

--------------

eth\_getBlockTransactionCountByNumber
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

        Returns the number of transactions in a block matching the given
        block number.

Parameters
''''''''''

1. ``QUANTITY|TAG`` - integer of a block number, or the string
   ``"earliest"``, ``"latest"`` or ``"pending"``, as in the `default
   block parameter <#the-default-block-parameter>`__.

.. code:: js

    params: [
       '0xe8', // 232
    ]

Returns
'''''''

``QUANTITY`` - integer of the number of transactions in this block.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByNumber","params":["0xe8"],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0xa" // 10
    }

--------------

eth\_getUncleCountByBlockHash
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the number of uncles in a block from a block matching the given
block hash.

Parameters
''''''''''

1. ``DATA``, 32 Bytes - hash of a block

.. code:: js

    params: [
       '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
    ]

Returns
'''''''

``QUANTITY`` - integer of the number of uncles in this block.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getUncleCountByBlockHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0x1" // 1
    }

--------------

eth\_getUncleCountByBlockNumber
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the number of uncles in a block from a block matching the given
block number.

Parameters
''''''''''

1. ``QUANTITY|TAG`` - integer of a block number, or the string "latest",
   "earliest" or "pending", see the `default block
   parameter <#the-default-block-parameter>`__

.. code:: js

    params: [
       '0xe8', // 232
    ]

Returns
'''''''

``QUANTITY`` - integer of the number of uncles in this block.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getUncleCountByBlockNumber","params":["0xe8"],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0x1" // 1
    }

--------------

eth\_getCode
~~~~~~~~~~~~

Returns code at a given address.

Parameters
''''''''''

1. ``DATA``, 20 Bytes - address
2. ``QUANTITY|TAG`` - integer block number, or the string ``"latest"``,
   ``"earliest"`` or ``"pending"``, see the `default block
   parameter <#the-default-block-parameter>`__

.. code:: js

    params: [
       '0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b',
       '0x2'  // 2
    ]

Returns
'''''''

``DATA`` - the code from the given address.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getCode","params":["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
    }

--------------

eth\_sign
~~~~~~~~~

The sign method calculates an Ethereum specific signature with:
``sign(keccak256("\x19Ethereum Signed Message:\n" + len(message) + message)))``.

By adding a prefix to the message makes the calculated signature
recognisable as an Ethereum specific signature. This prevents misuse
where a malicious DApp can sign arbitrary data (e.g. transaction) and
use the signature to impersonate the victim.

**Note** the address to sign with must be unlocked.

Parameters
''''''''''

account, message

1. ``DATA``, 20 Bytes - address
2. ``DATA``, N Bytes - message to sign

Returns
'''''''

``DATA``: Signature

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sign","params":["0x9b2055d370f73ec7d8a03e965129118dc8f5bf83", "0xdeadbeaf"],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
    }

An example how to use solidity ecrecover to verify the signature
calculated with ``eth_sign`` can be found
`here <https://gist.github.com/bas-vk/d46d83da2b2b4721efb0907aecdb7ebd>`__.
The contract is deployed on the testnet Ropsten and Rinkeby.

--------------

eth\_sendTransaction
~~~~~~~~~~~~~~~~~~~~

Creates new message call transaction or a contract creation, if the data
field contains code.

Parameters
''''''''''

1. ``Object`` - The transaction object

-  ``from``: ``DATA``, 20 Bytes - The address the transaction is send
   from.
-  ``to``: ``DATA``, 20 Bytes - (optional when creating new contract)
   The address the transaction is directed to.
-  ``gas``: ``QUANTITY`` - (optional, default: 90000) Integer of the gas
   provided for the transaction execution. It will return unused gas.
-  ``gasPrice``: ``QUANTITY`` - (optional, default: To-Be-Determined)
   Integer of the gasPrice used for each paid gas
-  ``value``: ``QUANTITY`` - (optional) Integer of the value sent with
   this transaction
-  ``data``: ``DATA`` - The compiled code of a contract OR the hash of
   the invoked method signature and encoded parameters. For details see
   `Ethereum Contract
   ABI <https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI>`__
-  ``nonce``: ``QUANTITY`` - (optional) Integer of a nonce. This allows
   to overwrite your own pending transactions that use the same nonce.

.. code:: js

    params: [{
      "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
      "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
      "gas": "0x76c0", // 30400
      "gasPrice": "0x9184e72a000", // 10000000000000
      "value": "0x9184e72a", // 2441406250
      "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
    }]

Returns
'''''''

``DATA``, 32 Bytes - the transaction hash, or the zero hash if the
transaction is not yet available.

Use `eth\_getTransactionReceipt <#eth_gettransactionreceipt>`__ to get
the contract address, after the transaction was mined, when you created
a contract.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendTransaction","params":[{see above}],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
    }

--------------

eth\_sendRawTransaction
~~~~~~~~~~~~~~~~~~~~~~~

Creates new message call transaction or a contract creation for signed
transactions.

Parameters
''''''''''

1. ``DATA``, The signed transaction data.

.. code:: js

    params: ["0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"]

Returns
'''''''

``DATA``, 32 Bytes - the transaction hash, or the zero hash if the
transaction is not yet available.

Use `eth\_getTransactionReceipt <#eth_gettransactionreceipt>`__ to get
the contract address, after the transaction was mined, when you created
a contract.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendRawTransaction","params":[{see above}],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
    }

--------------

eth\_call
~~~~~~~~~

Executes a new message call immediately without creating a transaction
on the block chain.

Parameters
''''''''''

1. ``Object`` - The transaction call object

-  ``from``: ``DATA``, 20 Bytes - (optional) The address the transaction
   is sent from.
-  ``to``: ``DATA``, 20 Bytes - The address the transaction is directed
   to.
-  ``gas``: ``QUANTITY`` - (optional) Integer of the gas provided for
   the transaction execution. eth\_call consumes zero gas, but this
   parameter may be needed by some executions.
-  ``gasPrice``: ``QUANTITY`` - (optional) Integer of the gasPrice used
   for each paid gas
-  ``value``: ``QUANTITY`` - (optional) Integer of the value sent with
   this transaction
-  ``data``: ``DATA`` - (optional) Hash of the method signature and
   encoded parameters. For details see `Ethereum Contract
   ABI <https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI>`__

2. ``QUANTITY|TAG`` - integer block number, or the string ``"latest"``,
   ``"earliest"`` or ``"pending"``, see the `default block
   parameter <#the-default-block-parameter>`__

Returns
'''''''

``DATA`` - the return value of executed contract.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_call","params":[{see above}],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0x"
    }

--------------

eth\_estimateGas
~~~~~~~~~~~~~~~~

Generates and returns an estimate of how much gas is necessary to allow
the transaction to complete. The transaction will not be added to the
blockchain. Note that the estimate may be significantly more than the
amount of gas actually used by the transaction, for a variety of reasons
including EVM mechanics and node performance.

Parameters
''''''''''

See `eth\_call <#eth_call>`__ parameters, expect that all properties are
optional. If no gas limit is specified geth uses the block gas limit
from the pending block as an upper bound. As a result the returned
estimate might not be enough to executed the call/transaction when the
amount of gas is higher than the pending block gas limit.

Returns
'''''''

``QUANTITY`` - the amount of gas used.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_estimateGas","params":[{see above}],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0x5208" // 21000
    }

--------------

eth\_getBlockByHash
~~~~~~~~~~~~~~~~~~~

Returns information about a block by hash.

Parameters
''''''''''

1. ``DATA``, 32 Bytes - Hash of a block.
2. ``Boolean`` - If ``true`` it returns the full transaction objects, if
   ``false`` only the hashes of the transactions.

.. code:: js

    params: [
       '0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331',
       true
    ]

Returns
'''''''

``Object`` - A block object, or ``null`` when no block was found:

-  ``number``: ``QUANTITY`` - the block number. ``null`` when its
   pending block.
-  ``hash``: ``DATA``, 32 Bytes - hash of the block. ``null`` when its
   pending block.
-  ``parentHash``: ``DATA``, 32 Bytes - hash of the parent block.
-  ``nonce``: ``DATA``, 8 Bytes - hash of the generated proof-of-work.
   ``null`` when its pending block.
-  ``sha3Uncles``: ``DATA``, 32 Bytes - SHA3 of the uncles data in the
   block.
-  ``logsBloom``: ``DATA``, 256 Bytes - the bloom filter for the logs of
   the block. ``null`` when its pending block.
-  ``transactionsRoot``: ``DATA``, 32 Bytes - the root of the
   transaction trie of the block.
-  ``stateRoot``: ``DATA``, 32 Bytes - the root of the final state trie
   of the block.
-  ``receiptsRoot``: ``DATA``, 32 Bytes - the root of the receipts trie
   of the block.
-  ``miner``: ``DATA``, 20 Bytes - the address of the beneficiary to
   whom the mining rewards were given.
-  ``difficulty``: ``QUANTITY`` - integer of the difficulty for this
   block.
-  ``totalDifficulty``: ``QUANTITY`` - integer of the total difficulty
   of the chain until this block.
-  ``extraData``: ``DATA`` - the "extra data" field of this block.
-  ``size``: ``QUANTITY`` - integer the size of this block in bytes.
-  ``gasLimit``: ``QUANTITY`` - the maximum gas allowed in this block.
-  ``gasUsed``: ``QUANTITY`` - the total used gas by all transactions in
   this block.
-  ``timestamp``: ``QUANTITY`` - the unix timestamp for when the block
   was collated.
-  ``transactions``: ``Array`` - Array of transaction objects, or 32
   Bytes transaction hashes depending on the last given parameter.
-  ``uncles``: ``Array`` - Array of uncle hashes.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByHash","params":["0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331", true],"id":1}'

    // Result
    {
    "id":1,
    "jsonrpc":"2.0",
    "result": {
        "number": "0x1b4", // 436
        "hash": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
        "parentHash": "0x9646252be9520f6e71339a8df9c55e4d7619deeb018d2a3f2d21fc165dde5eb5",
        "nonce": "0xe04d296d2460cfb8472af2c5fd05b5a214109c25688d3704aed5484f9a7792f2",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "logsBloom": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
        "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "stateRoot": "0xd5855eb08b3387c0af375e9cdb6acfc05eb8f519e419b874b6ff2ffda7ed1dff",
        "miner": "0x4e65fda2159562a496f9f3522f89122a3088497a",
        "difficulty": "0x027f07", // 163591
        "totalDifficulty":  "0x027f07", // 163591
        "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "size":  "0x027f07", // 163591
        "gasLimit": "0x9f759", // 653145
        "gasUsed": "0x9f759", // 653145
        "timestamp": "0x54e34e8e" // 1424182926
        "transactions": [{...},{ ... }] 
        "uncles": ["0x1606e5...", "0xd5145a9..."]
      }
    }

--------------

eth\_getBlockByNumber
~~~~~~~~~~~~~~~~~~~~~

Returns information about a block by block number.

Parameters
''''''''''

1. ``QUANTITY|TAG`` - integer of a block number, or the string
   ``"earliest"``, ``"latest"`` or ``"pending"``, as in the `default
   block parameter <#the-default-block-parameter>`__.
2. ``Boolean`` - If ``true`` it returns the full transaction objects, if
   ``false`` only the hashes of the transactions.

.. code:: js

    params: [
       '0x1b4', // 436
       true
    ]

Returns
'''''''

See `eth\_getBlockByHash <#eth_getblockbyhash>`__

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["0x1b4", true],"id":1}'

Result see `eth\_getBlockByHash <#eth_getblockbyhash>`__

--------------

eth\_getTransactionByHash
~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the information about a transaction requested by transaction
hash.

Parameters
''''''''''

1. ``DATA``, 32 Bytes - hash of a transaction

.. code:: js

    params: [
       "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"
    ]

Returns
'''''''

``Object`` - A transaction object, or ``null`` when no transaction was
found:

-  ``hash``: ``DATA``, 32 Bytes - hash of the transaction.
-  ``nonce``: ``QUANTITY`` - the number of transactions made by the
   sender prior to this one.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this
   transaction was in. ``null`` when its pending.
-  ``blockNumber``: ``QUANTITY`` - block number where this transaction
   was in. ``null`` when its pending.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position in the block. ``null`` when its pending.
-  ``from``: ``DATA``, 20 Bytes - address of the sender.
-  ``to``: ``DATA``, 20 Bytes - address of the receiver. ``null`` when
   its a contract creation transaction.
-  ``value``: ``QUANTITY`` - value transferred in Wei.
-  ``gasPrice``: ``QUANTITY`` - gas price provided by the sender in Wei.
-  ``gas``: ``QUANTITY`` - gas provided by the sender.
-  ``input``: ``DATA`` - the data send along with the transaction.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

    // Result
    {
    "id":1,
    "jsonrpc":"2.0",
    "result": {
        "hash":"0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b",
        "nonce":"0x",
        "blockHash": "0xbeab0aa2411b7ab17f30a99d3cb9c6ef2fc5426d6ad6fd9e2a26a6aed1d1055b",
        "blockNumber": "0x15df", // 5599
        "transactionIndex":  "0x1", // 1
        "from":"0x407d73d8a49eeb85d32cf465507dd71d507100c1",
        "to":"0x85h43d8a49eeb85d32cf465507dd71d507100c1",
        "value":"0x7f110", // 520464
        "gas": "0x7f110", // 520464
        "gasPrice":"0x09184e72a000",
        "input":"0x603880600c6000396000f300603880600c6000396000f3603880600c6000396000f360",
      }
    }

--------------

eth\_getTransactionByBlockHashAndIndex
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns information about a transaction by block hash and transaction
index position.

Parameters
''''''''''

1. ``DATA``, 32 Bytes - hash of a block.
2. ``QUANTITY`` - integer of the transaction index position.

.. code:: js

    params: [
       '0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331',
       '0x0' // 0
    ]

Returns
'''''''

See `eth\_getTransactionByHash <#eth_gettransactionbyhash>`__

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockHashAndIndex","params":["0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b", "0x0"],"id":1}'

Result see `eth\_getTransactionByHash <#eth_gettransactionbyhash>`__

--------------

eth\_getTransactionByBlockNumberAndIndex
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns information about a transaction by block number and transaction
index position.

Parameters
''''''''''

1. ``QUANTITY|TAG`` - a block number, or the string ``"earliest"``,
   ``"latest"`` or ``"pending"``, as in the `default block
   parameter <#the-default-block-parameter>`__.
2. ``QUANTITY`` - the transaction index position.

.. code:: js

    params: [
       '0x29c', // 668
       '0x0' // 0
    ]

Returns
'''''''

See `eth\_getTransactionByHash <#eth_gettransactionbyhash>`__

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockNumberAndIndex","params":["0x29c", "0x0"],"id":1}'

Result see `eth\_getTransactionByHash <#eth_gettransactionbyhash>`__

--------------

eth\_getTransactionReceipt
~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the receipt of a transaction by transaction hash.

**Note** That the receipt is not available for pending transactions.

Parameters
''''''''''

1. ``DATA``, 32 Bytes - hash of a transaction

.. code:: js

    params: [
       '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
    ]

Returns
'''''''

``Object`` - A transaction receipt object, or ``null`` when no receipt
was found:

-  ``transactionHash``: ``DATA``, 32 Bytes - hash of the transaction.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position in the block.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this
   transaction was in.
-  ``blockNumber``: ``QUANTITY`` - block number where this transaction
   was in.
-  ``cumulativeGasUsed``: ``QUANTITY`` - The total amount of gas used
   when this transaction was executed in the block.
-  ``gasUsed``: ``QUANTITY`` - The amount of gas used by this specific
   transaction alone.
-  ``contractAddress``: ``DATA``, 20 Bytes - The contract address
   created, if the transaction was a contract creation, otherwise
   ``null``.
-  ``logs``: ``Array`` - Array of log objects, which this transaction
   generated.
-  ``logsBloom``: ``DATA``, 256 Bytes - Bloom filter for light clients
   to quickly retrieve related logs.

It also returns *either* :

-  ``root`` : ``DATA`` 32 bytes of post-transaction stateroot (pre
   Byzantium)
-  ``status``: ``QUANTITY`` either ``1`` (success) or ``0`` (failure)

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionReceipt","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

    // Result
    {
    "id":1,
    "jsonrpc":"2.0",
    "result": {
         transactionHash: '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238',
         transactionIndex:  '0x1', // 1
         blockNumber: '0xb', // 11
         blockHash: '0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b',
         cumulativeGasUsed: '0x33bc', // 13244
         gasUsed: '0x4dc', // 1244
         contractAddress: '0xb60e8dd61c5d32be8058bb8eb970870f07233155', // or null, if none was created
         logs: [{
             // logs as returned by getFilterLogs, etc.
         }, ...],
         logsBloom: "0x00...0", // 256 byte bloom filter
         status: '0x1'
      }
    }

--------------

eth\_getUncleByBlockHashAndIndex
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns information about a uncle of a block by hash and uncle index
position.

Parameters
''''''''''

1. ``DATA``, 32 Bytes - hash a block.
2. ``QUANTITY`` - the uncle's index position.

.. code:: js

    params: [
       '0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b',
       '0x0' // 0
    ]

Returns
'''''''

See `eth\_getBlockByHash <#eth_getblockbyhash>`__

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getUncleByBlockHashAndIndex","params":["0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b", "0x0"],"id":1}'

Result see `eth\_getBlockByHash <#eth_getblockbyhash>`__

**Note**: An uncle doesn't contain individual transactions.

--------------

eth\_getUncleByBlockNumberAndIndex
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns information about a uncle of a block by number and uncle index
position.

Parameters
''''''''''

1. ``QUANTITY|TAG`` - a block number, or the string ``"earliest"``,
   ``"latest"`` or ``"pending"``, as in the `default block
   parameter <#the-default-block-parameter>`__.
2. ``QUANTITY`` - the uncle's index position.

.. code:: js

    params: [
       '0x29c', // 668
       '0x0' // 0
    ]

Returns
'''''''

See `eth\_getBlockByHash <#eth_getblockbyhash>`__

**Note**: An uncle doesn't contain individual transactions.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getUncleByBlockNumberAndIndex","params":["0x29c", "0x0"],"id":1}'

Result see `eth\_getBlockByHash <#eth_getblockbyhash>`__

--------------

eth\_getCompilers
~~~~~~~~~~~~~~~~~

Returns a list of available compilers in the client.

Parameters
''''''''''

none

Returns
'''''''

``Array`` - Array of available compilers.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getCompilers","params":[],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": ["solidity", "lll", "serpent"]
    }

--------------

eth\_compileSolidity
~~~~~~~~~~~~~~~~~~~~

Returns compiled solidity code.

Parameters
''''''''''

1. ``String`` - The source code.

.. code:: js

    params: [
       "contract test { function multiply(uint a) returns(uint d) {   return a * 7;   } }",
    ]

Returns
'''''''

``DATA`` - The compiled source code.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_compileSolidity","params":["contract test { function multiply(uint a) returns(uint d) {   return a * 7;   } }"],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": {
          "code": "0x605880600c6000396000f3006000357c010000000000000000000000000000000000000000000000000000000090048063c6888fa114602e57005b603d6004803590602001506047565b8060005260206000f35b60006007820290506053565b91905056",
          "info": {
            "source": "contract test {\n   function multiply(uint a) constant returns(uint d) {\n       return a * 7;\n   }\n}\n",
            "language": "Solidity",
            "languageVersion": "0",
            "compilerVersion": "0.9.19",
            "abiDefinition": [
              {
                "constant": true,
                "inputs": [
                  {
                    "name": "a",
                    "type": "uint256"
                  }
                ],
                "name": "multiply",
                "outputs": [
                  {
                    "name": "d",
                    "type": "uint256"
                  }
                ],
                "type": "function"
              }
            ],
            "userDoc": {
              "methods": {}
            },
            "developerDoc": {
              "methods": {}
            }
          }

    }

--------------

eth\_compileLLL
~~~~~~~~~~~~~~~

Returns compiled LLL code.

Parameters
''''''''''

1. ``String`` - The source code.

.. code:: js

    params: [
       "(returnlll (suicide (caller)))",
    ]

Returns
'''''''

``DATA`` - The compiled source code.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_compileLLL","params":["(returnlll (suicide (caller)))"],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0x603880600c6000396000f3006001600060e060020a600035048063c6888fa114601857005b6021600435602b565b8060005260206000f35b600081600702905091905056" // the compiled source code
    }

--------------

eth\_compileSerpent
~~~~~~~~~~~~~~~~~~~

Returns compiled serpent code.

Parameters
''''''''''

1. ``String`` - The source code.

.. code:: js

    params: [
       "/* some serpent */",
    ]

Returns
'''''''

``DATA`` - The compiled source code.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_compileSerpent","params":["/* some serpent */"],"id":1}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0x603880600c6000396000f3006001600060e060020a600035048063c6888fa114601857005b6021600435602b565b8060005260206000f35b600081600702905091905056" // the compiled source code
    }

--------------

eth\_newFilter
~~~~~~~~~~~~~~

Creates a filter object, based on filter options, to notify when the
state changes (logs). To check if the state has changed, call
`eth\_getFilterChanges <#eth_getfilterchanges>`__.

A note on specifying topic filters:
'''''''''''''''''''''''''''''''''''

Topics are order-dependent. A transaction with a log with topics [A, B]
will be matched by the following topic filters: \* ``[]`` "anything" \*
``[A]`` "A in first position (and anything after)" \* ``[null, B]``
"anything in first position AND B in second position (and anything
after)" \* ``[A, B]`` "A in first position AND B in second position (and
anything after)" \* ``[[A, B], [A, B]]`` "(A OR B) in first position AND
(A OR B) in second position (and anything after)"

Parameters
''''''''''

1. ``Object`` - The filter options:

-  ``fromBlock``: ``QUANTITY|TAG`` - (optional, default: ``"latest"``)
   Integer block number, or ``"latest"`` for the last mined block or
   ``"pending"``, ``"earliest"`` for not yet mined transactions.
-  ``toBlock``: ``QUANTITY|TAG`` - (optional, default: ``"latest"``)
   Integer block number, or ``"latest"`` for the last mined block or
   ``"pending"``, ``"earliest"`` for not yet mined transactions.
-  ``address``: ``DATA|Array``, 20 Bytes - (optional) Contract address
   or a list of addresses from which logs should originate.
-  ``topics``: ``Array of DATA``, - (optional) Array of 32 Bytes
   ``DATA`` topics. Topics are order-dependent. Each topic can also be
   an array of DATA with "or" options.

.. code:: js

    params: [{
      "fromBlock": "0x1",
      "toBlock": "0x2",
      "address": "0x8888f1f195afa192cfee860698584c030f4c9db1",
      "topics": ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", null, ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x0000000000000000000000000aff3454fce5edbc8cca8697c15331677e6ebccc"]]
    }]

Returns
'''''''

``QUANTITY`` - A filter id.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newFilter","params":[{"topics":["0x12341234"]}],"id":73}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0x1" // 1
    }

--------------

eth\_newBlockFilter
~~~~~~~~~~~~~~~~~~~

Creates a filter in the node, to notify when a new block arrives. To
check if the state has changed, call
`eth\_getFilterChanges <#eth_getfilterchanges>`__.

Parameters
''''''''''

None

Returns
'''''''

``QUANTITY`` - A filter id.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newBlockFilter","params":[],"id":73}'

    // Result
    {
      "id":1,
      "jsonrpc":  "2.0",
      "result": "0x1" // 1
    }

--------------

eth\_newPendingTransactionFilter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Creates a filter in the node, to notify when new pending transactions
arrive. To check if the state has changed, call
`eth\_getFilterChanges <#eth_getfilterchanges>`__.

Parameters
''''''''''

None

Returns
'''''''

``QUANTITY`` - A filter id.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newPendingTransactionFilter","params":[],"id":73}'

    // Result
    {
      "id":1,
      "jsonrpc":  "2.0",
      "result": "0x1" // 1
    }

--------------

eth\_uninstallFilter
~~~~~~~~~~~~~~~~~~~~

Uninstalls a filter with given id. Should always be called when watch is
no longer needed. Additonally Filters timeout when they aren't requested
with `eth\_getFilterChanges <#eth_getfilterchanges>`__ for a period of
time.

Parameters
''''''''''

1. ``QUANTITY`` - The filter id.

.. code:: js

    params: [
      "0xb" // 11
    ]

Returns
'''''''

``Boolean`` - ``true`` if the filter was successfully uninstalled,
otherwise ``false``.

Example
'''''''

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"eth_uninstallFilter","params":["0xb"],"id":73}'

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": true
    }

--------------

eth\_getFilterChanges
~~~~~~~~~~~~~~~~~~~~~

Polling method for a filter, which returns an array of logs which
occurred since last poll.

Parameters
''''''''''

1. ``QUANTITY`` - the filter id.

.. code:: js

    params: [
      "0x16" // 22
    ]

Returns
'''''''

``Array`` - Array of log objects, or an empty array if nothing has
changed since last poll.

-  For filters created with ``eth_newBlockFilter`` the return are block
   hashes (``DATA``, 32 Bytes), e.g. ``["0x3454645634534..."]``.
-  For filters created with ``eth_newPendingTransactionFilter`` the
   return are transaction hashes (``DATA``, 32 Bytes), e.g.
   ``["0x6345343454645..."]``.
-  For filters created with ``eth_newFilter`` logs are objects with
   following params:

-  ``removed``: ``TAG`` - ``true`` when the log was removed, due to a
   chain reorganization. ``false`` if its a valid log.
-  ``logIndex``: ``QUANTITY`` - integer of the log index position in the
   block. ``null`` when its pending log.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position log was created from. ``null`` when its pending log.
-  ``transactionHash``: ``DATA``, 32 Bytes - hash of the transactions
   this log was created from. ``null`` when its pending log.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this log
   was in. ``null`` when its pending. ``null`` when its pending log.
-  ``blockNumber``: ``QUANTITY`` - the block number where this log was
   in. ``null`` when its pending. ``null`` when its pending log.
-  ``address``: ``DATA``, 20 Bytes - address from which this log
   originated.
-  ``data``: ``DATA`` - contains one or more 32 Bytes 
