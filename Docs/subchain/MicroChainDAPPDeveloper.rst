DAPP Developers
---------------

DAPP Developer can create a MicroChain and running his DAPP on the
MicroChain. He need to deposit enough MOAC in the MicroChain to reward
the SCSs running the MicroChain.

DAPP Developer need to form his own MicroChain to run the Contracts.

Deploy MicroChain control contract
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

subchainbase is the contract for the DAPP developers to form the
MicroChain. It provides the MicroChain launch and running methods.

The parameters are as following:

1. proto：subchainprotocolbase contract address, obtain from MOAC team;
2. vnodeProtocolBaseAddr：vnodeprotocolbase contract address，obtain
   from MOAC team；
3. min：min SCSs required to launch MicroChain, suggest value > 3;
4. max：max SCS required to launch MicroChaib, suggest value < 100;
5. thousandth：default is 1;
6. flushRound：MicroChain flush interval between MotherChain blocks, min
   number is 40;
7. The gas limit need set to 6700000 for deploying.

Example:

.. code:: javascript

    chain3.personal.unlockAccount(mc.accounts[0], 'passwd',);//unlock the account to deploy
    var proto = "0x8fab1913cc......2deb725" ;
    var vnodeProtocolBaseAddr = "0x8fab1913c......2db52deb725" ;
    var min = 1 ;
    var max = 10 ;
    var thousandth = 1 ;
    var flushRound = 20 ;
    var subchainbaseContract = chain3.mc.contract([{"constant":true,"inputs":[],"name":"maxMember",......,"type":"event"}]);
    var subchainbase = subchainbaseContract.new(
       proto,
       vnodeProtocolBaseAddr,
       min,
       max,
       thousandth,
       flushRound,
       {
         from: chain3.mc.accounts[0], 
         data: '0x6060604052600c601555670d...708e8ee3c23da8b02d0278eb0029', 
         gas: '6700000'
       }, function (e, contract){
        console.log(e, contract);
        if (typeof contract.address !== 'undefined') {
             console.log('Contract mined! address: ' + contract.address + ' transactionHash: ' + contract.transactionHash);
        }
     })

B2、RegisterOpen：
~~~~~~~~~~~~~~~~~~

The RegisterOpen is doing the following tasks:

-  Dapp developer call this function on MotherChain to start the
   MicroChain；
-  MotherChain broadcast the call to all the VNODES. If the VNODE
   contains a valid SCS, it will wait for the selection signal；
-  If SCS receives a selection signal, it need to send a transaction to
   the MicroChain contract to finish the registeration (This is why SCS
   need to have some initial MOAC deposit).
-  MicroChain will collect the confirmations undtil it reaches the max
   limit as defined in the contract.

The MicroChain chooses in the SCS pool to form the microChain
validators. By default, this process is random. The microChain creator
can also change the selection process and only allow specific SCSs to
join.

Example:

.. code:: javascript

    function subchainRegisterOpen(dappAddr,dappPasswd,subchainAddr)
    {
        chain3.personal.unlockAccount(dappAddr, dappPasswd,0);
        sendtx(dappAddr, subchainAddr, '0','0x5defc56c' );
    }

Comments:

-  dappAddr、dappPasswd：Dapp Developer account and password to make the
   call；
-  subchainAddr: subchainbase contract address;
-  data：In sendtx, '0x5defc56c' is a constant to send with. It was
   generate from the first 4 bytes in the hash of registerOpen()
   function.

Example:

.. code:: javascript

    subchainRegisterOpen('0x1b9ce7e4f15...38913a56cd986786',
    ‘123’,
    '0x6f5b81365fb1d3d...536cc78749af2c')

After registerOpen is called，DAPP developer can use the following
methods to check how many SCS nodes registed in the MicroChain:

Method 1：

In the Console, check after call RegisterOpern in subchainBase:

This is to call the nodeCount function in subchainBase contract.

.. code:: javascript

    > subchainBase.nodeCount()

Method 2:

In the Console, call the subchain address to check the value of
nodeCount ('0x0e'):

.. code:: javascript

    > mc.getStorageAt(subchainAddr,0x0e)

When enough SCS nodes registerd in the MicroChain, continue to next
Step: RegisterClose().

B3、RegisterClose：
~~~~~~~~~~~~~~~~~~~

The RegisterClose is doing the following tasks:

-  Dapp developer call the RegisterClose function;
-  The contract checks if the number of SCS registered is larger than
   the min number required in the MicroChain contract. If yes, continue.
   Otherwise, the register is void;
-  The contract is broacast to all the VNODEs and SCSs that the
   registration is closed;
-  The registered SCSs receive this broachasting, init the MicroChain
   and start generating MicroChain blocks.

After RegisterClose，SCSs cannot register through the MicroChain
contract. The SCSs registered can participate the and get rewards from
the MicroChain.

Example：

.. code:: javascript

    function subchainRegisterClose(dappAddr,dappPasswd,subchainAddr)
    {
        chain3.personal.unlockAccount(dappAddr, dappPasswd,0);
        sendtx(dappAddr, subchainAddr, '0','0x69f3576f' );
    }

Comments:

-  dappAddr、dappPasswd：Dapp developer account and password to send the
   TX;
-  subchainAddr：MicroChain contract subchainbase address;
-  '0x69f3576f': constant, generated from the subchainbase
   registerClose() function by using Keccak256 hash.

Example:

.. code:: javascript

    subchainRegisterClose('0x1b9ce7e4f15......e0e38913a56cd986786',
    ‘123’,
    '0x6f5b81365fb1d3......6907fa536cc78749af2c')

Besure to have enough SCS nodes registered befor calling Registerclose.
Otherwise you need to start the process again.

B4、Deploy DAPP contract on the MicroChain
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

DAPP contact can be deployed on the MicroChain through directcall.
Directcall can be performed under the console using mc.sendTransaction.

1. from: The DAPP source account, need to unlock;
2. value: Direct call don't need any mc, you can put any non negative
   number here, suggest 0.
3. to: MicroChain contract subchainbase address;
4. gas: Direct calls don't use any gas, put 0;
5. gasPrice: Direct calls don't use any gas, put 0;
6. shardingflag: Need to set value to '0x1';
7. nonce: Note this is the nonce for the MicroChain.
8. data: The data is generated from compiled DAPP contract. After
   compiled the DAPP contract, you need to put the BIN as data.
9. via: this need to be a VNODE-PROXY address. You can get this address
   by run a local MOAC VNODE as proxy or use one from others.

To check the deploy results, referring to B6.

Example:

.. code:: javascript

    function deploycode()
    {
        chain3.personal.unlockAccount(mc.accounts[0],'',0);
        chain3.mc.sendTransaction(
            {
                from: mc.accounts[0],
                value:chain3.toSha('0','mc'),
                to: subchainbase,
                gas: "0",
                gasPrice: "0",
                shardingflag: "0x1",
                nonce: 1,
                data: '0x606060405234156......9c6697187ac00029',
                via: '0x78e1b4584085......e3cff29f11f8d5e08f54dc'
            });
            
        console.log('sending from:' +     src + ' to:' + tgtaddr  + ' with data:' + strData);
    }

B5、DAPP function calls
~~~~~~~~~~~~~~~~~~~~~~~

To make the DAPP function calls, users also need to make a direct call.
First, user need to compile the DAPP function calls and saved in the
data section. Then send a transaction to MicroChain address, with
correct parameters (B4). The results can be checked later (B6).

Example：

.. code:: javascript

    function testSet(num)
    {
        chain3.personal.unlockAccount(mc.accounts[0],'',0);
        chain3.mc.sendTransaction(
            {
                from: mc.accounts[0],
                value:chain3.toSha('0','mc'),
                to: subchainbase,
                gas: "0",
                gasPrice: "0",
                shardingflag: "0x1",
                nonce: num,
                data: '0x4f2be91f',
                via: '0x78e1b45840850......ff29f11f8d5e08f54dc'
            });
            
        //console.log('sending from:' +     src + ' to:' + tgtaddr  + ' with data:' + strData);
    }

B6、Check MicroChain status
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The status of the DAPP can be checked through SCS monitor service. User
can start a SCS with monitor service by using the RPC option:

-  -rpcaddr [addr] SCS turn on rpc ip
-  -rpcport [port] SCS turn on rpc port

Data structure：

.. code:: go

    type Args struct {
        Sender       common.Address      // Dapp owner
        SubChainAddr common.Address
    }
    type ArgsData struct {
        Sender       common.Address     // Dapp owner
        SubChainAddr common.Address
        Func         string             // eg:"SetData()", "rpcGetData()"
    }

SCS RPC reference
~~~~~~~~~~~~~~~~~

**GetScsId**

func GetScsId(args *Args, reply *\ common.Address) error

Return the SCSID of SCS.

Parameters: args - MicroChain id reply - Returned SCS id

Example：

.. code:: go

    client, err := rpc.DialHTTP("tcp", serverAddress+":"+serverPort)
    var scsid common.Address
    client.Call("ScsRPCMethod.GetScsId", Args{}, &scsid)

**GetNonce**

func GetNonce(args *Args, reply *\ uint64) error

Return the MicroChain nonce.

Parameters: args - MicroChain id reply - returned nonce value

Example:

.. code:: go

    args := Args{sender, subChainAddr}
    var noce uint64//
    client.Call("ScsRPCMethod.GetNonce", args, & noce)

**GetData**

func GetData(argsData *ArgsData, reply *\ []byte)

Return a function value in DAPP contract. The funcation need to have not
input parameters.

Example:

.. code:: go

    var replyData []byte
    argsData := ArgsData{sender, subChainAddr, "GetData()"}
    client.Call("ScsRPCMethod.GetData", argsData, &replyData)

**GetDappState**

func (scs *ScsRPCMethod) GetDappState(args *\ Args, reply \*uint64)
error

Return the DAPP contract status

-  0: not created;
-  1: created successfully;

Example:

.. code:: go

    args := Args{sender, subChainAddr}
    var reply uint64
    client.Call("ScsRPCMethod.GetDappState", args, &reply)

**GetContractInfo：**

Check DAPP MicroChain using http protocol.

Parameters:

.. code:: go

    type ContractInfoReq struct {
        Reqtype      int//Request type: 0 - all; 1 - Array; 2 - mapping; 3 - structure; 4 - short types; 5 - long types, such as string, bytes; 
        Key          string//64 bytes HEX string, this is the index of the variable in the contract. Optional if request all variables.
        Position     string//64 bytes HEX string，when Reqtype=1，this is the variable index in the array; when Reqtype = 2, this is the mapping indes. 
        Structformat []byte//Only used for structure type, 1 - single; 2 - list; 3 - string; 
    }

    type GetContractInfoReq struct {
        SubChainAddr common.Address//contract address of DAPP 
        Request      []ContractInfoReq//Variables requested. 
    }

Returned parameters:

.. code:: go

    type ContractInfo struct {
        Balance  *big.Int
        Nonce    uint64
        Root     common.Hash
        CodeHash []byte
        Code     []byte
        Storage  map[string]string
    }
