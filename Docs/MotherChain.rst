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
| **Price**      | price   | price you are offering to pay, in unit of Sha |
+-------------------------+-----------+-------------------------------------------+
| **GasLimit**       | gaslimit   | maximum amount of gas allowed for the transaction, should less than 9,000,000|
+-------------------------+-----------+-------------------------------------------+
| **Recipient**         | to   | destination address (account or contract address)                           |
+-------------------------+-----------+-------------------------------------------+
| **Amount**      | amount  | moac to transfer to the destination, if any, in unit of Sha  |
+-------------------------+-----------+-------------------------------------------+
| **Payload**           | data  |  Information aobut Global Contract call, MicroChain Dapp transactions, etc.|
+-------------------------+-----------+-------------------------------------------+
| **ShardingFlag**             | shardingFlag  | Used to identify the TX types: 0 - MotherChain TX; 2 - MicroChain token transfer; 3 - MicroChain DAPP deploy        |
+-------------------------+-----------+-------------------------------------------+
| **Via**           | via  | VNODE proxy beneficial address                |
+-------------------------+-----------+-------------------------------------------+
| **SystemContract**           | systemFlag  | usually 0, only set by the internal process   |
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



