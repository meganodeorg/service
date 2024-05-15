# Airchains

## Recommended Hardware Requirements

|   SPEC      |       Recommend          |
| :---------: | :-----------------------:|
|   **CPU**   |        4 Cores           |
|   **RAM**   |        8 GB              |
| **Storage** |        500 GB            |
| **NETWORK** |        1 Gbps            |
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
echo "export CHAIN_ID="junction"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## build binaries
#### download binary
```
wget https://github.com/airchains-network/junction/releases/download/v0.1.0/junctiond
chmod +x junctiond
```
#### prepare binaries for cosmovisor
```
mkdir -p $HOME/.junction/cosmovisor/genesis/bin
mv junctiond $HOME/.junction/cosmovisor/genesis/bin/
```
#### create symlinks
```
sudo ln -s $HOME/.junction/cosmovisor/genesis $HOME/.junction/cosmovisor/current -f
sudo ln -s $HOME/.junction/cosmovisor/current/bin/junctiond /usr/local/bin/junctiond -f
```

## cosmovisor setup
```
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.5.0
```

## create service
```
sudo tee /etc/systemd/system/junctiond.service > /dev/null << EOF
[Unit]
Description=airchains node service
After=network-online.target
 
[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.junction"
Environment="DAEMON_NAME=junctiond"
Environment="UNSAFE_SKIP_BACKUP=true"
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:$HOME/.junction/cosmovisor/current/bin"
 
[Install]
WantedBy=multi-user.target
EOF
```

## initialize node
```
junctiond init $MONIKER --chain-id $CHAIN_ID 
```
## download genesis
```
wget -O $HOME/.junction/config/genesis.json https://github.com/airchains-network/junction/releases/download/v0.1.0/genesis.json
```

## configure seeds
```
sed -i -e "s|^seeds *=.*|seeds = \"449297568d9d6d4aa51a93f7a1b1e92e1ec38619@65.108.242.9:26656\"|" $HOME/.junction/config/config.toml
```

## config pruning
```
sed -i -e "s/^pruning *=.*/pruning = \"custom\"/" $HOME/.junction/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.junction/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"50\"/" $HOME/.junction/config/app.toml
```

## set minimum gas price, enable prometheus and disable indexing
```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.001amf"|g' $HOME/.junction/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.junction/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.junction/config/config.toml
```

## enable and start service
```
sudo systemctl daemon-reload
sudo systemctl enable junctiond
sudo systemctl restart junctiond && sudo journalctl -u junctiond -f
```
