.. include:: include_announcement.rst

===============
Getting Started
===============

The chain3.js library is a collection of modules which contain specific functionality for the MOAC ecosystem.

- The ``chain3-mc`` is for the Ethereum blockchain and smart contracts
- The ``chain3-utils`` contains useful helper functions for DApp developers.


.. _adding-chain3:

Adding chain3.js
==============

.. index:: npm

First you need to get chain3.js into your project. This can be done using the following methods:

- npm: ``npm install chain3``

After that you need to create a chain3 instance and set a provider.
A MOAC compatible browser will have a ``window.ethereum`` or ``chain3.currentProvider`` available.
For  chain3.js, check ``Chain3.givenProvider``. If this property is ``null`` you should connect to your own local or remote node.

.. code-block:: javascript

    // in node.js use: const Chain3 = require('chain3');

    // use the given Provider, e.g in the browser with Metamask, or instantiate a new websocket provider
    const chain3 = new Chain3(Chain3.givenProvider || 'ws://localhost:8546', null, {});

    // or
    const chain3 = new Chain3(Chain3.givenProvider || new Chain3.providers.WebsocketProvider('ws://localhost:8546'), null, {});

    // Using the IPC provider in node.js
    const net = require('net');

    const chain3 = new Chain3('/Users/myuser/Library/MoacNode/moac.ipc', net, {}); // mac os path
    // or
    const chain3 = new Chain3(new Chain3.providers.IpcProvider('/Users/myuser/Library/MoacNode/moac.ipc', net, {})); // mac os path
    // on windows the path is: '\\\\.\\pipe\\moac.ipc'
    // on linux the path is: '/users/myuser/.MoacNode/moac.ipc'


That's it! now you can use the ``chain3`` object.
