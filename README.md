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

- Get your test wallet balance
```
const test_wallet = xrpl.Wallet.fromSeed("<your seed here>") 
```
