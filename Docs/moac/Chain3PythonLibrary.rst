Chain3 Python Library
~~~~~~~~~~~~~~~~~~~~~


Installation
------------

Chain3 python package supports python3.5.3 and up.

Chain3 python can be installed (preferably in a virtualenv) using pip as
follows:

::

    $ pip install chain3

If you run into problems during installation, you might have a broken
environment, such as you are on an unsupported version of Python or
another package might be installed that has a name or version conflict
Often, the best way to guarantee a correct environment is with
virtualenv, like:

::

    $ which pip || curl https://bootstrap.pypa.io/get-pip.py | python

    $ which virtualenv || pip install --upgrade virtualenv

    $ sudo pip install virtualenv

    $ virtualenv -p python3 ~/.venv-py3

    $ source ~/.venv-py3/bin/activate

    $ pip install --upgrade pip setuptools

    $ pip install --upgrade chain3

To use the virtualenv next time, run the following command:

::

    $ source ~/.venv-py3/bin/activate

Using Chain3
------------

.. code:: python

    from chain3 import Chain3
    chain3 = Chain3(Chain3.HTTPProvider("http://127.0.0.1:8545", request_kwargs={'timeout': 30}))

Chain3 API
----------

1. chain3.version.network：
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the current network id. For MOAC, mainnet network id = 99,
testnet network id = 101.

.. code:: python

    print('network id:'+ str(chain3.version.network))

2. chain3.HTTPProvider:
~~~~~~~~~~~~~~~~~~~~~~~

Convenience API to access chain3.providers.rpc.HTTPProvider

.. code:: python

    Chain3(Chain3.HTTPProvider("http://127.0.0.1:8545", request_kwargs={'timeout': 30}))

3. chain3.sha3(string,options)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

string：The string to hash using the SHA3 algorithm.
options：optional，need set to hex for HEX string.

.. code:: python

    hash = chain3.sha3("the string to be hashed");
    print(hash);
    hashOfHash = chain3.sha3(hash,{encoding:'hex'});
    print(hashOfHash);

4. chain3.net.peerCount:
~~~~~~~~~~~~~~~~~~~~~~~~

This property is read only and returns the number of connected peers.

.. code:: python

    peerCount = chain3.net.peerCount;
    print(peerCount);

5. chain3.net.listening:
~~~~~~~~~~~~~~~~~~~~~~~~

This property is read only and says whether the node is actively
listening for network connections or not.

.. code:: python

    listenState = chain3.net.listening;
    print(listenState);

6. chain3.mc.coinbase:
~~~~~~~~~~~~~~~~~~~~~~

Returns the current Coinbase address.

.. code:: python

    nodeCoinbase = chain3.mc.coinbase;
    print(nodeCoinbase);

7. chain3.mc.mining:
~~~~~~~~~~~~~~~~~~~~

Returns boolean as to whether the node is currently mining.

.. code:: python

    miningState = chain3.mc.mining;
    print(miningState);  //true or false

8. chain3.mc.accounts:
~~~~~~~~~~~~~~~~~~~~~~

Returns the list of known accounts.

.. code:: python

    nodeAccounts = chain3.mc.accounts;
    print(nodeAccounts);

9. chain3.mc.blockNumber:
~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the number of the most recent block

.. code:: python

    nowBlockNumber = chain3.mc.blockNumber;
    print(nowBlockNumber);

10. chain3.mc.getBlockTransactionCount(block\_identifier):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the number of transactions in the block specified by
block\_identifier.

Delegates to mc\_getBlockTransactionCountByNumber if block\_identifier
is an integer or one of the predefined block parameters 'latest',
'earliest', 'pending', otherwise delegates to
mc\_getBlockTransactionCountByHash.

.. code:: python

    transactionCount = chain3.mc.getBlockTransactionCount(96160);
    print(transactionCount);

11. chain3.mc.getBalance(account, block\_identifier=mc.defaultBlock):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the balance of the given account at the block specified by
block\_identifier.

account may be a hex address or an ENS name

.. code:: python

    balance = chain3.mc.getBalance("0x36eaa71d7383be53cb600743aad08a55222a4915", block_identifier=chain3.mc.defaultBlock);
    print("getBalance1" + balance); //instanceof BigNumber
    print("getBalance2" + balance.toString(10));
    //Result: getBalance1:3.04527226722e+21  
    //         getBalance2:3045272267220000000000

12. chain3.mc.defaultBlock:
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The default block number that will be used for any RPC methods that
accept a block identifier. Defaults to 'latest'.

.. code:: python

    defultBlock = chain3.mc.defaultBlock;
    print("defaultBlock" + defultBlock);
    //default is latest，
    chain3.mc.defaultBlock = 123;  
    print("defaultBlock" + defultBlock);

13. chain3.mc.gasPrice:
~~~~~~~~~~~~~~~~~~~~~~~

Returns the current gas price in Sha = 1e-18 mc. GasPrice is calculated
from most recent blocks.

.. code:: python

    gasPrice = chain3.mc.gasPrice;
    print(gasPrice.toString(10));

14. chain3.mc.estimateGas(transaction\_params=None):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Uses the selected gas price strategy to calculate a gas price. This
method returns the gas price denominated in sha. The transaction\_params
argument is optional however some gas price strategies may require it to
be able to produce a gas price.

.. code:: python

    result = chain3.mc.estimateGas({
     to :"0xf7ebc6b854a202efe08e91422a44ba2161ed50dc",
     data: '0x23455654'
        //gas: 11,          //Optional, gaslimit of the TX
        //gasPrice: 11      //Optional, gasPrice
    });
    print('estimateGas  :'+ result);
    //Output：gasprice :20000000000
    //        estimateGas :1273

15. chain3.mc.getCode(account, block\_identifier=mc.defaultBlock):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the bytecode for the given account at the block specified by
block\_identifier. account may be a hex address or an ENS name

.. code:: python

    code  = chain3.mc.getCode("0x0000000000000000000000000000000000000065");//contract address

16. chain3.mc.syncing:
~~~~~~~~~~~~~~~~~~~~~~

Returns either False if the node is not syncing or a dictionary showing
sync status.

.. code:: python

    sync = chain3.mc.syncing;
    print('syncing  :'+ sync );
    //
    AttributeDict({
        'currentBlock': 2177557,
        'highestBlock': 2211611,
        'knownStates': 0,
        'pulledStates': 0,
        'startingBlock': 2177365,
    })

17. chain3.mc.getTransaction(transaction\_hash):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    blockHash = "0x6aa4a0db1fc155009bd9ba3a64c1aef109e1418dc05ee241d3e9e3e58d7f3eeb";
    transaction = chain3.mc.getTransaction(blockHash);
    print('get transaction:'+ str(transaction));

    /* Result:
    get transaction: AttributeDict({
       'blockHash': HexBytes('0x77483002572dd29b58640c4ccf5ef30278679037ff17b51cf613f3df562e5e0a'), 
       'blockNumber': 815006,
       'from': '0x0000000000000000000000000000000000000064', 
       'gas': 0, 
       'gasPrice': 20000000000,
       'hash': HexBytes('0x6aa4a0db1fc155009bd9ba3a64c1aef109e1418dc05ee241d3e9e3e58d7f3eeb'), 
       'input': '0xc1c0e9c4', 
       'nonce': 815005,
       'syscnt': '0x65', 
       'to': '0x0000000000000000000000000000000000000065', 
       'transactionIndex': 0, 
       'value': 0,
       'v': 0, 'r': HexBytes('0x00'), 's': HexBytes('0x00'), 
       'shardingFlag': 0})
    */

18. chain3.mc.getBlock(block\_identifier=mc.defaultBlock, full\_transactions=False):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the block specified by block\_identifier. Delegates to
mc\_getBlockByNumber if block\_identifier is an integer or one of the
predefined block parameters 'latest', 'earliest', 'pending', otherwise
delegates to mc\_getBlockByHash.

If full\_transactions is True then the 'transactions' key will contain
full transactions objects. Otherwise it will be an array of transaction
hashes.

.. code:: python

    getTheBlock = chain3.mc.getBlock(815006);
    print('get the block: '+ str(getTheBlock));

    /* Result:
    get the block({
       'difficulty': 86803583, 
       'extraData': HexBytes('0xdd854d4f41432d85312e302e312d87676f312e392e358777696e646f7773'), 
       'gasLimit': 9000000, 
       'gasUsed': 0,
       'hash': HexBytes('0x77483002572dd29b58640c4ccf5ef30278679037ff17b51cf613f3df562e5e0a'),
       'logsBloom': HexBytes('0x00000000000000000000000000000000000000000000000000000000000000000000000000
               000000000000000000000000000000000000000000000000000000000000000000000000000000000000
               000000000000000000000000000000000000000000000000000000000000000000000000000000000000
               000000000000000000000000000000000000000000000000000000000000000000000000000000000000
               000000000000000000000000000000000000000000000000000000000000000000000000000000000000
               000000000000000000000000000000000000000000000000000000000000000000000000000000000000
               000000000000000000'),
       'miner': '0x0a2168D2f08161c01745fEC4e6E8FE06F314Ab41', 
       'mixHash': HexBytes('0xc154897a85ca63bbbbb76b618a288f6b33f7d2994848dc9c43c6d65e6a5da355'),
       'nonce': HexBytes('0x829f5b23cdf8224f'), 
       'number': 815006, 
       'parentHash': HexBytes('0x73c0e4a94b48b41bf5a6a22151e38799a0e17e8b798848af5340f6d725027af1'),
       'receiptsRoot': HexBytes('0x9287370eb27f11b0c2188431cbc58a23b685f02dbd851ed4d974f932bd780839'), 
       'sha3Uncles': HexBytes('0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347'), 
       'size': 590, 
       'stateRoot': HexBytes('0x615d0a39783ae546e11aa0cd6e00c70c2ec989f51316c0f9e07cfc99f1088669'), 
       'timestamp': 1535530608, 
       'totalDifficulty': 136959813601540,
       'transactions': [HexBytes('0x6aa4a0db1fc155009bd9ba3a64c1aef109e1418dc05ee241d3e9e3e58d7f3eeb')],
       'transactionsRoot': HexBytes('0x7aba2a9c974693f1cfb96d506e6aa62942a174b4df39c831cf844a35e03249f0'), 
       'uncles': []
    })
    */

19. chain3.personal.unlockAccount(account, passphrase, duration=None):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Unlocks the given account for duration seconds. If duration is None then
the account will remain unlocked indefinitely. Returns boolean as to
whether the account was successfully unlocked.

.. code:: python

    chain3.personal.unlockAccount(mc.accounts[0], 'password')

20. chain3.miner.start(num\_threads):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Start the CPU mining process using the given number of threads.

.. code:: python

    chain3.miner.start(2) # number of threads

21. chain3.miner.stop:
~~~~~~~~~~~~~~~~~~~~~~

Stop the CPU mining operation

.. code:: python

    chain3.miner.stop()

22. chain3.miner.setGasPrice(gas\_price):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sets the minimum accepted gas price that this node will accept when
mining transactions. Any transactions with a gas price below this value
will be ignored.

.. code:: python

    chain3.miner.setGasPrice(19999999999)

23. chain3.mc.getTransactionReceipt((transaction\_hash, timeout=120):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the transaction receipt specified by transaction\_hash. If the
transaction has not yet been mined returns None

.. code:: python

    txr = chain3.mc.getTransactionReceipt('0x77483002572dd29b58640c4ccf5ef30278679037ff17b51cf613f3df562e5e0a')
    print(txr)

    /* Result:
    AttributeDict({
       'blockHash': HexBytes('0x77483002572dd29b58640c4ccf5ef30278679037ff17b51cf613f3df562e5e0a'), 
       'blockNumber': 815006,
       'contractAddress': '0x0000000000000000000000000000000000000065', 
       'cumulativeGasUsed': 0, 
       'from': '0x0000000000000000000000000000000000000064',
       'gasUsed': 0, 
       'logs': [], 
       'logsBloom': HexBytes('0x00000000000000000000000000000000000000000000000000000000000000000000000000
           000000000000000000000000000000000000000000000000000000000000000000000000000000000000
           000000000000000000000000000000000000000000000000000000000000000000000000000000000000
           000000000000000000000000000000000000000000000000000000000000000000000000000000000000
           000000000000000000000000000000000000000000000000000000000000000000000000000000000000
           000000000000000000000000000000000000000000000000000000000000000000000000000000000000
           000000000000000000'),
       'status': 1, 
       'to': '0x0000000000000000000000000000000000000065', 
       'transactionHash': HexBytes('0x6aa4a0db1fc155009bd9ba3a64c1aef109e1418dc05ee241d3e9e3e58d7f3eeb'),
       'transactionIndex': 0})
    */

24. chain3.mc.getTransactionCount((block\_identifier):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the number of transactions that have been sent from account as
of the block specified by block\_identifier.

.. code:: python

    chain3.mc.getTransactionCount('0x87E369172Af1e817ebD8d63bcD9f685A513a6736', block_identifier=chain3.mc.defaultBlock)

25. chain3.mc.sendTransaction(transaction, passphrase):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Signs and sends the given transaction

The transaction parameter should be a dictionary with the following
fields.

-  from: bytes or text, hex address or ENS name - (optional, default:
   chain3.mc.defaultAccount) The address the transaction is send from.
-  to: bytes or text, hex address or ENS name - (optional when creating
   new contract) The address the transaction is directed to.
-  gas: integer - (optional) Integer of the gas provided for the
   transaction execution. It will return unused gas.
-  gasPrice: integer - (optional, default: To-Be-Determined) Integer of
   the gasPrice used for each paid gas
-  value: integer - (optional) Integer of the value send with this
   transaction
-  data: bytes or text - The compiled code of a contract OR the hash of
   the invoked method signature and encoded parameters.
-  nonce: integer - (optional) Integer of a nonce. This allows to
   overwrite your own pending transactions that use the same nonce.

If the transaction specifies a data value but does not specify gas then
the gas value will be populated using the estimateGas() function with an
additional buffer of 100000 gas up to the gasLimit of the latest block.
In the event that the value returned by estimateGas() method is greater
than the gasLimit a ValueError will be raised.

-  shardingFlag:integer - (optional for Global Transactions), MicroChain
   flag, default value is 0 for Global TXs. To call MicroChain, this
   value has to be 1.

-  via: bytes or text, hex addres - (optional for Global Transactions),
   vode beneficial address, default is null for Global TXs. For
   microChain call

.. code:: python

    chain3.mc.sendTransaction({
        'to':'0xf103BC1c054baBcecD13e7AC1CF34F029647B08C',
        'from':'0x87E369172Af1e817ebD8d63bcD9f685A513a6736', 
        'value': 100000, 
        'gasPrice': chain3.mc.gasPrice,
        'shardingFlag': 0,
        'via': '0x0000000000000000000000000000000000000000',})
     

26. chain3.mc.sendRawTransaction(raw\_transaction):
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sends a signed and serialized transaction. Returns the transaction hash.

::

    private_key = '0x94645c7a048771045f90e0b88adf3ddf5afbb5029c2b1b5586d5afa9ba87c8f5'
    signed_txn = chain3.mc.account.signTransaction(
        dict(
            nonce=chain3.mc.getTransactionCount(chain3.mc.coinbase),
            gasPrice=chain3.mc.gasPrice,
            gas=100000,
            to='0xf103BC1c054baBcecD13e7AC1CF34F029647B08C',
            value=100000,
            data='0x',
            'chainId': networkid,
            'shardingFlag': 0,
            'via': '0x',
        ),
        private_key,
    )
    chain3.mc.sendRawTransaction(signed_txn.rawTransaction)

    /* Result: tx hash
        '0xd7e3a30f9eec70d5626b70a2082bd2573a2b0a282756479c2f48a57a833204ab'
    */  

