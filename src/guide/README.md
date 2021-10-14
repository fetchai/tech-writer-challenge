# CLI - Introduction

The command line client provides all of the capabilities for interacting with the fetch ledger such as creating addresses, sending transactions and the governance capabilities. Before starting with the command line client you need to follow the installation instructions [here](building.md)

## Connecting to a network

While some users will want to connect a node to the network and sync the entire blockchain, for many however, it is quicker and easier to connect directly to existing publically available nodes.

### Connecting to fetchhub mainnet v2 network

To connect to the mainnet v2 network run the following configuration steps:

```bash
fetchd config chain-id fetchhub-2
fetchd config node https://rpc-fetchhub.fetch-ai.com:443
```

### Connecting to stargateworld network

To connect to the stargateworld network run the following configuration steps:

```bash
fetchd config chain-id stargateworld-3
fetchd config node https://rpc-stargateworld.fetch-ai.com:443
```

Checkout the [Network Information](../networks/) page for more detailed information on the available networks.


# CLI - Managing Tokens

## Querying your balance

Once `fetchd` is configured for the desired [network](../cli-introduction/). The user can query there balance using the following command:

```bash
fetchd query bank balances fetch1akvyhle79nts4rwn075t85xrwmp5ysuqynxcn4
```

If the address exists on the network then the user will expect to see an output in the following form:

```text
balances:
- amount: "8000000000000000000"
  denom: atestfet
pagination:
  next_key: null
  total: "0"
```


## Sending funds

To send funds from one address to another address then you would use the `tx send` subcommand. As shown below:

```bash
fetchd tx bank send <from address or key name> <target address> <amount>
```

In a more concrete example if the user wanted to send `100atestfet` from `main` key/address to `fetch106vm9q6ezu9va7v7e0cvq0nedc54egjm692fcp` then the following command would be used.

```bash
fetchd tx bank send main fetch106vm9q6ezu9va7v7e0cvq0nedc54egjm692fcp 100atestfet
```

When you run the command you will get a similar output and prompt. The user can check the details of the transfer and then press 'y' to confirm the transfer.

```text
{"body":{"messages":[{"@type":"/cosmos.bank.v1beta1.MsgSend","from_address":"fetch12cjntwl32dry7fxck8qlgxq6na3fk5juwjdyy3","to_address":"fetch1hph8kd54gl6qk0hy5rl08qw9gcr4vltmk3w02v","amount":[{"denom":"atestfet","amount":"100"}]}],"memo":"","timeout_height":"0","extension_options":[],"non_critical_extension_options":[]},"auth_info":{"signer_infos":[],"fee":{"amount":[],"gas_limit":"200000","payer":"","granter":""}},"signatures":[]}

confirm transaction before signing and broadcasting [y/N]: y
```

Once the transfer has been made a summary is presented to the user. An example is shown below:

```text
code: 0
codespace: ""
data: ""
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: 77C7382A0B1B9FE39257A6C16C7E3169A875CB3A87F2CE9D947D7C1335B53E76
```

On failure, the response will have a non zero code, as well as some logs under the `raw_log` key:

```text
code: 4
codespace: sdk
data: ""
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: 'signature verification failed; please verify account number (5815) and chain-id
  (stargateworld-1): unauthorized'
timestamp: ""
tx: null
txhash: 23701B052B423D63EB4AC94773B5B8227B03A576692A57999E92F2554F2372D4
```

# Multisig keys

This feature of `fetchd` allows users to securely control keys in a number of configurations. Using a threshold number K of maximum N keys, a user or group of users can set the minimum number of keys required to sign a transaction. Some examples of these configurations allow some useful features such as the choice of a spare key, where only one key is required to sign (K=1) but there are two keys available to do so. Another more complex example configuration is set out below.

## Creating a multisig key
The following represents the syntax and argument layout of the `fetchd` command to create a multisig key.

```
# Create a simple multisig key with a threshold of 1 as default
fetchd keys add <multisig_key_name> --multisig <list_of_key_names>

# Creating a multisig key with a higher threshold, K
fetchd keys add <multisig_key_name> --multisig <list_of_key_names> --multisig-threshold <threshold integer K>
```

### Example instantiation of a multisig key
This example represents a shared multisig key that could be used within a business amongst three account holders - where at least two of three (K=2) must sign off on each transaction.

```
# Create the three keys owned by the separate account holders
fetchd keys add fred
fetchd keys add ted
fetchd keys add ned 

# Create the multisig key from keys above
fetchd keys add business_key --multisig fred,ted,ned --multisig-threshold 2
```

## Signing and broadcasting multisig transactions
Transactions must be signed and broadcast before they are carried out.

In order to sign a multisig transaction, the transaction itself must not be immediately broadcast; but instead, the keyholders must each sign until a the minimum threshold K signatures are present.

*For this example we will be representing the transaction on the [Stargate](https://explore-stargateworld.fetch.ai/) network and will therefore will be using `atestfet` as the denomination - this should be changed relative to the currency used on the relevant network*

### Multisig transaction example

```
# Create a key to represent a vendor that the business must pay
fetchd keys add vendor

# Generate a transaction as an output file to be signed by 
# the keyholders, 'ted' and 'fred' in this example
fetchd tx bank send <business_key address> <vendor address> 1000atestfet --generate-only > transfer.json

# This transaction file (transfer.json) is then made available for
# the first keyholder to sign, 'fred'
fetchd tx sign transfer.json --chain-id stargateworld-1 --from fred --multisig <address of business_key> > transfer_fredsigned.json

# This is repeated for 'ted'
fetchd tx sign transfer.json --chain-id stargateworld-1 --from ted --multisig <address of business_key> > transfer_tedsigned.json

# These two files are then collated together and used as inputs to the
# multisign command to create a fully signed transaction
fetchd tx multisign transfer.json business_key transfer_fredsigned.json transfer_tedsigned.json > signed_transfer.json

# Now that the transaction is fully signed, it may be broadcast
fetchd tx broadcast signed_transfer.json

# Now display the result of the transaction and confirm that the vendor has
# received payment
fetchd query bank balances <address of vendor>
```
It is important to note that this method of signing transactions can apply to all types of transaction.

### Other multisig transaction examples

```
# In order to create a staking transaction using a multisig key
# the same process as above can be used with the output file of this command
fetchd tx staking delegate <wallet address> 10000atestfet --generate-only > stake.json

# The following command can also be used to create a withdrawal transaction for the
# rewards from staking when using a multisig key - this too must be signed as before
fetchd tx distribution withdraw-all-rewards --generate-only > withdrawal.json
```

