Move to Moac
============

MOAC developed based on the
`Ethereum <https://github.com/ethereum/go-ethereum>`__ project. It also
implements a ***javascript runtime environment*** (JSRE) that can be
used in either interactive (console) or non-interactive (script) mode.
It supports Dapp development through its own `chain3 <>`__ JavaScript
API, just like `web3.js <https://github.com/ethereum/web3.js>`__ on
Ethereum.

If you want to develop Dapp on MOAC, you should use Solidity. MOAC
supports the deployment of smart contracts through Remix, wallet.moac.io
and For a Dapp that wants to move to MOAC from Ethereum, please follow
these steps:

1. Deploy the smart contract:

   -  `Remix <https://remix.ethereum.org/>`__, a browser compiler for
      Solidity.
   -  `wallet.moac.io <http://wallet.moac.io/>`__, a browser Dapp
      connect with local nodes to deploy contracts.
   -  `Node.Js Solidity
      compiler <https://www.npmjs.com/package/solc>`__, a JavaScript
      bindings for the Solidity compiler.

   You can check the two examples in our wiki page:

   `ERC20 <https://github.com/MOACChain/moac-core/wiki/ERC20>`__

   `ERC721 <https://github.com/MOACChain/moac-core/wiki/ERC721>`__

2. Interact with the smart contract:

   -  `chain3 <https://github.com/MOACChain/chain3>`__, developed based
      on web3.js 0.2.x, is the JavaScript library supported by MOAC.
   -  `web3.js <https://github.com/ethereum/web3.js>`__, only the
      functions listed in chain3 are workable, such as web3, eth, admin.

Solidity
--------

-  `Solidity: a smart contract programming language sharing similarities
   with Javascript and
   C++ <https://solidity.readthedocs.org/en/latest/>`__
-  `Solidity
   Glossary <https://github.com/ethereum/wiki/wiki/Solidity-Glossary>`__
-  `Solidity
   Features <https://github.com/ethereum/wiki/wiki/Solidity-Features>`__
-  `Solidity
   Collections <https://github.com/ethereum/wiki/wiki/Solidity-Collections>`__
-  `Solidity
   Cheat-sheet <https://github.com/manojpramesh/solidity-cheatsheet>`__

Miscellaneous resources and info
--------------------------------

-  `Safety <https://github.com/ethereum/wiki/wiki/Safety>`__
-  `Ethereum ÐApp Developer
   Resources <https://github.com/ethereum/wiki/wiki/Dapp-Developer-Resources>`__
-  `Ethereum JavaScript
   API <https://github.com/ethereum/wiki/wiki/JavaScript-API>`__
-  `Ethereum JSON RPC
   API <https://github.com/ethereum/wiki/wiki/JSON-RPC>`__
-  `Standardized Contract
   APIs <https://github.com/ethereum/wiki/wiki/Standardized_Contract_APIs>`__
-  `Ethereum development
   tutorial <https://github.com/ethereum/wiki/wiki/Ethereum-Development-Tutorial>`__
-  `ÐApp using
   Meteor <https://github.com/ethereum/wiki/wiki/Dapp-using-Meteor>`__
-  `Dapp Insight: dapp statistics <https://dappinsight.com>`__
