# RPC,API,gRPC and snapshot

#### RPC: 
```junction-rpc.meganode.org```
#### API: 
```junction-api.meganode.org```
#### gRPC: 
```junction-grpc.meganode.org```

## snapshot
```
sudo systemctl stop junctiond

cp $HOME/.junction/data/priv_validator_state.json $HOME/.junction/priv_validator_state.json.backup

rm -rf $HOME/.junction/data $HOME/.junction/wasmPath

curl https://testnet-files.itrocket.net/airchains/snap_airchains.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.junction

mv $HOME/.junction/priv_validator_state.json.backup $HOME/.junction/data/priv_validator_state.json

sudo systemctl restart junctiond && sudo journalctl -u junctiond -f
```

## state sync

```
sudo systemctl stop junctiond

cp $HOME/.junction/data/priv_validator_state.json $HOME/.junction/priv_validator_state.json.backup
junctiond tendermint unsafe-reset-all --home $HOME/.junction

peers="47f61921b54a652ca5241e2a7fc4ed8663091e89@airchains-testnet-peer.itrocket.net:19656,e78a440c57576f3743e6aa9db00438462980927e@5.161.199.115:26656,747b20b00224128bb3a3022cfa557fa105a8e41d@84.247.140.127:43456,f786dcc80601ddd33ba98c609795083ba418d740@158.220.119.11:43456,0b1159b05e940a611b275fe0006070439e5b6e69@[2a03:cfc0:8000:13::b910:277f]:13756,c8f6b1a795a6d9cd2ec39faf277163a9711fc81b@38.242.194.19:43456,552d2a5c3d9889444f123d740a20237c89711109@109.199.96.143:43456,cc27f4e54a78b950adaf46e5413f92f5d53d2212@209.126.86.186:43456,f5b69a02abeb3340ccd266f049ed6aabc7c0ea88@94.72.114.150:43456,db38d672f66df4de01b26e1fa97e1632fbfb1bdf@173.249.57.190:26656,d9a5e20668955bdd5c2fc28cffd6f06e23794638@[2a01:4f8:10a:3a51::2]:43456"  
SNAP_RPC="https://airchains-testnet-rpc.itrocket.net:443"

sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.junction/config/config.toml 

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height);
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000));
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) 

echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH && sleep 2

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ;
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ;
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ;
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ;
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.junction/config/config.toml

mv $HOME/.junction/priv_validator_state.json.backup $HOME/.junction/data/priv_validator_state.json

sudo systemctl restart junctiond && sudo journalctl -u junctiond -f
```




