# Nodemoney API

## 1. General information.

Nodemoney works on port 19775. All API methods have prefix: */api*.

**Like:** */api/someMethod*

API methods response format - JSON.

All responses have *status* field and *success* field. If something is wrong, response contains error field.


|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of operation. If operation is successfuly completed,  value =  true, if operation fails, value is  = false.|
|status|String|Contains result operation. If operation is successfully completed, value = "OK", if operation fails, value =  error message.|


## 2. API Methods.

### NOTE: This is only a partial list, more methods will be added in the near future.


### 2. 1. Create account.

**Url:** */api/login*

**Method:** *POST*

**Description:** Creates a new account, or unlocks an existing one, using the provided secret.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|walletKey|String|Secret phrase of account. From 10 characters.|

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or error message.|
|accountId|String|String. Account Id.|
|publicKey|String|Public key of account converted to hex format.|
|secretWord|String|Secret World of account converted to hex format.|
|accountTypes|Object|Contain all accounts (see example).|

**Important:** At this moment working only *nodeAccount*, other *in progress*

**Example of accountTypes:**
```javascript
{
    "nodeAccount": {
        "name": "node",
        "id": "15378171919540219400NODE",
        "postfix": "NODE",
        "amount": 0,
        "unconfirmedAmount": 0
    },
    "dollarAccount": {
        "name": "dollar",
        "id": "15378171919540219400USD",
        "postfix": "USD",
        "amount": 0,
        "unconfirmedAmount": 0
    },
    "euroAccount": {
        "name": "euro",
        "id": "15378171919540219400EUR",
        "postfix": "EUR",
        "amount": 0,
        "unconfirmedAmount": 0
    },
    "btcAccount": {
        "name": "btc",
        "id": "175tWpb8K1S7NmH4Zx6rewF9WQrcZv245W",
        "postfix": "BTC",
        "amount": 0,
        "unconfirmedAmount": 0
    }
}
```

### 2. 2. Get balance.

**Url:** */api/getAccountsByHash*

**Method:** *POST*

**Description:** Return balance of current account.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|secretWord|String|secretWord - received when login (2.1)|

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or error message.|
|accountTypes|Object|Contain all accounts (see in 2.1).|


### 2. 3. Send NODE.

**Url:** */api/createTransaction*

**Method:** *POST*

**Description:** Send NODE to another account. Fee will be calculated and applied automatically.

**Important:** Use json in body requests. And set Content-Type header to application/json.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|secretWord|String|SecretPhrase of sender account.|
|amount|Float/Integer|Amount to be sent.|
|recipientId|String|Recipient address.|

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or error message.|
|transactionId|String|Transactions ID.|

### 2. 4. Transactions List.

**Url:** */api/getMyTransactions*

**Method:** *GET*

**Description:** Return last N transactions for the provided address.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|accountId|String|Address of transactions (sender, recipient).|
|limit|Integer|Limit of transactions. Optional parameter. Default value: 100.|


**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or unique error code.|
|transactions|Array|JSON array of transactions.|


**Example of transaction:**
```javascript
{
    "deadline": "24",
    "senderPublicKey": "79178b35569fc3deeafda7ec69599bfbf8419c560d88d54a5e4a84cf7d780617",
    "recipientId": "17536351017439197252",
    "amount": 0.01,
    "fee": 0.00004,
    "referencedTransactionId": "5216593482388179282",
    "type": 0,
    "height": 652,
    "blockId": "16664288129201258323",
    "signature": "0a0ef3c110c8b877a4a50c4a7be21f2e362b6f6caa5276e77406d07ecf3f7002ef475959577fd2a67c65dc1062d3f3e6d5f23536e30ff010d4d2e0cfd2335f6c",
    "timestamp": 1408715727523,
    "attachment": null,
    "id": "16811594585232943987",
    "senderId": "9060009384610631744",
    "hash": "f89ee0e40331c2ab67a7f7962394e01f8f324a5e675a44aee94ec83420b0ab73",
    "confirmations": 2
}
```

### 2. 6. Get specific transaction details

**Url:** */api/getTransactionById?transactionId=TXT_ID*

**Method:** *GET*

**Description:** Return the details of the transaction specified by the transaction id.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|transactionId|String|ID of the transaction.|


**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or unique error code.|
|transaction|Object|JSON object of transaction.|


**Example of transaction: **
```javascript
{
    "deadline": "24",
    "senderPublicKey": "79178b35569fc3deeafda7ec69599bfbf8419c560d88d54a5e4a84cf7d780617",
    "recipientId": "17536351017439197252",
    "amount": 0.00997,
    "fee": 0.00003,
    "referencedTransactionId": "15465062855392744677",
    "type": 0,
    "height": 602,
    "blockId": "6041348209012418737",
    "signature": "351a89abf0671dc963df8cb55a7182fb908fb292e69e0ef38ba90b6d3e13cd066807229ab240d1d593b901721ee0abe68c77731cf86a43b55e494cc12f8c1079",
    "timestamp": 1408523231189,
    "attachment": null,
    "id": "3251142046188651787",
    "senderId": "9060009384610631744",
    "hash": "d1cf0b99f69b39adc0a39ff18779b0b8481eb573304d55a22d1e60773969450b",
    "confirmations": 3
}
```

### 2. 7. Get account list

**Url:** */api/getAllAccounts*

**Method:** *GET*

**Description:** Return the list of accounts witch was made at least one transaction or login into client.

**Parameters:**

none

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or unique error code.|
|accountList|Object|JSON object list of accounts.|

**Example of account list: **
```javascript
{
	"5347395950918755643":
	{
		"id":"5347395950918755643",
		"height":0,
		"publicKey":null,
		"keyHeight":null,
		"balance":50000,
		"unconfirmedBalance":50000,
		"guaranteedBalances":{},
		"assetBalances":{},
		"unconfirmedAssetBalances":{},
		"accountTypes":{}
	},
	...
}
```

### 2. 8. Get account by ID

**Url:** */api/getAccountById*

**Method:** *GET*

**Description:** Return the account info by ID.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|accountId|String|ID of the account (account number).|

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or unique error code.|
|account|Object|JSON object of account.|

**Example of account data: **
```javascript
{
		"15641667066091303754":
		{
			"id":"15641667066091303754",
			"height":0,
			"publicKey":[245,198,181,238,98,27,210,155,138,128,191,142,120,160,133,225,40,228,196,51,131,137,10,2,9,231,253,60,215,162,254,63],
			"keyHeight":null,
			"balance":2119.4535,
			"unconfirmedBalance":2119.4535,
			"guaranteedBalances":{},
			"assetBalances":{},
			"unconfirmedAssetBalances":{},
			"accountTypes":
			{
				"nxtlAccount":
				{
					"name":"nxtl",
					"id":"15641667066091303754NODE",
					"postfix":"NODE",
					"amount":2119.4535,
					"unconfirmedAmount":2119.4535
				}
				,"dollarAccount":
				{
					"name":"dollar",
					"id":"15641667066091303754USD",
					"postfix":"USD",
					"amount":0,
					"unconfirmedAmount":0
				},
				"euroAccount":
				{
					"name":"euro",
					"id":"15641667066091303754EUR",
					"postfix":"EUR",
					"amount":0,
					"unconfirmedAmount":0
				},
				"btcAccount":
				{
					"name":"btc",
					"id":"175tWpb8K1S7NmH4Zx6rewF9WQrcZv245W",
					"postfix":"BTC",
					"amount":0,
					"unconfirmedAmount":0
				}
			},
			"recentTransactions":[{"deadline":"24","senderPublicKey":"d917af7f13f4abad1907ea205d8fdb13ebde20278a78dabde043837676513b3d","recipientId":"17591617551191551229","amount":0.01,"fee":0.00004,"referencedTransactionId":"5100241183951494181","type":0,"height":787,"blockId":"6537391888209947095","signature":"bec9bb83ed59edada422920ab3a5584f047457bcc169c298150ec7bde4d10b078e8cd3057cc9061eff9742a2176683624f22826fe36c68191f6951eaeb92b8cc","timestamp":1409671213427,"attachment":null,"id":"12591178635433054986","senderId":"14931191424261653564","hash":"d8fb6e79556fa4d8d93aa14eb7b6a6bae3f2c6861bc8288aaebcda420c79930a","confirmations":1,"tbl":"transaction","_id":"xYxulFHLNvHRLAdM"}, ...]
		}
}
```

### 2. 9. Get peer list

**Url:** */api/getAllPeers*

**Method:** *GET*

**Description:** Return the list of peers connected to current client.

**Parameters:**

none

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or unique error code.|
|peers|Object|JSON object list of peers.|

**Example of peer list: **
```javascript
[
	{
		"port":19776,
		"host":"127.0.0.1",
		"uploadBytes":861,
		"downloadBytes":4312,
		"services":null,
		"MAX_LOST_CONNECTION":10,
		"lostConnection":0,
		"status":"check"
	},
	...
]
```