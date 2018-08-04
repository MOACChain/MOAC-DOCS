MicroChain SCS Monitor
----------------------

SCS Monitor is a SCS node monitoring MicroChain status. MicroChain users
can use this SCS node to monitor MicroChain status and get data from
MicroChain.

Users can use RPC settings to get information fron SCS monitor.

SCS monitor can only read data from MicroChain, not submitting any data.
It does not join the consensus and cannot get rewards from MicroChain.

SCS monitor can monitor MicroChain after it starts. The MicroChain user
need to call the "registerAsMonitor" method in MicroChain contract to
register it.

Example 1:

The registerAsMonitor call method to connect SCS monitor with
MicroChain:

.. code:: javascript

    function subchainRegisterAsMonitor (dappAddr,dappPasswd,microchainAddr,scsAddr)
    {
        chain3.personal.unlockAccount(dappAddr, dappPasswd,0);
        sendtx(dappAddr, microchainAddr, '0', '0x4e592e2f000000000000000000000000' + scsAddr );
    }

Parameters:

-  dappAddr、dappPasswd：Dapp user account and password to unlock the
   account;
-  microchainAddr：MicroChain contract address;
-  scsAddr：scsid of SCS monitor, should be in the
   directory:“…/scsserver/scskeystore”；
-  string: ‘0x4e592e2f’ is a constant to access the subchainbase method
   ‘registerAsMonitor(address monitor), do not change it.

Example 2:

Calling example

.. code:: javascrit

    subchainRegisterAsMonitor ('0x1b9ce7e4f156c1......913a56cd986786',
    ‘123’,
    '0x09f0dfc09bee......b85e5189a7493671',
    'f687272ae00f8cea01b8cedf37dd9be30329d8cf')//No prefix '0x' for the scsId
