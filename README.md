# XRPL Transactions Workshop

## Getting wallet credentials
[XRP Faucets](https://xrpl.org/xrp-testnet-faucet.html?utm_source=workshop&utm_medium=jason-morgan-state-mar-23&utm_campaign=dev-advocacy&utm_term=xrpl-transactions-workshop&utm_content=xrpl-transactions-workshop)

- First generate some test credentials on Testnet.
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

- View some of the docs
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
