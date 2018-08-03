*MicroChain select SCSs using the following methods:*

1. In the MicroChain contract, DAPP developer need to choose the range
   of SCSs[min,max] needed for the MicroChain.
2. Vnode compare the distance between the SCS account and the VNODE. If
   its smaller than the selected target, then notify the SCS.
3. SCS received the notice from register, then it need to answer with
   RegisterAsSCS function to confirm to join the MicroChain.

*By using this SCS selection method, we want to:*

1. The selection is random;
2. The selection can adjust with number of SCS in the pool;
3. The selection can be sure the selected SCSs are live in the network.

*Note: The required distance between two address(hash\_dist) is decided
from the value of index\_range RangeIndex[] 。*

.. figure:: https://raw.githubusercontent.com/wiki/moacchain/moac-core/image/scschiose.png
   :alt: 

