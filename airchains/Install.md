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
[ ! -f ~/.bashrc] && touch ~/.bashrc
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bashrc
source $HOME/.bashrc
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```
## set some variables

```
echo "export WALLET="wallet"" >> $HOME/.bash_profile
echo "export MONIKER=<Your Moniker>" >> $HOME/.bash_profile
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
````
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

## enable service
```
sudo systemctl daemon-reload
sudo systemctl enable junctiond
```

