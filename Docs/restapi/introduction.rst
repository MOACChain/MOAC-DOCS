Introduction
^^^^^^^^^^^^^

To help the users access the MicroChains, we have built services and APIs around MOAC JSON RPC over HTTP/HTTPS that you can use with your favorite libraries and frameworks, on the MOAC networks.


URLs
---------------------------

Using https to communicate with API server and POST methods to use the APIs.

url format:    http(s)://server.com/api/{module}/{version}/{method}

default version = v1.0

POST format is using the format:    Content-Type: application/x-www-form-urlencoded (a=name&b=666)


Results
---------------------------
The return results will be in JSON format
::	
	{
		"success": true,
		"message": "",
		"data": "*********"
	}
	success:  true/false
	message： error message
	data：	  data returned

Status code:
200 OK
403 Limited access token


Access control
---------------------------


Users need to have a Access Token(AT) to access the API service, otherwise a 403 ERROR is returned from the API server. User should use the auth method to request an AT. Each AT will be valide for 2 hours at the moment.


Safety control
---------------------------

The API server provides two options for user's Private Key:

1. After registration, user receive his private key. User needs to send the private key to the API server to sign a transaction;
2. After registration, user only gets an encode. The private key is saved in the API server with the encode. User sends the encode for the API server to decode the Private Key and sign the transaction.


API servers
---------------------------

Currently only the testnet API server is available to the public.

Testnet API server：http://139.198.126.104:8080

Mainnet API server：Available for request.






