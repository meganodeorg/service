# Cheat sheet

### key management
#### #add new wallet
```
0gchaind keys add $WALLET --eth
```
#### #restore wallet
```
0gchaind keys add $WALLET --recover --eth
```

#### #check balances of wallet
```
0gchaind q bank balances $(0gchaind keys show $WALLET -a)
```

### management validator
##### #create validator
```
0gchaind tx staking create-validator \
  --amount=1000000ua0gi \
  --pubkey=$(0gchaind tendermint show-validator) \
  --moniker="$MONIKER" \
  --chain-id=zgtendermint_16600-1 \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --from=$WALLET \
  --gas=auto \
  --gas-adjustment=1.4
```
##### #edit validator
```
0gchaind tx staking edit-validator \
--commission-rate 0.1 \
--new-moniker "$MONIKER" \
--identity "" \
--details "From MegaNode with love ❤️" \
--from $WALLET \
--chain-id zgtendermint_16600-1 \
--fees 200ua0gi \
-y 
```
##### #validator info
```
0gchaind status 2>&1 | jq
```

##### #validator details
```
0gchaind q staking validator $(0gchaind keys show $WALLET --bech val -a) 
```

##### #jail info
```
0gchaind q slashing signing-info $(0gchaind tendermint show-validator) 
```
##### #unjail validator
```
0gchaind tx slashing unjail --from $WALLET --chain-id zgtendermint_16600-1 --gas-adjustment 1.4 --gas auto -y
```
### remove node

```
cd $HOME
sudo systemctl stop 0gchaind
sudo systemctl disable 0gchaind
sudo rm /etc/systemd/system/0gchaind.service
sudo systemctl daemon-reload
sudo rm -f $(which 0gchaind)
sudo rm -rf $HOME/.0gchain
```



