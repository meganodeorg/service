# RPC Node

## 0G Galileo v3 testnet node guide

## Recommended Hardware Requirements

|     SPEC    |        Recommend       |
| :---------: | :--------------------: |
|   **CPU**   |         8 Cores        |
|   **RAM**   |          64 GB         |
| **Storage** |          4 TB          |
| **NETWORK** |        100 Mbps        |
|    **OS**   | Ubuntu 22.04 or latest |

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

## binaries

**download binary**

```
wget https://github.com/0glabs/0gchain-ng/releases/download/v1.0.1/galileo-v1.0.1.tar.gz
tar -xzvf galileo-v1.0.1.tar.gz -C ~
cd galileo
```

## Initialize Geth

**Init Geth**

```
./bin/geth init --datadir ~/.ethereum ./genesis.json
```

**Copy jwt-secret.hex and geth-config.toml**

```
cp ~/galileo/jwt-secret.hex ~/.ethereum
cp ~/galileo/geth-config.toml ~/.ethereum
```

## Initialize 0gchaind

**Init Node**

```
./bin/0gchaind init MEGANODE --chain-id beacon-kurtosis-80087
```

**Copy genesis.json**

```
rm ~/.0gchaind/config/genesis.json
cp ~/galileo/0g-home/0gchaind-home/config/genesis.json ~/.0gchaind/config
```

**Copy jwt-secret.hex and kzg-trusted-setup.json**

```
cp ~/galileo/jwt-secret.hex ~/.0gchaind/config
cp ~/galileo/kzg-trusted-setup.json ~/.0gchaind/config
```

**Edit app.toml with extract path**

```
sed -i \
  -e 's|^jwt-secret-path *=.*|jwt-secret-path = "/root/.0gchaind/config/jwt-secret.hex"|' \
  -e 's|^trusted-setup-path *=.*|trusted-setup-path = "/root/.0gchaind/config/kzg-trusted-setup.json"|' \
  ~/.0gchaind/config/app.toml
```

**Set seeds for node**

```
sed -i -e "s|^seeds *=.*|seeds = \"b30fb241f3c5aee0839c0ea55bd7ca18e5c855c1@8.218.94.246:26656\"|" $HOME/.0gchaind/config/config.toml
```

## create service

**0gchaind.service**

```
sudo tee /etc/systemd/system/0gchaind.service > /dev/null << EOF
[Unit]
Description="0g node"
After=network-online.target

[Service]
User=root
ExecStart= /bin/bash -c 'CHAIN_SPEC=devnet /root/galileo/bin/0gchaind start --home ~/.0gchaind'
Restart=always
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

**geth.service**

```
sudo tee /etc/systemd/system/geth.service > /dev/null << EOF
[Unit]
Description="GETH"
After=network-online.target

[Service]
User=root
ExecStart=/root/galileo/bin/geth --config /root/.ethereum/geth-config.toml --datadir /root/.ethereum --networkid 80087 --authrpc.jwtsecret /root/.ethereum/jwt-secret.hex
Restart=always
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

## enable and start service

**start geth**

```
sudo systemctl daemon-reload
sudo systemctl enable geth
sudo systemctl start geth
```

**start 0gchaind**

```
sudo systemctl daemon-reload
sudo systemctl enable 0gchaind
sudo systemctl start 0gchaind
```

## Monitor service

**monitor logs geth**

```
sudo journalctl -fu geth -o cat
```

**monitor logs 0gchaind**

```
sudo journalctl -fu 0gchaind -o cat
```
