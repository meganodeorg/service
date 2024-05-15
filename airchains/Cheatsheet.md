# Cheat sheet

### key management
#### #add new wallet
```
junctiond keys add $WALLET
```
#### #restore wallet
```
junctiond keys add $WALLET --recover
```

#### #check balances of wallet
```
junctiond q bank balances $(junctiond keys show $WALLET -a)
```

### management validator
Before executing the command `junctiond tx staking create-validator path/to/validator.json --from keyname`, you need to create a `validator.json` file with the following details. Below is an example:

To obtain the pubkey, you can use the command:

```
junctiond comet show-validator
```
The output will be something like this:
```
{"@type":"/cosmos.crypto.ed25519.PubKey","key":"ZXONS7NNjLWH4HePBOoHKDAYeLXQO5iUwpCRQSi1poI="}
```
You'll need to paste the pubkey value into the pubkey section of the JSON file.

{% hint style="info" %}
** Please adjust the staking amount and other keys as you see fit. **
{% endhint %}
```
{
	"pubkey": <validator-pub-key>,
	"amount": "1000000amf",
	"moniker": "<validator-name>",
	"identity": "optional identity signature (ex. UPort or Keybase)",
	"website": "validator's (optional) website",
	"security": "validator's (optional) security contact email",
	"details": "validator's (optional) details",
	"commission-rate": "0.1",
	"commission-max-rate": "0.2",
	"commission-max-change-rate": "0.01",
	"min-self-delegation": "1"
}
```
```
junctiond tx staking create-validator path/to/validator.json --from <key-name> --chain-id junction --fees 500amf
```
{% hint style="info" %}
** A prompt will appear in the CLI. To proceed, type 'y' and press enter. **
{% endhint %}
It will return Transaction hash Like this

```
code: 0
codespace: ""
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: 3068ED7C9867D9DC926A200363704715AE9470EE73452324A32C2583E62B1D79
```


