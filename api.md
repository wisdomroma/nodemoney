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
