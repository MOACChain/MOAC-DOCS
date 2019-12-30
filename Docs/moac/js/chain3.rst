
.. include:: include_announcement.rst

=======
Chain3
=======

    The Chain3 class is a wrapper to house all MOAC related modules.
    
.. _api-ref:

Initiating of Chain3
=====================

----------
Parameters
----------

1. ``provider`` - ``string|object``: A URL or one of the Chain3 provider classes.
2. ``net`` - ``net.Socket`` (optional): The net NodeJS package.
3. ``options`` - ``object`` (optional) The Chain3 :ref:`options <chain3-module-options>`


-------
Example
-------

.. code-block:: javascript

    import Chain3 from 'chain3';

    // "Chain3.givenProvider" will be set in a MOAC supported browser.
    const chain3 = new Chain3(Chain3.givenProvider || 'ws://some.local-or-remote.node:8546', net, options);

    > chain3.mc
    > chain3.utils
    > chain3.version


------------------------------------------------------------------------------


Chain3.modules
=====================

    This Static property will return an object with the classes of all major sub modules, to be able to instantiate them manually.

-------
Returns
-------

``Object``: A list of modules:
    - ``Mc`` - ``Function``: the Mc module for interacting with the MOAC network see :ref:`chain3.mc <mc>` for more.
    - ``Net`` - ``Function``: the Net module for interacting with network properties see :ref:`chain3.mc.net <mc-net>` for more.
    - ``Personal`` - ``Function``: the Personal module for interacting with the Ethereum accounts see :ref:`chain3.mc.personal <mc-personal>` for more.

-------
Example
-------

.. code-block:: javascript

    Chain3.modules
    > {
        Mc(provider, net?, options?),
        Net(provider, net?, options?),
        Personal(provider, net?, options?),
    }


.. include:: include_package-core.rst

------------------------------------------------------------------------------

version
=====================

    Property of the Chain3 class.

.. code-block:: javascript

    chain3.version

Contains the version of the ``chain3`` wrapper class.

-------
Returns
-------

``String``: The current version.

-------
Example
-------

.. code-block:: javascript

    chain3.version;
    > "1.0.0"
