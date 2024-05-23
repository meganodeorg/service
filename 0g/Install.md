# Airchains

## Recommended Hardware Requirements

|   SPEC      |       Recommend          |
| :---------: | :-----------------------:|
|   **CPU**   |        8 Cores           |
|   **RAM**   |        64 GB              |
| **Storage** |        1 TB            |
| **NETWORK** |        100 Mbps            |
|   **OS**    |        Ubuntu 22.04      |
|   **Port**  |        26657             | 

## install go, if needed

```
cd $HOME
VER="1.22.3"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bashrc
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```



## set some variables

```
echo "export WALLET="wallet"" >> $HOME/.bash_profile
echo "export MONIKER="meganode"" >> $HOME/.bash_profile
echo "export CHAIN_ID="zgtendermint_16600-1"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## build binaries
#### download binary
```
git clone https://github.com/0glabs/0g-chain.git
cd 0g-chain
git checkout v0.1.0
make install
```
#### prepare binaries for cosmovisor
```
mkdir -p $HOME/.0gchain/cosmovisor/genesis/bin
mv /root/go/bin/0gchaind $HOME/.0gchain/cosmovisor/genesis/bin/
```
#### create symlinks
```
sudo ln -s $HOME/.0gchain/cosmovisor/genesis $HOME/.0gchain/cosmovisor/current -f
sudo ln -s $HOME/.0gchain/cosmovisor/current/bin/0gchaind /usr/local/bin/0gchaind -f
```

## cosmovisor setup
```
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.5.0
```

## create service
```
sudo tee /etc/systemd/system/0gchaind.service > /dev/null << EOF
[Unit]
Description=0g node service
After=network-online.target
 
[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.0gchain"
Environment="DAEMON_NAME=0gchaind"
Environment="UNSAFE_SKIP_BACKUP=true"
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:$HOME/.0gchain/cosmovisor/current/bin"
 
[Install]
WantedBy=multi-user.target
EOF
```

## initialize node
```
0gchaind init $MONIKER --chain-id $CHAIN_ID 
```
## download genesis
```
wget -O $HOME/.0gchain/config/genesis.json https://github.com/0glabs/0g-chain/releases/download/v0.1.0/genesis.json
```

## configure seeds
```
sed -i -e "s|^seeds *=.*|seeds = \"449297568d9d6d4aa51a93f7a1b1e92e1ec38619@65.108.242.9:26656\"|" $HOME/.0gchain/config/config.toml
```

## config pruning
```
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.0gchain/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.0gchain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"50\"/" $HOME/.0gchain/config/app.toml
```

## set minimum gas price, enable prometheus and disable indexing
```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0ua0gi"|g' $HOME/.0gchain/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.0gchain/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.0gchain/config/config.toml
```

## enable and start service
```
sudo systemctl daemon-reload
sudo systemctl enable 0gchaind
sudo systemctl restart 0gchaind && sudo journalctl -u 0gchaind -f
```
