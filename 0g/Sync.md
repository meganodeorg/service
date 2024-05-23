# RPC,API,gRPC and snapshot

#### RPC: 
```0g-rpc.meganode.org```
#### API: 
```0g-api.meganode.org```
#### gRPC: 
```0g-grpc.meganode.org```

## snapshot
```
sudo systemctl stop junctiond

cp $HOME/.0gchain/data/priv_validator_state.json $HOME/.0gchain/priv_validator_state.json.backup

rm -rf $HOME/.0g-chain/data $HOME/.junction/wasmPath

curl https://0g-snapshot.meganode.org/data.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.0gchain

mv $HOME/.0gchain/priv_validator_state.json.backup $HOME/.0gchain/data/priv_validator_state.json

sudo systemctl restart 0gchaind && sudo journalctl -u 0gchaind -f
```

## state sync

```
sudo systemctl stop 0gchaind

cp $HOME/.0gchain/data/priv_validator_state.json $HOME/.0gchain/priv_validator_state.json.backup
junctiond tendermint unsafe-reset-all --home $HOME/.0gchain

peers="e78a440c57576f3743e6aa9db00438462980927e@5.161.199.115:26656,747b20b00224128bb3a3022cfa557fa105a8e41d@84.247.140.127:43456,f786dcc80601ddd33ba98c609795083ba418d740@158.220.119.11:43456,0b1159b05e940a611b275fe0006070439e5b6e69@[2a03:cfc0:8000:13::b910:277f]:13756,c8f6b1a795a6d9cd2ec39faf277163a9711fc81b@38.242.194.19:43456,552d2a5c3d9889444f123d740a20237c89711109@109.199.96.143:43456,cc27f4e54a78b950adaf46e5413f92f5d53d2212@209.126.86.186:43456,f5b69a02abeb3340ccd266f049ed6aabc7c0ea88@94.72.114.150:43456,db38d672f66df4de01b26e1fa97e1632fbfb1bdf@173.249.57.190:26656,d9a5e20668955bdd5c2fc28cffd6f06e23794638@[2a01:4f8:10a:3a51::2]:43456"  
SNAP_RPC="https://0gchain-rpc.meganode.org"

sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.0gchain/config/config.toml 

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000));
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) 

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH && sleep 2

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ;
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ;
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ;
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ;
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.0gchain/config/config.toml

mv $HOME/.0gchain/priv_validator_state.json.backup $HOME/.0gchain/data/priv_validator_state.json

sudo systemctl restart 0gchaind && sudo journalctl -u 0gchaind -f
```




