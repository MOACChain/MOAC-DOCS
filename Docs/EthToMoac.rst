Ethereum to Moac
============
If you want to develop Dapp on MOAC, you should use Solidity. MOAC
supports the deployment of smart contracts through Remix, wallet.moac.io
and for a Dapp that wants to move to MOAC from Ethereum, please follow
these steps:

1. Deploy the smart contract:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`Remix <https://remix.ethereum.org/>`__, a browser compiler for
Solidity.

`wallet.moac.io <http://wallet.moac.io/>`__, a browser Dapp
connect with local nodes to deploy contracts.

`Node.Js Solidity
compiler <https://www.npmjs.com/package/solc>`__, a JavaScript
bindings for the Solidity compiler.

You can check the two examples in our wiki page:

`ERC20 <https://github.com/MOACChain/moac-core/wiki/ERC20>`__

`ERC721 <https://github.com/MOACChain/moac-core/wiki/ERC721>`__

2. Interact with the smart contract:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`chain3 <https://github.com/MOACChain/chain3>`__, developed based on web3.js 0.2.x, is the JavaScript library supported by MOAC.
      
`web3.js <https://github.com/ethereum/web3.js>`__, only the functions listed in chain3 are workable, such as web3, eth, admin.

Example
~~~~~~~

Web3.js:

.. code:: js

    var Web3 = require('../index.js');
    var web3 = new Web3();
    
    web3.setProvider(new web3.providers.HttpProvider('http://localhost:8545'));
    
    var coinbase = web3.eth.coinbase;
    console.log(coinbase);
    
    var balance = web3.eth.getBalance(coinbase);
    console.log(balance.toString(10));
    
Chain3:

.. code:: js


    var Chain3 = require('../index.js');
    var chain3 = new Chain3();
    
    chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
    
    var coinbase = chain3.mc.accounts[0];
    console.log(coinbase);
    
    var balance = web3.eth.getBalance(coinbase);
    console.log(balance.toString(10));
    

Links:
^^^^^^

:doc:`JSON-RPC`

`MOAC-tx <https://github.com/MOACChain/moac-tx>`__

`Ethereum to MOAC Webinar <https://www.youtube.com/watch?v=EALZuizLeb4>`__

`Ethereumjs-tx <https://www.npmjs.com/package/ethereumjs-tx>`__
