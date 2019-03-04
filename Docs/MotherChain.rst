MotherChain
^^^^^^^^^^^

In MOAC network, MotherChain is an intersystem Proof of Work(POW) blockchain that handles data storage and compute processing for Smart Contracts and DApps.

The POW consensus node participates in a volunteer way. Each node contributes its computing power to solve the computation-intensive problem and verifies the validity of transactions in the agreed transaction set. 

Besides the POW consensus on transaction and data store set, each POW node is associated with one or more Smart Contract Server(SCS). SCS node could be local to the POW node, or it could be a remote node. The SCS identity is fully verifiable by the corresponding POW node, or Validating Node (VNODE). 

MOAC tranasaction format is different format from Ethereum. It contains three more fields: shardingFlag, via, and systemFlag. It also requires a chainId for all the transactions. 


+-------------------------+-----------+-------------------------------------------+
| Txdata field            | Json | Usage                                       |
+=========================+===========+===========================================+
| **AccountNonce**        | nonce     | transaction sequence number fr the sending account  |
+-------------------------+-----------+-------------------------------------------+
| **Price**      | 1e3 sha   | 1,000                                     |
+-------------------------+-----------+-------------------------------------------+
| **GasLimit**       | 1e6 sha   | 1,000,000                                 |
+-------------------------+-----------+-------------------------------------------+
| **Recipient**         | 1e9 sha   | 1,000,000,000                             |
+-------------------------+-----------+-------------------------------------------+
| **Amount**      | amount  | 1,000,000,000,000                         |
+-------------------------+-----------+-------------------------------------------+
| **Payload**           | data  | 1,000,000,000,000,000,000                 |
+-------------------------+-----------+-------------------------------------------+
| **ShardingFlag**             | shardingFlag  | 1,000,000,000,000,000                     |
+-------------------------+-----------+-------------------------------------------+
| **Via**           | via  | 1,000,000,000,000,000,000                 |
+-------------------------+-----------+-------------------------------------------+
| **SystemContract**           | systemFlag  | 1,000,000,000,000,000,000          |
+-------------------------+-----------+-------------------------------------------+

   ::
  type txdata struct {
    AccountNonce   `json:"nonce"    gencodec:"required"`
    SystemContract `json:"syscnt" gencodec:"required"`
              `json:"gasPrice" gencodec:"required"`
    GasLimit       `json:"gas"      gencodec:"required"`
    Recipient      `json:"to"       rlp:"nil"` // nil means contract creation
    Amount         `json:"value"    gencodec:"required"`
    Payload        `json:"input"    gencodec:"required"`
    ShardingFlag   `json:"shardingFlag" gencodec:"required"`
    Via            `json:"via"       rlp:"nil"`

    // Signature values
    V `json:"v" gencodec:"required"`
    R `json:"r" gencodec:"required"`
    S `json:"s" gencodec:"required"`

  }



