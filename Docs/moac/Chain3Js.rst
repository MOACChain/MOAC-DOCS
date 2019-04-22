Chain3 JavaScript library
=========================

Chain3 JavaScript API was built for MOAC chain. It followed the Ethereum
web3.js API routines so the users can easily move their Ðapp to MOAC
chain.

To make a Ðapp work on MOAC network, user should use the ``chain3``
object provided by the `chain3.js
library <https://github.com/MOACChain/chain3.js>`__. It communicates to
a local MOAC node through `RPC
calls <https://github.com/MOACChain/moac-core/wiki/RPC>`__. chain3.js
works with any MOAC node, which exposes an RPC layer.

``chain3`` contains the ``mc`` object - ``chain3.mc`` (for MOAC
blockchain VNODE interactions) and account.js (for transaction signing).
Over time we'll introduce other objects such as SCS for the other chain3
protocols. Working examples can be found
`here <https://github.com/MOACChain/Chain3/tree/master/example>`__.

Using callbacks
---------------

As this API is designed to work with a local RPC node and all its
functions are by default use synchronous HTTP requests.con

If you want to make an asynchronous request, you can pass an optional
callback as the last parameter to most functions. All callbacks are
using an `error first
callback <http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/>`__
style:

.. code:: js

    chain3.mc.getBlock(48, function(error, result){
        if(!error)
            console.log(result)
        else
            console.error(error);
    })

Batch requests
--------------

Batch requests allow queuing up requests and processing them at once.

.. code:: js

    var batch = chain3.createBatch();
    batch.add(chain3.mc.getBalance.request('0x0000000000000000000000000000000000000000', 'latest', callback));
    batch.add(chain3.mc.contract(abi).at(address).balance.request(address, callback2));
    batch.execute();

A note on big numbers in chain3.js
----------------------------------

You will always get a BigNumber object for balance values as JavaScript
is not able to handle big numbers correctly. Look at the following
examples:

.. code:: js

    "101010100324325345346456456456456456456"
    // "101010100324325345346456456456456456456"
    101010100324325345346456456456456456456
    // 1.0101010032432535e+38

chain3.js depends on the `BigNumber
Library <https://github.com/MikeMcl/bignumber.js/>`__ and adds it
automatically.

.. code:: js

    var balance = new BigNumber('123456786979284780000000000');
    // or var balance = chain3.mc.getBalance(someAddress);

    balance.plus(21).toString(10); // toString(10) converts it to a number string
    // "123456786979284780000000021"

The next example wouldn't work as we have more than 20 floating points,
therefore it is recommended that you always keep your balance in *sha*
and only transform it to other units when presenting to the user:

.. code:: js

    var balance = new BigNumber('13124.234435346456466666457455567456');

    balance.plus(21).toString(10); // toString(10) converts it to a number string, but can only show max 20 floating points
    // "13145.23443534645646666646" // you number would be cut after the 20 floating point

Chain3 Javascript Ðapp API Reference
------------------------------------

-  `chain3 <#chain3>`__
-  `version <#chain3versionapi>`__

   -  `api <#chain3versionapi>`__
   -  `node <#chain3versionnode>`__
   -  `network <#chain3versionnetwork>`__
   -  `moac <#chain3versionmoac>`__

-  `isConnected() <####chain3isconnected>`__
-  `setProvider(provider) <#chain3setprovider>`__
-  `currentProvider <#chain3currentprovider>`__
-  `reset() <#chain3reset>`__
-  `sha3(string) <#chain3sha3>`__
-  `toHex(stringOrNumber) <#chain3tohex>`__
-  `toAscii(hexString) <#chain3toascii>`__
-  `fromAscii(textString, [padding]) <#chain3fromascii>`__
-  `toDecimal(hexString) <#chain3todecimal>`__
-  `toChecksumAddress(string) <#chain3tochecksumaddress>`__
-  `fromDecimal(number) <#chain3fromdecimal>`__
-  `fromSha(numberStringOrBigNumber, unit) <#chain3fromsha>`__
-  `toSha(numberStringOrBigNumber, unit) <#chain3tosha>`__
-  `toBigNumber(numberOrHexString) <#chain3tobignumber>`__
-  `isAddress(hexString) <#chain3isAddress>`__
-  `net <#chain3net>`__

   -  `listening/getListening <#chain3netlistening>`__
   -  `peerCount/getPeerCount <#chain3mcpeercount>`__

-  `mc <#chain3mc>`__

   -  `defaultAccount <#chain3mcdefaultaccount>`__
   -  `defaultBlock <#chain3mcdefaultblock>`__
   -  `syncing/getSyncing <#chain3mcsyncing>`__
   -  `isSyncing <#chain3mcissyncing>`__
   -  `coinbase/getCoinbase <#chain3mccoinbase>`__
   -  `hashrate/getHashrate <#chain3mchashrate>`__
   -  `gasPrice/getGasPrice <#chain3mcgasprice>`__
   -  `accounts/getAccounts <#chain3mcaccounts>`__
   -  `mining/getMining <#chain3mcmining>`__
   -  `blockNumber/getBlockNumber <#chain3mcblocknumber>`__
   -  `getBalance(address) <#chain3mcgetbalance>`__
   -  `getStorageAt(address, position) <#chain3mcgetstorageat>`__
   -  `getCode(address) <#chain3mcgetcode>`__
   -  `getBlock(hash/number) <#chain3mcgetblock>`__
   -  `getBlockTransactionCount(hash/number) <#chain3mcgetblocktransactioncount>`__
   -  `getUncle(hash/number) <#chain3mcgetuncle>`__
   -  `getBlockUncleCount(hash/number) <#chain3mcgetblockunclecount>`__
   -  `getTransaction(hash) <#chain3mcgettransaction>`__
   -  `getTransactionFromBlock(hashOrNumber,
      indexNumber) <#chain3mcgettransactionfromblock>`__
   -  `getTransactionReceipt(hash) <#chain3mcgettransactionreceipt>`__
   -  `getTransactionCount(address) <#chain3mcgettransactioncount>`__
   -  `sendTransaction(object) <#chain3mcsendtransaction>`__
   -  `call(object) <#chain3mccall>`__
   -  `estimateGas(object) <#chain3mcestimategas>`__
   -  `filter(array (, options) ) <#chain3mcfilter>`__

      -  `watch(callback) <#chain3mcfilter>`__
      -  `stopWatching(callback) <#chain3mcfilter>`__
      -  `get() <#chain3mcfilter>`__

   -  `contract(abiArray) <#chain3mccontract>`__
   -  `contract.myMethod() <#contract-methods>`__
   -  `contract.myEvent() <#contract-events>`__
   -  `contract.allEvents() <#contract-allevents>`__
   -  `encodeParams <#chain3encodeParams>`__
   -  `namereg <#chain3mcnamereg>`__
   -  `sendIBANTransaction <#chain3mcsendibantransaction>`__
   -  `iban <#chain3mciban>`__
   -  `fromAddress <#chain3mcibanfromaddress>`__
   -  `fromBban <#chain3mcibanfrombban>`__
   -  `createIndirect <#chain3mcibancreateindirect>`__
   -  `isValid <#chain3mcibanisvalid>`__
   -  `isDirect <#chain3mcibanisdirect>`__
   -  `isIndirect <#chain3mcibanisindirect>`__
   -  `checksum <#chain3mcibanchecksum>`__
   -  `institution <#chain3mcibaninstitution>`__
   -  `client <#chain3mcibanclient>`__
   -  `address <#chain3mcibanaddress>`__
   -  `toString <#chain3mcibantostring>`__

Usage
~~~~~



.. raw:: html

   <h4 id="chain3">

chain3.version.api

.. raw:: html

   </h4>

The ``chain3`` object provides all methods.

Example
'''''''

.. code:: js

    var Chain3 = require('chain3');
    // create an instance of chain3 using the HTTP provider.
    var chain3 = new Chain3(new Chain3.providers.HttpProvider("http://localhost:8545"));

--------------

.. raw:: html

   <h4 id="chain3versionapi">

chain3.version.api

.. raw:: html

   </h4>

.. code:: js

    chain3.version.api
    // or async
    chain3.version.getApi(callback(error, result){ ... })

Returns
'''''''

``String`` - The moac js api version.

Example
'''''''

.. code:: js

    var version = chain3.version.api;
    console.log(version); // "0.2.0"

--------------

.. raw:: html

   <h4 id="chain3versionnode">

chain3.version.node

.. raw:: html

   </h4>

::

    chain3.version.node
    // or async
    chain3.version.getClient(callback(error, result){ ... })

Returns
'''''''

``String`` - The client/node version.

Example
'''''''

.. code:: js

    var version = chain3.version.node;
    console.log(version); // "Moac/v0.1.0-develop/darwin-amd64/go1.9"

--------------

.. raw:: html

   <h4 id="chain3versionnetwork">

chain3.version.network

.. raw:: html

   </h4>

::

    chain3.version.network
    // or async
    chain3.version.getNetwork(callback(error, result){ ... })

Returns
'''''''

``String`` - The network protocol version.

Example
'''''''

.. code:: js

    var version = chain3.version.network;
    console.log(version); // 54

--------------

.. raw:: html

   <h4 id="chain3versionnetmoac">

chain3.version.moac

.. raw:: html

   </h4>

::

    chain3.version.moac
    // or async
    chain3.version.getMoac(callback(error, result){ ... })

Returns
'''''''

``String`` - The moac protocol version.

Example
'''''''

.. code:: js

    var version = chain3.version.moac;
    console.log(version); // 0x3f

--------------

.. raw:: html

   <h4 id="chain3isconnected">

chain3.isConnected

.. raw:: html

   </h4>

::

    chain3.isConnected()

Should be called to check if a connection to a node exists

Parameters
''''''''''

none

Returns
'''''''

``Boolean``

Example
'''''''

.. code:: js

    if(!chain3.isConnected()) {

       // show some dialog to ask the user to start a node

    } else {

       // start chain3 filters, calls, etc

    }

--------------

.. raw:: html

   <h4 id="chain3setprovider">

chain3.setProvider

.. raw:: html

   </h4>

::

    chain3.setProvider(provider)

Should be called to set provider.

Parameters
''''''''''

none

Returns
'''''''

``undefined``

Example
'''''''

.. code:: js

    chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545')); // 8545 for go/moac

--------------

.. raw:: html

   <h4 id="chain3currentprovider">

chain3.currentProvider

.. raw:: html

   </h4>

::

    chain3.currentProvider

Will contain the current provider, if one is set. This can be used to
check if moac etc. set already a provider.

Returns
'''''''

``Object`` - The provider set or ``null``;

Example
'''''''

.. code:: js

    // Check if moac etc. already set a provider
    if(!chain3.currentProvider)
        chain3.setProvider(new chain3.providers.HttpProvider("http://localhost:8545"));

--------------

.. raw:: html

   <h4 id="chain3currentreset">

chain3.reset

.. raw:: html

   </h4>

::

    chain3.reset(keepIsSyncing)

Should be called to reset state of chain3. Resets everything except
manager. Uninstalls all filters. Stops polling.

Parameters
''''''''''

1. ``Boolean`` - If ``true`` it will uninstall all filters, but will
   keep the `chain3.mc.isSyncing() <#chain3mcissyncing>`__ polls

Returns
'''''''

``undefined``

Example
'''''''

.. code:: js

    chain3.reset();

--------------

.. raw:: html

   <h4 id="chain3sha3">

chain3.sha3

.. raw:: html

   </h4>

::

    chain3.sha3(string [, callback])

Parameters
''''''''''

1. ``String`` - The string to hash using the SHA3 algorithm
2. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - The SHA3 of the given data.

Example
'''''''

.. code:: js

    var str = chain3.sha3("Some ASCII string to be hashed in MOAC");
    console.log(str); // "0xbfa24877cd68e6734710402a2af3e29cf18bd6d2f304aa528ffa3a32fa7652d2"

--------------

.. raw:: html

   <h4 id="chain3tohex">

chain3.toHex

.. raw:: html

   </h4>

::

    chain3.toHex(mixed);

Converts any value into HEX.

Parameters
''''''''''

1. ``String|Number|Object|Array|BigNumber`` - The value to parse to HEX.
   If its an object or array it will be ``JSON.stringify`` first. If its
   a BigNumber it will make it the HEX value of a number.

Returns
'''''''

``String`` - The hex string of ``mixed``.

Example
'''''''

.. code:: js

    var str = chain3.toHex("moac network");
    console.log(str); // '0x6d6f6163206e6574776f726b'

    console.log(chain3.toHex({moac: 'test'}));
    //'0x7b226d6f6163223a2274657374227d'

--------------

.. raw:: html

   <h4 id="chain3toascii">

chain3.toAscii

.. raw:: html

   </h4>

chain3.toAscii
^^^^^^^^^^^^^^

::

    chain3.toAscii(hexString);

Converts a HEX string into a ASCII string.

Parameters
''''''''''

1. ``String`` - A HEX string to be converted to ascii.

Returns
'''''''

``String`` - An ASCII string made from the given ``hexString``.

Example
'''''''

.. code:: js

    var str = chain3.toAscii("0x0x6d6f6163206e6574776f726b");
    console.log(str); // "moac network"

--------------

.. raw:: html

   <h4 id="chain3fromascii">

chain3.fromAscii

.. raw:: html

   </h4>

::

    chain3.fromAscii(string);

Converts any ASCII string to a HEX string.

Parameters
''''''''''

``String`` - An ASCII string to be converted to HEX.

Returns
'''''''

``String`` - The converted HEX string.

Example
'''''''

.. code:: js

    var str = chain3.fromAscii('moac network');
    console.log(str); // "0x6d6f6163206e6574776f726b"

--------------

.. raw:: html

   <h4 id="chain3tochecksumaddress">

chain3.toChecksumAddress

.. raw:: html

   </h4>

::

    chain3.toChecksumAddress(hexString);

Converts a string to the checksummed address equivalent.

Parameters
''''''''''

1. ``String`` - A string to be converted to a checksummed address.

Returns
'''''''

``String`` - A string containing the checksummed address.

Example
'''''''

.. code:: js

    var myAddress = chain3.toChecksumAddress('0xa0c876ec9f2d817c4304a727536f36363840c02c');
    console.log(myAddress); // '0xA0C876eC9F2d817c4304A727536f36363840c02c'

--------------

.. raw:: html

   <h4 id="chain3todecimal">

chain3.toDecimal

.. raw:: html

   </h4>

::

    chain3.toDecimal(hexString);

Converts a HEX string to its number representation.

Parameters
''''''''''

1. ``String`` - An HEX string to be converted to a number.

Returns
'''''''

``Number`` - The number representing the data ``hexString``.

Example
'''''''

.. code:: js

    var number = chain3.toDecimal('0x15');
    console.log(number); // 21

--------------

.. raw:: html

   <h4 id="chain3fromdecimal">

chain3.fromDecimal

.. raw:: html

   </h4>

::

    chain3.fromDecimal(number);

Converts a number or number string to its HEX representation.

Parameters
''''''''''

1. ``Number|String`` - A number to be converted to a HEX string.

Returns
'''''''

``String`` - The HEX string representing of the given ``number``.

Example
'''''''

.. code:: js

    var value = chain3.fromDecimal('21');
    console.log(value); // "0x15"

--------------

.. raw:: html

   <h4 id="chain3fromsha">

chain3.fromSha

.. raw:: html

   </h4>

::

    chain3.fromSha(number, unit)

Converts a number of sha into the following moac units:

-  ``ksha``/``femtomc``
-  ``msha``/``picomc``
-  ``gsha``/``nano``/``xiao``
-  ``micro``/``sand``
-  ``milli``
-  ``mc``
-  ``kmc``/``grand``
-  ``mmc``
-  ``gmc``
-  ``tmc``

Parameters
''''''''''

1. ``Number|String|BigNumber`` - A number or BigNumber instance.
2. ``String`` - One of the above moac units.

Returns
'''''''

``String|BigNumber`` - Either a number string, or a BigNumber instance,
depending on the given ``number`` parameter.

Example
'''''''

.. code:: js

    var value = chain3.fromSha('21000000000000', 'Xiao');
    console.log(value); // "21000"

--------------

.. raw:: html

   <h4 id="chain3tosha">

chain3.toSha

.. raw:: html

   </h4>

::

    chain3.toSha(number, unit)

Converts a moac unit into sha. Possible units are:

-  ``ksha``/``femtomc``
-  ``msha``/``picomc``
-  ``gsha``/``nano``/``xiao``
-  ``micro``/``sand``
-  ``milli``
-  ``mc``
-  ``kmc``/``grand``
-  ``mmc``
-  ``gmc``
-  ``tmc``

Parameters
''''''''''

1. ``Number|String|BigNumber`` - A number or BigNumber instance.
2. ``String`` - One of the above moac units.

Returns
'''''''

``String|BigNumber`` - Either a number string, or a BigNumber instance,
depending on the given ``number`` parameter.

Example
'''''''

.. code:: js

    var value = chain3.toSha('1', 'mc');
    console.log(value); // "1000000000000000000" = 1e18

--------------

.. raw:: html

   <h4 id="chain3tobignumber">

chain3.toBigNumber

.. raw:: html

   </h4>

::

    chain3.toBigNumber(numberOrHexString);

Converts a given number into a BigNumber instance.

See the `note on BigNumber <#a-note-on-big-numbers-in-javascript>`__.

Parameters
''''''''''

1. ``Number|String`` - A number, number string or HEX string of a
   number.

Returns
'''''''

``BigNumber`` - A BigNumber instance representing the given value.

Example
'''''''

.. code:: js

    var value = chain3.toBigNumber('200000000000000000000001');
    console.log(value); // instanceOf BigNumber, { [String: '2.00000000000000000000001e+23'] s: 1, e: 23, c: [ 2000000000, 1 ] }
    console.log(value.toNumber()); // 2.0000000000000002e+23
    console.log(value.toString(10)); // '200000000000000000000001'

--------------

.. raw:: html

   <h3 id="chain3net">

chain3.net

.. raw:: html

   </h3>

.. raw:: html

   <h4 id="chain3netlistening">

chain3.net.listening

.. raw:: html

   </h4>

::

    chain3.net.listening
    // or async
    chain3.net.getListening(callback(error, result){ ... })

This property is read only and says whether the node is actively
listening for network connections or not.

Returns
'''''''

``Boolean`` - ``true`` if the client is actively listening for network
connections, otherwise ``false``.

Example
'''''''

.. code:: js

    var listening = chain3.net.listening;
    console.log(listening); // true of false

--------------

.. raw:: html

   <h4 id="chain3netpeercount">

chain3.net.peerCount

.. raw:: html

   </h4>

::

    chain3.net.peerCount
    // or async
    chain3.net.getPeerCount(callback(error, result){ ... })

This property is read only and returns the number of connected peers.

Returns
'''''''

``Number`` - The number of peers currently connected to the client.

Example
'''''''

.. code:: js

    var peerCount = chain3.net.peerCount;
    console.log(peerCount); // 4

--------------

.. raw:: html

   <h3 id="chain3mc">

chain3.mc

.. raw:: html

   </h3>

Contains the MOAC blockchain related methods.

Example
'''''''

.. code:: js

    var mc = chain3.mc;

--------------

.. raw:: html

   <h4 id="chain3mcdefaultaccount">

chain3.mc.defaultAccount

.. raw:: html

   </h4>

::

    chain3.mc.defaultAccount

This default address is used for the following methods (optionally you
can overwrite it by specifying the ``from`` property):

-  `chain3.mc.sendTransaction() <#chain3mcsendtransaction>`__
-  `chain3.mc.call() <#chain3mccall>`__

Values
''''''

``String``, 20 Bytes - Any address you own, or where you have the
private key for.

*Default is* ``undefined``.

Returns
'''''''

``String``, 20 Bytes - The currently set default address.

Example
'''''''

.. code:: js

    var defaultAccount = chain3.mc.defaultAccount;
    console.log(defaultAccount); // 'undefined'

    // set the default block
    chain3.mc.defaultAccount = '0x8888f1f195afa192cfee860698584c030f4c9db1';
    console.log(chain3.mc.defaultAccount);//'0x8888f1f195afa192cfee860698584c030f4c9db1'

--------------

.. raw:: html

   <h4 id="chain3mcdefaultblock">

chain3.mc.defaultBlock

.. raw:: html

   </h4>

::

    chain3.mc.defaultBlock

This default block is used for the following methods (optionally you can
overwrite the defaultBlock by passing it as the last parameter):

-  `chain3.mc.getBalance() <#chain3mcgetbalance>`__
-  `chain3.mc.getCode() <#chain3mcgetcode>`__
-  `chain3.mc.getTransactionCount() <#chain3mcgettransactioncount>`__
-  `chain3.mc.getStorageAt() <#chain3mcgetstorageat>`__
-  `chain3.mc.call() <#chain3mccall>`__

Values
''''''

Default block parameters can be one of the following:

-  ``Number`` - a block number
-  ``String`` - ``"earliest"``, the genisis block
-  ``String`` - ``"latest"``, the latest block (current head of the
   blockchain)
-  ``String`` - ``"pending"``, the currently mined block (including
   pending transactions)

*Default is* ``latest``

Returns
'''''''

``Number|String`` - The default block number to use when querying a
state.

Example
'''''''

.. code:: js

    var defaultBlock = chain3.mc.defaultBlock;
    console.log(defaultBlock); // 'latest'

    // set the default block
    chain3.mc.defaultBlock = 12345;
    console.log(chain3.mc.defaultAccount);//'12345'

--------------

.. raw:: html

   <h4 id="chain3mcsyncing">

chain3.mc.syncing

.. raw:: html

   </h4>

::

    chain3.mc.syncing
    // or async
    chain3.mc.getSyncing(callback(error, result){ ... })

This property is read only and returns the either a sync object, when
the node is syncing or ``false``.

Returns
'''''''

``Object|Boolean`` - A sync object as follows, when the node is
currently syncing or ``false``: - ``startingBlock``: ``Number`` - The
block number where the sync started. - ``currentBlock``: ``Number`` -
The block number where at which block the node currently synced to
already. - ``highestBlock``: ``Number`` - The estimated block number to
sync to.

Example
'''''''

.. code:: js

    var sync = chain3.mc.syncing;
    console.log(sync);
    /*
    {
       startingBlock: 300,
       currentBlock: 312,
       highestBlock: 512
    }
    */

--------------

.. raw:: html

   <h4 id="chain3mcissyncing">

chain3.mc.isSyncing

.. raw:: html

   </h4>

::

    chain3.mc.isSyncing(callback);

This convenience function calls the ``callback`` everytime a sync
starts, updates and stops.

Returns
'''''''

``Object`` - a isSyncing object with the following methods:

-  ``syncing.addCallback()``: Adds another callback, which will be
   called when the node starts or stops syncing.
-  ``syncing.stopWatching()``: Stops the syncing callbacks.

Callback return value
'''''''''''''''''''''

-  ``Boolean`` - The callback will be fired with ``true`` when the
   syncing starts and with ``false`` when it stopped.
-  ``Object`` - While syncing it will return the syncing object:
-  ``startingBlock``: ``Number`` - The block number where the sync
   started.
-  ``currentBlock``: ``Number`` - The block number where at which block
   the node currently synced to already.
-  ``highestBlock``: ``Number`` - The estimated block number to sync to.

Example
'''''''

.. code:: js

    chain3.mc.isSyncing(function(error, sync){
        if(!error) {
            // stop all app activity
            if(sync === true) {
               // we use `true`, so it stops all filters, but not the chain3.mc.syncing polling
               chain3.reset(true);

            // show sync info
            } else if(sync) {
               console.log(sync.currentBlock);

            // re-gain app operation
            } else {
                // run your app init function...
            }
        }
    });

--------------

.. raw:: html

   <h4 id="chain3mccoinbase">

chain3.mc.coinbase

.. raw:: html

   </h4>

::

    chain3.mc.coinbase
    // or async
    chain3.mc.getCoinbase(callback(error, result){ ... })

This property is read only and returns the coinbase address were the
mining rewards go to.

Returns
'''''''

``String`` - The coinbase address of the client.

Example
'''''''

.. code:: js

    var coinbase = chain3.mc.coinbase;
    console.log(coinbase); // "0x407d73d8a49eeb85d32cf465507dd71d507100c1"

--------------

.. raw:: html

   <h4 id="chain3mcmining">

chain3.mc.mining

.. raw:: html

   </h4>

::

    chain3.mc.mining
    // or async
    chain3.mc.getMining(callback(error, result){ ... })

This property is read only and says whether the node is mining or not.

Returns
'''''''

``Boolean`` - ``true`` if the client is mining, otherwise ``false``.

Example
'''''''

.. code:: js

    var mining = chain3.mc.mining;
    console.log(mining); // true or false

--------------

.. raw:: html

   <h4 id="chain3mchashrate">

chain3.mc.hashrate

.. raw:: html

   </h4>

::

    chain3.mc.hashrate
    // or async
    chain3.mc.getHashrate(callback(error, result){ ... })

This property is read only and returns the number of hashes per second
that the node is mining with.

Returns
'''''''

``Number`` - number of hashes per second.

Example
'''''''

.. code:: js

    var hashrate = chain3.mc.hashrate;
    console.log(hashrate); // 493736

--------------

.. raw:: html

   <h4 id="chain3mcgasprice">

chain3.mc.gasPrice

.. raw:: html

   </h4>

::

    chain3.mc.gasPrice
    // or async
    chain3.mc.getGasPrice(callback(error, result){ ... })

This property is read only and returns the current gas price. The gas
price is determined by the x latest blocks median gas price.

Returns
'''''''

``BigNumber`` - A BigNumber instance of the current gas price in sha.

See the `note on BigNumber <#a-note-on-big-numbers-in-javascript>`__.

Example
'''''''

.. code:: js

    var gasPrice = chain3.mc.gasPrice;
    console.log(gasPrice.toString(10)); // "20000000000"

--------------

.. raw:: html

   <h4 id="chain3mcaccounts">

chain3.mc.accounts

.. raw:: html

   </h4>

::

    chain3.mc.accounts
    // or async
    chain3.mc.getAccounts(callback(error, result){ ... })

This property is read only and returns a list of accounts the node
controls.

Returns
'''''''

``Array`` - An array of addresses controlled by client.

Example
'''''''

.. code:: js

    var accounts = chain3.mc.accounts;
    console.log(accounts); // ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]

--------------

.. raw:: html

   <h4 id="chain3mcblocknumber">

chain3.mc.blockNumber

.. raw:: html

   </h4>

::

    chain3.mc.blockNumber
    // or async
    chain3.mc.getBlockNumber(callback(error, result){ ... })

This property is read only and returns the current block number.

Returns
'''''''

``Number`` - The number of the most recent block.

Example
'''''''

.. code:: js

    var number = chain3.mc.blockNumber;
    console.log(number); // 2744

--------------

.. raw:: html

   <h4 id="chain3mcgetbalance">

chain3.mc.getBalance

.. raw:: html

   </h4>

::

    chain3.mc.getBalance(addressHexString [, defaultBlock] [, callback])

Get the balance of an address at a given block.

Parameters
''''''''''

1. ``String`` - The address to get the balance of.
2. ``Number|String`` - (optional) If you pass this parameter it will not
   use the default block set with
   `chain3.mc.defaultBlock <#chain3mcdefaultblock>`__.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - A BigNumber instance of the current balance for the given
address in sha.

See the `note on BigNumber <#a-note-on-big-numbers-in-javascript>`__.

Example
'''''''

.. code:: js

    var balance = chain3.mc.getBalance("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
    console.log(balance); // instanceof BigNumber
    console.log(balance.toString(10)); // '1000000000000'
    console.log(balance.toNumber()); // 1000000000000

--------------

.. raw:: html

   <h4 id="chain3mcgetstorageat">

chain3.mc.getStorageAt

.. raw:: html

   </h4>

::

    chain3.mc.getStorageAt(addressHexString, position [, defaultBlock] [, callback])

Get the storage at a specific position of an address.

Parameters
''''''''''

1. ``String`` - The address to get the storage from.
2. ``Number`` - The index position of the storage.
3. ``Number|String`` - (optional) If you pass this parameter it will not
   use the default block set with
   `chain3.mc.defaultBlock <#chain3mcdefaultblock>`__.
4. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - The value in storage at the given position.

Example
'''''''

.. code:: js

    var state = chain3.mc.getStorageAt("0x407d73d8a49eeb85d32cf465507dd71d507100c1", 0);
    console.log(state); // "0x03"

--------------

.. raw:: html

   <h4 id="chain3mcgetcode">

chain3.mc.getCode

.. raw:: html

   </h4>

::

    chain3.mc.getCode(addressHexString [, defaultBlock] [, callback])

Get the code at a specific address. For a contract address, return the
binary code of the contract.

Parameters
''''''''''

1. ``String`` - The address to get the code from.
2. ``Number|String`` - (optional) If you pass this parameter it will not
   use the default block set with
   `chain3.mc.defaultBlock <#chain3mcdefaultblock>`__.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - The data at given address ``addressHexString``.

Example
'''''''

.. code:: js

    var code = chain3.mc.getCode("0x1d12aec505caa2b8513cdad9a13e9d4806a1b704");
    console.log(code); // "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"

--------------

.. raw:: html

   <h4 id="chain3mcgetblock">

chain3.mc.getBlock

.. raw:: html

   </h4>

::

     chain3.mc.getBlock(blockHashOrBlockNumber [, returnTransactionObjects] [, callback])

Returns a block matching the block number or block hash.

Parameters
''''''''''

1. ``String|Number`` - The block number or hash. Or the string
   ``"earliest"``, ``"latest"`` or ``"pending"`` as in the `default
   block parameter <#chain3mcdefaultblock>`__.
2. ``Boolean`` - (optional, default ``false``) If ``true``, the returned
   block will contain all transactions as objects, if ``false`` it will
   only contains the transaction hashes.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Object`` - The block object:

-  ``number``: ``Number`` - the block number. ``null`` when its pending
   block.
-  ``hash``: ``String``, 32 Bytes - hash of the block. ``null`` when its
   pending block.
-  ``parentHash``: ``String``, 32 Bytes - hash of the parent block.
-  ``nonce``: ``String``, 8 Bytes - hash of the generated proof-of-work.
   ``null`` when its pending block.
-  ``sha3Uncles``: ``String``, 32 Bytes - SHA3 of the uncles data in the
   block.
-  ``logsBloom``: ``String``, 256 Bytes - the bloom filter for the logs
   of the block. ``null`` when its pending block.
-  ``transactionsRoot``: ``String``, 32 Bytes - the root of the
   transaction trie of the block
-  ``stateRoot``: ``String``, 32 Bytes - the root of the final state
   trie of the block.
-  ``miner``: ``String``, 20 Bytes - the address of the beneficiary to
   whom the mining rewards were given.
-  ``difficulty``: ``BigNumber`` - integer of the difficulty for this
   block.
-  ``totalDifficulty``: ``BigNumber`` - integer of the total difficulty
   of the chain until this block.
-  ``extraData``: ``String`` - the "extra data" field of this block.
-  ``size``: ``Number`` - integer the size of this block in bytes.
-  ``gasLimit``: ``Number`` - the maximum gas allowed in this block.
-  ``gasUsed``: ``Number`` - the total used gas by all transactions in
   this block.
-  ``timestamp``: ``Number`` - the unix timestamp for when the block was
   collated.
-  ``transactions``: ``Array`` - Array of transaction objects, or 32
   Bytes transaction hashes depending on the last given parameter.
-  ``uncles``: ``Array`` - Array of uncle hashes.

Example
'''''''

.. code:: js

    var info = chain3.mc.getBlock(0);
    console.log(info);
    /*
    { difficulty: { [String: '100000000000'] s: 1, e: 11, c: [ 100000000000 ] },
      extraData: '0x31393639415250414e4554373354435049503039425443323031384d4f4143',
      gasLimit: 9000000,
      gasUsed: 0,
      hash: '0x6b9661646439fab926ffc9bccdf3abb572d5209ae59e3390abf76aee4e2d49cd',
      logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
      miner: '0x0000000000000000000000000000000000000000',
      mixHash: '0x0000000000000000000000000000000000000000000000000000000000000000',
      nonce: '0x0000000000000042',
      number: 0,
      parentHash: '0x0000000000000000000000000000000000000000000000000000000000000000',
      receiptsRoot: '0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421',
      sha3Uncles: '0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347',
      size: 540,
      stateRoot: '0xe221c9e4ad19514d7ce3e6b0bec3ad7f6cc293336e59c301cda293cfbda83df6',
      timestamp: 0,
      totalDifficulty: { [String: '100000000000'] s: 1, e: 11, c: [ 100000000000 ] },
      transactions: [],
      transactionsRoot: '0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421',
      uncles: [] }
    */

--------------

.. raw:: html

   <h4 id="chain3mcgetblocktransactioncount">

chain3.mc.getBlockTransactionCount

.. raw:: html

   </h4>

::

    chain3.mc.getBlockTransactionCount(hashStringOrBlockNumber [, callback])

Returns the number of transaction in a given block.

Parameters
''''''''''

1. ``String|Number`` - The block number or hash. Or the string
   ``"earliest"``, ``"latest"`` or ``"pending"`` as in the `default
   block parameter <#chain3mcdefaultblock>`__.
2. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Number`` - The number of transactions in the given block.

Example
'''''''

.. code:: js

    var number = chain3.mc.getBlockTransactionCount("0xddfb8508bff841242099e640efe59f5e5252be1a60fa701d333e1a8bfdee6263");
    console.log(number); // 1

--------------

.. raw:: html

   <h4 id="chain3mcgetuncle">

chain3.mc.getUncle

.. raw:: html

   </h4>

::

    chain3.mc.getUncle(blockHashStringOrNumber, uncleNumber [, returnTransactionObjects] [, callback])

Returns a blocks uncle by a given uncle index position.

Parameters
''''''''''

1. ``String|Number`` - The block number or hash. Or the string
   ``"earliest"``, ``"latest"`` or ``"pending"`` as in the `default
   block parameter <#chain3mcdefaultblock>`__.
2. ``Number`` - The index position of the uncle.
3. ``Boolean`` - (optional, default ``false``) If ``true``, the returned
   block will contain all transactions as objects, if ``false`` it will
   only contains the transaction hashes.
4. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Object`` - the returned uncle. For a return value see
`chain3.mc.getBlock() <#chain3mcgetblock>`__.

**Note**: An uncle doesn't contain individual transactions.

Example
'''''''

.. code:: js

    var uncle = chain3.mc.getUncle(500, 0);
    console.log(uncle); // see chain3.mc.getBlock

--------------

.. raw:: html

   <h4 id="chain3mcgettransaction">

chain3.mc.getTransaction

.. raw:: html

   </h4>

::

    chain3.mc.getTransaction(transactionHash [, callback])

Returns a transaction matching the given transaction hash.

Parameters
''''''''''

1. ``String`` - The transaction hash.
2. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Object`` - A transaction object its hash ``transactionHash``:

-  ``hash``: ``String``, 32 Bytes - hash of the transaction.
-  ``nonce``: ``Number`` - the number of transactions made by the sender
   prior to this one.
-  ``blockHash``: ``String``, 32 Bytes - hash of the block where this
   transaction was in. ``null`` when its pending.
-  ``blockNumber``: ``Number`` - block number where this transaction was
   in. ``null`` when its pending.
-  ``transactionIndex``: ``Number`` - integer of the transactions index
   position in the block. ``null`` when its pending.
-  ``from``: ``String``, 20 Bytes - address of the sender.
-  ``to``: ``String``, 20 Bytes - address of the receiver. ``null`` when
   its a contract creation transaction.
-  ``value``: ``BigNumber`` - value transferred in Sha.
-  ``gasPrice``: ``BigNumber`` - gas price provided by the sender in
   Sha.
-  ``gas``: ``Number`` - gas provided by the sender.
-  ``input``: ``String`` - the data sent along with the transaction.

Example
'''''''

.. code:: js

    var txhash = "0x687751dd47684f4b5df263ae4ec39f54f057d0e2a1dde56f9d52766849c9c7fe";

    var transaction = chain3.mc.getTransaction(txhash);
    console.log(transaction);
    /*
    { blockHash: '0x716c602d49055b4e24fbe7da952c1ad81820ad0401f3cb3ce12e832fbcc368f5',
      blockNumber: 57259,
      from: '0x7cfd775c7a97aa632846eff35dcf9dbcf502d0f3',
      gas: 1000,
      gasPrice: { [String: '200000000000'] s: 1, e: 11, c: [ 200000000000 ] },
      hash: '0xf1c1771204431c1c584e793b49d41586a923c370be93673aac42d66252bc8d0a',
      input: '0x',
      nonce: 840,
      syscnt: '0x0',
      to: '0x3435410589ebd06b74079a1e141759d8502aeb8b',
      transactionIndex: 689,
      value: { [String: '1000000000'] s: 1, e: 9, c: [ 1000000000 ] },
      v: '0xea',
      r: '0x77aa72d0900e436b60f8be377e43dead3567241a85fbeb5517283e2dc8aca2b4',
      s: '0x24477ec130fc4101289b37e6107acfe64026c898994273110faa8f19b8ea6985',
      shardingFlag: '0x0' }
    */

--------------

.. raw:: html

   <h4 id="chain3mcgettransactionfromBlock">

chain3.mc.getTransactionFromBlock

.. raw:: html

   </h4>

::

    getTransactionFromBlock(hashStringOrNumber, indexNumber [, callback])

Returns a transaction based on a block hash or number and the
transactions index position.

Parameters
''''''''''

1. ``String`` - A block number or hash. Or the string ``"earliest"``,
   ``"latest"`` or ``"pending"`` as in the `default block
   parameter <#chain3mcdefaultblock>`__.
2. ``Number`` - The transactions index position.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Object`` - A transaction object, see
`chain3.mc.getTransaction <#chain3mcgettransaction>`__:

Example
'''''''

.. code:: js

    var transaction = chain3.mc.getTransactionFromBlock(921, 2);
    console.log(transaction); // see chain3.mc.getTransaction

--------------

.. raw:: html

   <h4 id="chain3mcgettransactionreceipt">

chain3.mc.getTransactionReceipt

.. raw:: html

   </h4>

::

    chain3.mc.getTransactionReceipt(hashString [, callback])

Returns the receipt of a transaction by transaction hash.

**Note** That the receipt is not available for pending transactions.

Parameters
''''''''''

1. ``String`` - The transaction hash.
2. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Object`` - A transaction receipt object, or ``null`` when no receipt
was found:

-  ``blockHash``: ``String``, 32 Bytes - hash of the block where this
   transaction was in.
-  ``blockNumber``: ``Number`` - block number where this transaction was
   in.
-  ``transactionHash``: ``String``, 32 Bytes - hash of the transaction.
-  ``transactionIndex``: ``Number`` - integer of the transactions index
   position in the block.
-  ``from``: ``String``, 20 Bytes - address of the sender.
-  ``to``: ``String``, 20 Bytes - address of the receiver. ``null`` when
   its a contract creation transaction.
-  ``cumulativeGasUsed``: ``Number`` - The total amount of gas used when
   this transaction was executed in the block.
-  ``gasUsed``: ``Number`` - The amount of gas used by this specific
   transaction alone.
-  ``contractAddress``: ``String`` - 20 Bytes - The contract address
   created, if the transaction was a contract creation, otherwise
   ``null``.
-  ``logs``: ``Array`` - Array of log objects, which this transaction
   generated.

Example
'''''''

.. code:: js

    var receipt = chain3.mc.getTransactionReceipt('0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b');
    console.log(receipt);
    {
      "transactionHash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
      "transactionIndex": 0,
      "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
      "blockNumber": 3,
      "contractAddress": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",
      "cumulativeGasUsed": 314159,
      "gasUsed": 30234,
      "logs": [{
             // logs as returned by getFilterLogs, etc.
         }, ...]
    }

--------------

.. raw:: html

   <h4 id="chain3mcgettransactioncount">

chain3.mc.getTransactionCount

.. raw:: html

   </h4>

::

    chain3.mc.getTransactionCount(addressHexString [, defaultBlock] [, callback])

Get the numbers of transactions sent from this address.

Parameters
''''''''''

1. ``String`` - The address to get the numbers of transactions from.
2. ``Number|String`` - (optional) If you pass this parameter it will not
   use the default block set with
   `chain3.mc.defaultBlock <#chain3mcdefaultblock>`__.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Number`` - The number of transactions sent from the given address.

Example
'''''''

.. code:: js

    var number = chain3.mc.getTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
    console.log(number); // 1

--------------

.. raw:: html

   <h4 id="chain3mcsendtransaction">

chain3.mc.sendTransaction

.. raw:: html

   </h4>

::

    chain3.mc.sendTransaction(transactionObject [, callback])

Sends a transaction to the network.

Parameters
''''''''''

1. ``Object`` - The transaction object to send:

-  ``from``: ``String`` - The address for the sending account. Uses the
   `chain3.mc.defaultAccount <#chain3mcdefaultaccount>`__ property, if
   not specified.
-  ``to``: ``String`` - (optional) The destination address of the
   message, can be an account address or a contract address, left
   undefined for a contract-creation transaction.
-  ``value``: ``Number|String|BigNumber`` - (optional) The value
   transferred for the transaction in Sha, also the endowment if it's a
   contract-creation transaction.
-  ``gas``: ``Number|String|BigNumber`` - (optional, default:
   To-Be-Determined) The amount of gas to use for the transaction
   (unused gas is refunded).
-  ``gasPrice``: ``Number|String|BigNumber`` - (optional, default:
   To-Be-Determined) The price of gas for this transaction in sha,
   defaults to the mean network gas price.
-  ``data``: ``String`` - (optional) Either a byte string containing the
   associated data of the message, or in the case of a contract-creation
   transaction, the initialisation code.
-  ``nonce``: ``Number`` - (optional) Integer of a nonce. This allows to
   overwrite your own pending transactions that use the same nonce.
-  ``shardingFlag``: ``Number|String`` - (optional) Set to 1 for
   micro/subchain direct contract-call, 0 or undefine for global
   contract-creation transaction.

2. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - The 32 Bytes transaction hash as HEX string.

If the transaction was a contract creation use
`chain3.mc.getTransactionReceipt() <#chain3mcgettransactionreceipt>`__
to get the contract address, after the transaction was mined.

Example
'''''''

.. code:: js


    // compiled solidity source code using https://chriseth.github.io/cpp-ethereum/
    var code = "603d80600c6000396000f3007c01000000000000000000000000000000000000000000000000000000006000350463c6888fa18114602d57005b600760043502
    8060005260206000f3";

    chain3.mc.sendTransaction({data: code}, function(err, address) {
      if (!err)
        console.log(address); // "0x7f9fade1c0d57a7af66ab4ead7c2eb7b11a91385"
    });

--------------

.. raw:: html

   <h4 id="chain3mccall">

chain3.mc.call

.. raw:: html

   </h4>

::

    chain3.mc.call(callObject [, defaultBlock] [, callback])

Executes a message call transaction, which is directly executed in the
VM of the node, but never mined into the blockchain.

Parameters
''''''''''

1. ``Object`` - A transaction object see
   `chain3.mc.sendTransaction <#chain3mcsendtransaction>`__, with the
   difference that for calls the ``from`` property is optional as well.
2. ``Number|String`` - (optional) If you pass this parameter it will not
   use the default block set with
   `chain3.mc.defaultBlock <#chain3mcdefaultblock>`__.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - The returned data of the call, e.g. a codes functions
return value.

Example
'''''''

.. code:: js

    var result = chain3.mc.call({
        to: "0xc4abd0339eb8d57087278718986382264244252f",
        data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
    });
    console.log(result); // "0x0000000000000000000000000000000000000000000000000000000000000015"

--------------

.. raw:: html

   <h4 id="chain3mcestimategas">

chain3.mc.estimateGas

.. raw:: html

   </h4>

::

    chain3.mc.estimateGas(callObject [, defaultBlock] [, callback])

Executes a message call or transaction, which is directly executed in
the VM of the node, but never mined into the blockchain and returns the
amount of the gas used.

Parameters
''''''''''

See `chain3.mc.sendTransaction <#chain3mcsendtransaction>`__, expect
that all properties are optional.

Returns
'''''''

``Number`` - the used gas for the simulated call/transaction.

Example
'''''''

.. code:: js

    var result = chain3.mc.estimateGas({
        to: "0xc4abd0339eb8d57087278718986382264244252f",
        data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
    });
    console.log(result); // "1465"

--------------

.. raw:: html

   <h4 id="chain3mcfilter">

chain3.mc.filter

.. raw:: html

   </h4>

.. code:: js

    // can be 'latest' or 'pending'
    var filter = chain3.mc.filter(filterString);
    // OR object are log filter options
    var filter = chain3.mc.filter(options);

    // watch for changes
    filter.watch(function(error, result){
      if (!error)
        console.log(result);
    });

    // Additionally you can start watching right away, by passing a callback:
    chain3.mc.filter(options, function(error, result){
      if (!error)
        console.log(result);
    });

Parameters
''''''''''

1. ``String|Object`` - The string ``"latest"`` or ``"pending"`` to watch
   for changes in the latest block or pending transactions respectively.
   Or a filter options object as follows:

-  ``fromBlock``: ``Number|String`` - The number of the earliest block
   (``latest`` may be given to mean the most recent and ``pending``
   currently mining, block). By default ``latest``.
-  ``toBlock``: ``Number|String`` - The number of the latest block
   (``latest`` may be given to mean the most recent and ``pending``
   currently mining, block). By default ``latest``.
-  ``address``: ``String`` - An address or a list of addresses to only
   get logs from particular account(s).
-  ``topics``: ``Array of Strings`` - An array of values which must each
   appear in the log entries. The order is important, if you want to
   leave topics out use ``null``, e.g. ``[null, '0x00...']``. You can
   also pass another array for each topic with options for that topic
   e.g. ``[null, ['option1', 'option2']]``

Returns
'''''''

``Object`` - A filter object with the following methods:

-  ``filter.get(callback)``: Returns all of the log entries that fit the
   filter.
-  ``filter.watch(callback)``: Watches for state changes that fit the
   filter and calls the callback. See `this note <#using-callbacks>`__
   for details.
-  ``filter.stopWatching()``: Stops the watch and uninstalls the filter
   in the node. Should always be called once it is done.

Watch callback return value
'''''''''''''''''''''''''''

-  ``String`` - When using the ``"latest"`` parameter, it returns the
   block hash of the last incoming block.
-  ``String`` - When using the ``"pending"`` parameter, it returns a
   transaction hash of the last add pending transaction.
-  ``Object`` - When using manual filter options, it returns a log
   object as follows:

   -  ``logIndex``: ``Number`` - integer of the log index position in
      the block. ``null`` when its pending log.
   -  ``transactionIndex``: ``Number`` - integer of the transactions
      index position log was created from. ``null`` when its pending
      log.
   -  ``transactionHash``: ``String``, 32 Bytes - hash of the
      transactions this log was created from. ``null`` when its pending
      log.
   -  ``blockHash``: ``String``, 32 Bytes - hash of the block where this
      log was in. ``null`` when its pending. ``null`` when its pending
      log.
   -  ``blockNumber``: ``Number`` - the block number where this log was
      in. ``null`` when its pending. ``null`` when its pending log.
   -  ``address``: ``String``, 32 Bytes - address from which this log
      originated.
   -  ``data``: ``String`` - contains one or more 32 Bytes non-indexed
      arguments of the log.
   -  ``topics``: ``Array of Strings`` - Array of 0 to 4 32 Bytes
      ``DATA`` of indexed log arguments. (In *solidity*: The first topic
      is the *hash* of the signature of the event (e.g.
      ``Deposit(address,bytes32,uint256)``), except you declared the
      event with the ``anonymous`` specifier.)

**Note** For event filter return values see `Contract
Events <#contract-events>`__

Example
'''''''

.. code:: js

    var filter = chain3.mc.filter('pending');

    filter.watch(function (error, log) {
      console.log(log); //  {"address":"0x0000000000000000000000000000000000000000", "data":"0x0000000000000000000000000000000000000000000000000000000000000000", ...}
    });

    // get all past logs again.
    var myResults = filter.get(function(error, logs){ ... });

    ...

    // stops and uninstalls the filter
    filter.stopWatching();

--------------

.. raw:: html

   <h4 id="chain3mccontract">

chain3.mc.contract

.. raw:: html

   </h4>

::

    chain3.mc.contract(abiArray)

Creates a contract object for a solidity contract, which can be used to
initiate contracts on an address. You can read more about events
`here <https://github.com/MOACChain/wiki/wiki/MOAC-Contract-ABI#example-javascript-usage>`__.

Parameters
''''''''''

1. ``Array`` - ABI array with descriptions of functions and events of
   the contract.

Returns
'''''''

``Object`` - A contract object, which can be initiated as follows:

.. code:: js

    var MyContract = chain3.mc.contract(abiArray);

    // instantiate by address
    var contractInstance = MyContract.at([address]);

    // deploy new contract
    var contractInstance = MyContract.new([contructorParam1] [, contructorParam2], {data: '0x12345...', from: myAccount, gas: 1000000});

    // Get the data to deploy the contract manually
    var contractData = MyContract.new.getData([contructorParam1] [, contructorParam2], {data: '0x12345...'});
    // contractData = '0x12345643213456000000000023434234'

And then you can either initiate an existing contract on an address, or
deploy the contract using the compiled byte code:

.. code:: js

    // Instantiate from an existing address:
    var myContractInstance = MyContract.at(myContractAddress);


    // Or deploy a new contract:

    // Deploy the contract asyncronous:
    var myContractReturned = MyContract.new(param1, param2, {
       data: myContractCode,
       gas: 300000,
       from: mySenderAddress}, function(err, myContract){
        if(!err) {
           // NOTE: The callback will fire twice!
           // Once the contract has the transactionHash property set and once its deployed on an address.

           // e.g. check tx hash on the first call (transaction send)
           if(!myContract.address) {
               console.log(myContract.transactionHash) // The hash of the transaction, which deploys the contract

           // check address on the second call (contract deployed)
           } else {
               console.log(myContract.address) // the contract address
           }

           // Note that the returned "myContractReturned" === "myContract",
           // so the returned "myContractReturned" object will also get the address set.
        }
      });

    // Deploy contract syncronous: The address will be added as soon as the contract is mined.
    // Additionally you can watch the transaction by using the "transactionHash" property
    var myContractInstance = MyContract.new(param1, param2, {data: myContractCode, gas: 300000, from: mySenderAddress});
    myContractInstance.transactionHash // The hash of the transaction, which created the contract
    myContractInstance.address // undefined at start, but will be auto-filled later

**Note** When you deploy a new contract, you should check for the next
12 blocks or so if the contract code is still at the address (using
`chain3.mc.getCode() <#chain3mcgetcode>`__), to make sure a fork didn't
change that.

Example
'''''''

.. code:: js

    // contract abi
    var abi = [{
         name: 'myConstantMethod',
         type: 'function',
         constant: true,
         inputs: [{ name: 'a', type: 'string' }],
         outputs: [{name: 'd', type: 'string' }]
    }, {
         name: 'myStateChangingMethod',
         type: 'function',
         constant: false,
         inputs: [{ name: 'a', type: 'string' }, { name: 'b', type: 'int' }],
         outputs: []
    }, {
         name: 'myEvent',
         type: 'event',
         inputs: [{name: 'a', type: 'int', indexed: true},{name: 'b', type: 'bool', indexed: false]
    }];

    // creation of contract object
    var MyContract = chain3.mc.contract(abi);

    // initiate contract for an address
    var myContractInstance = MyContract.at('0xc4abd0339eb8d57087278718986382264244252f');

    // call constant function
    var result = myContractInstance.myConstantMethod('myParam');
    console.log(result) // '0x25434534534'

    // send a transaction to a function
    myContractInstance.myStateChangingMethod('someParam1', 23, {value: 200, gas: 2000});

    // short hand style
    chain3.mc.contract(abi).at(address).myAwesomeMethod(...);

    // create filter
    var filter = myContractInstance.myEvent({a: 5}, function (error, result) {
      if (!error)
        console.log(result);
        /*
        {
            address: '0x8718986382264244252fc4abd0339eb8d5708727',
            topics: "0x12345678901234567890123456789012", "0x0000000000000000000000000000000000000000000000000000000000000005",
            data: "0x0000000000000000000000000000000000000000000000000000000000000001",
            ...
        }
        */
    });

--------------

Contract Methods
^^^^^^^^^^^^^^^^

.. code:: js

    // Automatically determines the use of call or sendTransaction based on the method type
    myContractInstance.myMethod(param1 [, param2, ...] [, transactionObject] [, callback]);

    // Explicitly calling this method
    myContractInstance.myMethod.call(param1 [, param2, ...] [, transactionObject] [, callback]);

    // Explicitly sending a transaction to this method
    myContractInstance.myMethod.sendTransaction(param1 [, param2, ...] [, transactionObject] [, callback]);

    // Explicitly sending a transaction to this method
    myContractInstance.myMethod.sendTransaction(param1 [, param2, ...] [, transactionObject] [, callback]);

    // Get the call data, so you can call the contract through some other means
    var myCallData = myContractInstance.myMethod.getData(param1 [, param2, ...]);
    // myCallData = '0x45ff3ff6000000000004545345345345..'

The contract object exposes the contracts methods, which can be called
using parameters and a transaction object.

Parameters
''''''''''

-  ``String|Number`` - (optional) Zero or more parameters of the
   function.
-  ``Object`` - (optional) The (previous) last parameter can be a
   transaction object, see
   `chain3.mc.sendTransaction <#chain3mcsendtransaction>`__ parameter 1
   for more.
-  ``Function`` - (optional) If you pass a callback as the last
   parameter the HTTP request is made asynchronous. See `this
   note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - If its a call the result data, if its a send transaction a
created contract address, or the transaction hash, see
`chain3.mc.sendTransaction <#chain3mcsendtransaction>`__ for details.

Example
'''''''

.. code:: js

    // creation of contract object
    var MyContract = chain3.mc.contract(abi);

    // initiate contract for an address
    var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

    var result = myContractInstance.myConstantMethod('myParam');
    console.log(result) // '0x25434534534'

    myContractInstance.myStateChangingMethod('someParam1', 23, {value: 200, gas: 2000}, function(err, result){ ... });

--------------

Contract Events
^^^^^^^^^^^^^^^

.. code:: js

    var event = myContractInstance.MyEvent({valueA: 23} [, additionalFilterObject])

    // watch for changes
    event.watch(function(error, result){
      if (!error)
        console.log(result);
    });

    // Or pass a callback to start watching immediately
    var event = myContractInstance.MyEvent([{valueA: 23}] [, additionalFilterObject] , function(error, result){
      if (!error)
        console.log(result);
    });

You can use events like `filters <#chain3mcfilter>`__ and they have the
same methods, but you pass different objects to create the event filter.

Parameters
''''''''''

1. ``Object`` - Indexed return values you want to filter the logs by,
   e.g. ``{'valueA': 1, 'valueB': [myFirstAddress, mySecondAddress]}``.
   By default all filter values are set to ``null``. It means, that they
   will match any event of given type sent from this contract.
2. ``Object`` - Additional filter options, see
   `filters <#chain3mcfilter>`__ parameter 1 for more. By default
   filterObject has field 'address' set to address of the contract. Also
   first topic is the signature of event.
3. ``Function`` - (optional) If you pass a callback as the last
   parameter it will immediately start watching and you don't need to
   call ``myEvent.watch(function(){})``. See `this
   note <#using-callbacks>`__ for details.

Callback return
'''''''''''''''

``Object`` - An event object as follows:

-  ``args``: ``Object`` - The arguments coming from the event.
-  ``event``: ``String`` - The event name.
-  ``logIndex``: ``Number`` - integer of the log index position in the
   block.
-  ``transactionIndex``: ``Number`` - integer of the transactions index
   position log was created from.
-  ``transactionHash``: ``String``, 32 Bytes - hash of the transactions
   this log was created from.
-  ``address``: ``String``, 32 Bytes - address from which this log
   originated.
-  ``blockHash``: ``String``, 32 Bytes - hash of the block where this
   log was in. ``null`` when its pending.
-  ``blockNumber``: ``Number`` - the block number where this log was in.
   ``null`` when its pending.

Example
'''''''

.. code:: js

    var MyContract = chain3.mc.contract(abi);
    var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

    // watch for an event with {some: 'args'}
    var myEvent = myContractInstance.MyEvent({some: 'args'}, {fromBlock: 0, toBlock: 'latest'});
    myEvent.watch(function(error, result){
       ...
    });

    // would get all past logs again.
    var myResults = myEvent.get(function(error, logs){ ... });

    ...

    // would stop and uninstall the filter
    myEvent.stopWatching();

--------------

Contract allEvents
^^^^^^^^^^^^^^^^^^

.. code:: js

    var events = myContractInstance.allEvents([additionalFilterObject]);

    // watch for changes
    events.watch(function(error, event){
      if (!error)
        console.log(event);
    });

    // Or pass a callback to start watching immediately
    var events = myContractInstance.allEvents([additionalFilterObject,] function(error, log){
      if (!error)
        console.log(log);
    });

Will call the callback for all events which are created by this
contract.

Parameters
''''''''''

1. ``Object`` - Additional filter options, see
   `filters <#chain3mcfilter>`__ parameter 1 for more. By default
   filterObject has field 'address' set to address of the contract. Also
   first topic is the signature of event.
2. ``Function`` - (optional) If you pass a callback as the last
   parameter it will immediately start watching and you don't need to
   call ``myEvent.watch(function(){})``. See `this
   note <#using-callbacks>`__ for details.

Callback return
'''''''''''''''

``Object`` - See `Contract Events <#contract-events>`__ for more.

Example
'''''''

.. code:: js

    var MyContract = chain3.mc.contract(abi);
    var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

    // watch for an event with {some: 'args'}
    var events = myContractInstance.allEvents({fromBlock: 0, toBlock: 'latest'});
    events.watch(function(error, result){
       ...
    });

    // would get all past logs again.
    events.get(function(error, logs){ ... });

    ...

    // would stop and uninstall the filter
    myEvent.stopWatching();

--------------

.. raw:: html

   <h4 id="chain3encodeParams">

chain3.encodeParams

.. raw:: html

   </h4>

::

    chain3.encodeParams

Encode a list of parameters array into HEX codes. ##### Parameters

-  ``types`` - list of types of params
-  ``params`` - list of values of params

Example
'''''''

.. code:: js

    var types = ['int','string'];
    var args = [100, '4000'];

    var dataHex = '0x' + chain3.encodeParams(types, args);
    console.log("encoded params:", dataHex);

    // outputs
    encoded params:0x0000000000000000000000000000000000000000000000000000000000000064000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000043430303000000000000000000000000000000000000000000000000000000000

--------------

.. raw:: html

   <h4 id="chain3mcnamereg">

chain3.mc.namereg

.. raw:: html

   </h4>

::

    chain3.mc.namereg

Returns GlobalRegistrar object.

Usage
'''''

see
`namereg <https://github.com/MOACChain/chain3.js/blob/master/example/namereg.html>`__
example

--------------

.. raw:: html

   <h4 id="chain3mcsendibantransaction">

chain3.mc.sendIBANTransaction

.. raw:: html

   </h4>

.. code:: js

    var txHash = chain3.mc.sendIBANTransaction('0x00c5496aee77c1ba1f0854206a26dda82a81d6d8', 'XE66MOACXREGGAVOFYORK', 0x100);

Sends IBAN transaction from user account to destination IBAN address.

Parameters
''''''''''

-  ``string`` - address from which we want to send transaction
-  ``string`` - IBAN address to which we want to send transaction
-  ``value`` - value that we want to send in IBAN transaction

--------------

.. raw:: html

   <h4 id="chain3mciban">

chain3.mc.iban

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE81ETHXREGGAVOFYORK");

--------------

.. raw:: html

   <h4 id="chain3mcibanfromaddress">

chain3.mc.iban.fromAddress

.. raw:: html

   </h4>

.. code:: js

    var i = chain3.mc.iban.fromAddress('0xd814f2ac2c4ca49b33066582e4e97ebae02f2ab9');
    console.log(i.toString()); // 'XE72P8O19KRSWXUGDY294PZ66T4ZGF89INT

--------------

.. raw:: html

   <h4 id="chain3mcibanfrombban">

chain3.mc.iban.fromBban

.. raw:: html

   </h4>

.. code:: js

    var i = chain3.mc.iban.fromBban('XE66MOACXREGGAVOFYORK');
    console.log(i.toString()); // "XE71XE66MOACXREGGAVOFYORK"

--------------

.. raw:: html

   <h4 id="chain3mcibancreateindirect">

chain3.mc.iban.createIndirect

.. raw:: html

   </h4>

.. code:: js

    var i = chain3.mc.iban.createIndirect({
      institution: "XREG",
      identifier: "GAVOFYORK"
    });
    console.log(i.toString()); // "XE66MOACXREGGAVOFYORK"

--------------

.. raw:: html

   <h4 id="chain3mcibanisvalid">

chain3.mc.iban.isValid

.. raw:: html

   </h4>

.. code:: js

    var valid = chain3.mc.iban.isValid("XE66MOACXREGGAVOFYORK");
    console.log(valid); // true

    var valid2 = chain3.mc.iban.isValid("XE76MOACXREGGAVOFYORK");
    console.log(valid2); // false, cause checksum is incorrect

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var valid3 = i.isValid();
    console.log(valid3); // true

--------------

.. raw:: html

   <h4 id="chain3mcibanisdirect">

chain3.mc.iban.isDirect

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var direct = i.isDirect();
    console.log(direct); // false

--------------

.. raw:: html

   <h4 id="chain3mcibanisindirect">

chain3.mc.iban.isIndirect

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var indirect = i.isIndirect();
    console.log(indirect); // true

--------------

.. raw:: html

   <h4 id="chain3mcibanchecksum">

chain3.mc.iban.checksum

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var checksum = i.checksum();
    console.log(checksum); // "66"

--------------

.. raw:: html

   <h4 id="chain3mcibaninstitution">

chain3.mc.iban.institution

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var institution = i.institution();
    console.log(institution); // 'XREG'

--------------

.. raw:: html

   <h4 id="chain3mcibanclient">

chain3.mc.iban.client

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var client = i.client();
    console.log(client); // 'GAVOFYORK'

--------------

.. raw:: html

   <h4 id="chain3mcibanaddress">

chain3.mc.iban.address

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban('XE72P8O19KRSWXUGDY294PZ66T4ZGF89INT');
    var address = i.address();
    console.log(address); // 'd814f2ac2c4ca49b33066582e4e97ebae02f2ab9'

--------------

.. raw:: html

   <h4 id="chain3mcibantostring">

chain3.mc.iban.toString

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban('XE72P8O19KRSWXUGDY294PZ66T4ZGF89INT');
    console.log(i.toString()); // 'XE72P8O19KRSWXUGDY294PZ66T4ZGF89INT'
