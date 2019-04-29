Multiple Dapps on a MicroChain
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After nuwa 1.0.8, multiple Dapps can be depoyed on one single MicroChain.
It enables the developers to use more complex contracts or upgrade the Dapps on the MicroChain.


Dapps interaction
-----------------

This example use dapp2 to call the functions in dapp1.

STEP0：Deploy dappbase.sol;

STEP1：Deploy dapp1.sol, register in dappbase;
::

    contract Dapp1 {
    struct tup {
        uint amount;
        string desc;
    }
    tup[] public tups;
    function addTup(uint amount, string desc) public {
        tups.push(tup(desc));
    }
    function getString() public view returns (string) {
        return tups[0].desc;
    }
    function getTup(address addr) public view returns (tup) {
        for (uint i=0; i<tups.length; i++) {
            if (tups[i].owner == addr) {
                return tups[i];
            }
        }
    }

STEP2：Deploy dapp2.sol, calls the Function getString in dapp1;
::

    contract Dapp1 {
        struct tup {
            uint amount;
            string desc;
        }
        function getUint() public view returns (uint);
        function getString() public view returns (string);
        function getTup(address addr) public view returns (tup);
    }
    contract Dapp2 {
        struct tup {
            uint amount;
            string desc;
        }
        function Dapp2(address dapp1addr) public {
            Dapp1 dp1 = Dapp1(addr1);
        }
        function getString(address addr1) public view returns (string) {
            return dp1.getString();
        }
    }

Example：
::

    > nonce = 1   // call ScsRPCMethod.GetNonce on SCS monitor
    > subchainaddr = '0xb877bf4e4cc94fd9168313e00047b77217760930';
    > dappaddr = '0xcc0D18E77748AeBe3cC6462be0EF724e391a4aD9';
    > via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c';
    > data = dappaddr + '89ea642f';
    > chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
    > chain3.mc.sendTransaction( { nonce: nonce, from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas:0, shardingFlag:'0x1', data: data, via: via,});


Upgrade the Dapp
-------------------

This example showed how to upgrade the Dapp.
First, use dapp3 to call the functions in dapp1,
then use dapp4 to use the data in dapp3.


STEP0：Deploy dappbase.sol;

STEP1：Deploy dapp1.sol，register in dappbase;
::

     contract Dapp1 {
        struct tup {
            uint amount;
            address addr;
        }
        tup[] public tups;
        tup[] private mytups;
       
        function addTup(uint amount, address addr) public {
            tups.push(tup(amount, addr));
        }
      
        function getAmountByAddr(address myaddr) public view returns (tup[]) {
            for (uint i=0;i<tups.length;i++){
                if (tups[i].addr == myaddr){
                    mytups.push(tups[i]);
                }
            }
            return mytups;
        }
    }

STEP2：Deploy dapp3.sol to test calling method in dapp1. Then Dapp3 call getAmountByAddr method in Dapp1 to get the value into the variable mytups from Dapp1.

::

    contract Dapp1 {
        struct tup {
            uint amount;
            address addr;
        }
        function getAmountByAddr(address) public view returns (tup[]);
    }
      
    contract Dapp3 {
        struct tup {
            uint amount;
            address addr;
        }
      
        Dapp1 public dp1;
        tup[] public tups;
        tup[] private mytups;
        function Dapp3(address dapp1addr) public{
            dp1 = Dapp1(dapp1addr);
        }
       
        function addTup(uint amount, address addr) public {
            tups.push(tup(amount, addr));
        }
     
        function getAmountByAddr(address addr) public view returns (tup[]) {
            Dapp1.tup[] memory oldtups = new Dapp1.tup[](10);
            oldtups = dp1.getAmountByAddr(addr);
            for (uint i=0;i<oldtups.length;i++){
                if (oldtups[i].addr == addr){
                    mytups.push(tup(oldtups[i].amount, oldtups[i].addr));
                }
            }
       
            for (i=0;i<tups.length;i++){
                if (tups[i].addr == addr){
                    mytups.push(tups[i]);
                }
            }
            return mytups;
        }
    }


STEP3：Deploy dapp4.sol to test calling method in dapp3. The getAmountByAddr method call the method getAmountByAddr in dapp3. Since dapp3 method calls the method in dapp1, this call finally returns the value of variable in dapp1.

::

  contract Dapp3 {
            struct tup {
                uint amount;
                address addr;
            }
            function getUint() public view returns (uint);
            function getString() public view returns (string);
            function getAmountByAddr(address) public view returns (tup[]);
    } 
     
    contract Dapp4 {
        struct tup {
                uint amount;
                address addr;
        }
     
        tup[] public tups;
        Dapp3 dp3;
        tup[] private mytups;
        function Dapp4(address dapp3addr)  public {
            dp3 = Dapp3(dapp3addr);
        }
          
        function addTup(uint amount, address addr) public {
            tups.push(tup(amount, addr));
        }
      
        function getAmountByAddr(address addr) public view returns (tup[]) {
            //get dapp3 data
            Dapp3.tup[] memory oldtups = new Dapp3.tup[](10);
            oldtups = dp3.getAmountByAddr(addr);
            for (uint i=0;i<oldtups.length;i++){
                if (oldtups[i].addr == addr){
                    mytups.push(tup(oldtups[i].amount, oldtups[i].addr));
                }
            }
            //new data
            for (i=0;i<tups.length;i++){
                if (tups[i].addr == addr){
                    mytups.push(tups[i]);
                }
            }
            return mytups;
        }
    }
