MOAC network
^^^^^^^^^^^^

By separating balance transfer and Smart Contracts, the platform’s advanced layered Multi-blockchain    architecture    increases   overall transaction processing speeds up to 100x faster (TPS) than Ethereum. The MOAC architecture  is  overviewed  below,  consisting of MotherChain       , an event handling system, Smart Contracts as MicroChains , blockchain sharding, Cross-Chain capabilities, security, and an API.

.. figure:: ../image/MOACNetwork.png
   :alt: moac\_key\_person


BaseChain
~~~~~~~~~~~

MotherChain is the public blockchain layer that processes balance transfers, blockchain operation, consensus, and data access. It is an intersystem Proof of Work blockchain that handles data storage and compute processing for Smart Contracts and DApps.


AppChain
~~~~~~~~~~
MOAC is one of the first blockchain solutions to implement a unique MicroChain per Smart Contract, providing for efficiency and scalability beyond existing solutions.

The MOAC Platform uses MicroChains to separate processing tasks and isolate blockchain functions from business logic for each individual smart contract. By providing each Smart Contract with its own unique MicroChain, it enables Smart Contracts to use a variety of consensus protocols and results in a wider range of potential business logic use cases. Developers have the freedom to select the consensus protocol that best fits their use case and determine the number of nodes allocated to a specific Smart Contract. All the states of the Smart Contract are saved inside the local MicroChain™ and can write data to the MotherChain™ as needed for finality.


1. :doc:`MotherChain`
2. :doc:`MicroChain`


