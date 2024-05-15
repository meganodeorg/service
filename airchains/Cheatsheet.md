# Cheat sheet

### key management
#### add new wallet
```
junctiond keys add $WALLET
```
#### restore wallet
```
junctiond keys add $WALLET --recover
```

#### check balances of wallet
```
junctiond q bank balances $(junctiond keys show $WALLET -a)
```

### management validator
Before executing the command `junctiond tx staking create-validator path/to/validator.json --from keyname`, you need to create a `validator.json` file with the following details. Below is an example:

To obtain the pubkey, you can use the command:

```
junctiond comet show-validator
```



