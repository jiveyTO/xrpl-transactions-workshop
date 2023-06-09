# XRPL Transactions Workshop

Useful doc sites (right click on all links to open in new tab)

- [XRPL.org](https://xrpl.org?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)
- [js.XRPL.org](https://js.xrpl.org?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)

## Getting wallet credentials
[XRP Faucets](https://xrpl.org/xrp-testnet-faucet.html?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)

- First generate some test credentials on Testnet
- Copy and paste your seed and address somewhere, we'll need it later
- Check your balance on [Bithomp](https://bithomp.com/) or [XRPL Explorer](https://testnet.xrpl.org/?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)


- Open a terminal and go to Downloads
- Create a new directory and cd into it
- Do an `npm init -y`
- `npm install xrpl`
- Touch file 'my-first-XRPL-transaction.js'
- Open file with VS Code


- Import the XRPL library `const xrpl = require("xrpl")`

[XRPL JS](https://js.xrpl.org/?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)

- Connect to the ledger
```
async function main() {

  // Define the network client
  const client = new xrpl.Client("wss://s.altnet.rippletest.net:51233")
  await client.connect()

  // ... custom code goes here

  // Disconnect when done (If you omit this, Node.js won't end the process)
  client.disconnect()
}

main()
```

- Get your test wallet 
- Consoloe log `test_wallet`
```
const test_wallet = xrpl.Wallet.fromSeed("<your seed here>") 
console.log(test_wallet)
```

- Query the ledger
```
// Get info from the ledger about the address we just funded
  const response = await client.request({
    "command": "account_info",
    "account": test_wallet.address,
    "ledger_index": "validated"
  })
  console.log(response)
```

View some of the docs
- [account_info](https://js.xrpl.org/interfaces/AccountInfoRequest.html?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)
- [ledger_index](https://github.com/XRPLF/xrpl.js/blob/1a1d80874a7e971ab433484ba027f95f8c8c0030/packages/xrpl/src/models/common/index.ts)
- The JS docs are autogenerated so sometimes you have to go hunting for things


- Account info [response](https://js.xrpl.org/interfaces/AccountInfoResponse.html?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)
 
 
 ## Listen for events
 ```
 // Listen to ledger close events
  client.request({
    "command": "subscribe",
    "streams": ["ledger"]
  })
  client.on("ledgerClosed", async (ledger) => {
    console.log(`Ledger #${ledger.ledger_index} validated with ${ledger.txn_count} transactions!`)
  })
  ```
  
  - [Subscribe request](https://js.xrpl.org/interfaces/SubscribeRequest.html?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)
  - [Subscribe response](https://js.xrpl.org/interfaces/SubscribeResponse.html?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)


- Comment out client.disconnect() to keep listening


## Send XRP to each other
```
  // Prepare transaction 
  const prepared = await client.autofill({
    "TransactionType": "Payment",
    "Account": test_wallet.address,
    "Amount": xrpl.xrpToDrops("<put a random amound to send, notice the function converts to drops for you>"),
    "Destination": "<get your classmates address>"
  })
  const max_ledger = prepared.LastLedgerSequence
  console.log("Prepared transaction instructions:", prepared)
  console.log("Transaction cost:", xrpl.dropsToXrp(prepared.Fee), "XRP")
  console.log("Transaction expires after ledger:", max_ledger, "\n")
  ```
  
  - Let's put our addresses in the chat
  - Now choose one address to send to and enter it in the destination field
  - Note that technically you need more fields to complete the transaction but we use a special function called autofill to populate these fields for us.
  
Some docs to look at 
- [Transaction common fields](https://xrpl.org/transaction-common-fields.html?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)   
- [Transaction costs](https://xrpl.org/transaction-cost.html?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)
- [Other transaction types](https://xrpl.org/transaction-types.html?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)


```
  // Sign prepared instructions 
  const signed = test_wallet.sign(prepared)
  console.log("Identifying hash:", signed.hash)
  console.log("Signed blob:", signed.tx_blob, "\n")
```

The result of the signing operation is a transaction object containing a signature. Typically, XRP Ledger APIs expect a signed transaction to be the hexadecimal representation of the transaction's canonical binary format, called a "blob".

- [The sign function](https://js.xrpl.org/classes/Wallet.html?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop#sign)

```
  // Submit signed blob
  const tx = await client.submitAndWait(signed.tx_blob)
  console.log("tx = " + JSON.stringify(tx, null, 3))
  console.log("Transaction result:", tx.result.meta.TransactionResult)
  console.log("Balance changes:", JSON.stringify(xrpl.getBalanceChanges(tx.result.meta), null, 2))  
```

In a few seconds you should see the `tesSUCCESS` in the `TransactionResult` field

- [Submit and wait function](https://js.xrpl.org/classes/Client.html?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop#submitAndWait)


## Using XUMM

- Download the [XUMM app](https://xumm.app/). You will need it for the NFT workshop and during the hackathon to get you POAP.
- Import an account using a seed you just generated
- Switch to Testnet
- Register a new app on the [developer dashboard](https://apps.xumm.dev/).
- In your terminal run `npm install xumm-sdk`
- Touch a new file `xumm-test.js`

```
const {XummSdk} = require('xumm-sdk')
const Sdk = new XummSdk(<your xumm key>, <your xumm key>)

const main = async () => {
  const appInfo = await Sdk.ping()
  console.log(appInfo.application.name)

  const request = {
    "TransactionType": "Payment",
    "Destination": "<address to send to>",
    "Amount": "<an amount of xrp>",
    "Memos": [
      {
        "Memo": {
          "MemoData": "4d6f7267616e20537461746520526f636b7321"
        }
      }
    ]
  }

  const payload = await Sdk.payload.create(request, true)
  console.log(payload)
}

main()
```
