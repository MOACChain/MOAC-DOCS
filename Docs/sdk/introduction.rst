Introduction
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

MOAC developed a nodejs SDK for user to access the MOAC API methods.

Node.JS SDK installation
::
	npm install moac-api
	
Node.JS SDK error handling：

Users need to handle the SDK errors by using try and catch.

Example：
::
	var VnodeChain = require("moac-api").vnodeChain;

	try{
		var vc = new VnodeChain("http://47.106.69.61:8989");
		var blockNumber = vc.getBlockNumber();
		console.log(blockNumber);
	}catch (e){
		console.log(e);
	}
	
	



