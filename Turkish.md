![2](https://github.com/user-attachments/assets/8381330e-2d35-4e14-a549-dababb0aa4bf)



[Website](https://axone.xyz/)<br>
[Faucet](https://faucet.vana.org/moksha/)<br>
[Discord](https://discord.gg/UgW9xu92/)<br>
[Docs](https://docs.axone.xyz/technical-documentation/overview/)<br>


# Sistem Gereksinimleri
| Bileşenler	 | Minimum Gereksinimler | 
| ------------ | ------------ |
| CPU | 4 Core |
| RAM | 8 GB RAM |
| Storage | 200 GB Nvme |



# Sunucuyu güncelleyelim

```
sudo apt update && sudo apt upgrade -y
```


# GO'yu indirelim

```
sudo rm -rvf /usr/local/go/
wget https://golang.org/dl/go1.23.1.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.23.1.linux-amd64.tar.gz
rm go1.23.1.linux-amd64.tar.gz
```

# Yapılandırmalar vs.

```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```

# Cosmovisor'ün son sürümünü kuralım

```
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.6.0
```



# Son paylaşılan Binary'i indirelim

```
sudo apt install curl git jq lz4 build-essential
```

```
git clone https://github.com/axone-protocol/axoned axone
cd axone
git checkout v10.0.0
make install
```



# Moniker yaz?

```
axoned init MONIKER --chain-id axone-dentrite-1
```


# Genesis 

```
curl -L -o genesis.json 'https://drive.google.com/uc?id=1VUOkhRY2kVWFsbQbo8RrK87_H1E3km-s'
mv genesis.json ~/.axoned/config
```

# Addrbook

```
curl -L -o addrbook.json 'https://drive.google.com/uc?id=1VUOkhRY2kVWFsbQbo8RrK87_H1E3km-s'
mv addrbook.json ~/.axoned/config
```

# Seed
```
sed -i 's/seeds = ""/seeds = "ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@testnet-seeds.polkachu.com:17656"/' ~/.axoned/config/config.toml
```


# Peers
```
PEERS=8e7dc1bc3c9dc2106e077e6bbd48f3790dd5c934@144.91.115.146:26656,9614c853f70a0010215587a31677b99144e96507@152.228.211.19:26656,e2eeb94a734de5d4cdca408ffb3ef675183105d5@65.109.83.40:29856,8ea05a621d5fdfbda4192ae8369f289ef04c04ba@78.46.74.23:25656,e8838b99dabdbc60d776b359f9929ecbaf7ba82f@65.109.93.58:20056
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.axoned/config/config.toml
```

# Pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "50"|' \
  $HOME/.axoned/config/app.toml
```

### Bunlar Axone'un kullandığı portlardır. İhtiyacınıza göre değiştirmelisiniz 🧨

<img width="687" alt="Ekran Resmi 2024-09-30 15 01 56" src="https://github.com/user-attachments/assets/0bec540c-8c40-4b72-9131-31a9ded3119e">



# Şimdi service dosyasını oluşturalım

```
sudo tee /etc/systemd/system/axone.service > /dev/null << EOF
[Unit]
Description=axone node
After=network-online.target

[Service]
User=$USER
ExecStart=/root/go/bin/axoned start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.axoned"
Environment="DAEMON_NAME=axoned"
Environment="UNSAFE_SKIP_BACKUP=true"
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:$HOME/.axoned/cosmovisor/current/bin"

[Install]
WantedBy=multi-user.target
EOF
```


# Yakıt dolumu yaptığımıza göre

```
sudo systemctl daemon-reload
sudo systemctl start axone.service
```


# Loglar gelmiş olmalı


```
journalctl -u axone.service -f
```



# Snapshot

```
wget -O axone_311246.tar.lz4 https://snapshots.polkachu.com/testnet-snapshots/axone/axone_311246.tar.lz4 --inet4-only
```


```
sudo systemctl stop axone.service
```

```
cp ~/.axoned/data/priv_validator_state.json  ~/.axoned/priv_validator_state.json
```

```
lz4 -c -d axone_311246.tar.lz4  | tar -x -C $HOME/.axoned
```


```
rm -v axone_311246.tar.lz4
```

```
cp ~/.axoned/priv_validator_state.json  ~/.axoned/data/priv_validator_state.json
```

```
sudo systemctl restart axone.service
```

```
journalctl -u axone.service -f
```

# Yeni bir cüzdan oluşturmak için

```
axoned keys add wallet
```

# Cüzdanınızı içe aktarmak için

```
axoned keys add wallet --recover
```


# Validator adresi talep etmek için: (Bu işlem zorunludur çünkü bize verilen cüzdan yapısı farklı) 🧨

```
axoned tendermint show-validator
```

```
cd
```

```
nano validator.json
```

# Bilgileri düzenledikten sonra, CommandX+CommandY+Enter

```
{
  "pubkey": ,
  "amount": "1000000uaxone",
  "moniker": "test",
  "identity": "YOUR_KEYBASE_ID",
  "website": "YOUR_WEBSITE_URL",
  "security": "YOUR_SECURITY_EMAIL",
  "details": "YOUR_DETAILS",
  "commission-rate": "0.05",
  "commission-max-rate": "0.20",
  "commission-max-change-rate": "0.05",
  "min-self-delegation": "1"
}
```


# Sanırım başardın ✅ 😁

```
axoned tx staking create-validator ~/validator.json \
--chain-id axone-dentrite-1 \
--from wallet \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices "0uaxone" \
-y
```

# İşlemler tamamdır. Repoya ufak bir yıldız bırakırsan sevinirim. 🐅



### Faydalı olabilecek birkaç komut

# Kendi adresine delege

```
/root/go/bin/axoned tx staking delegate <TO_VALOPER_ADDRESS> 1000000uaxone --from wallet --chain-id axone-dentrite-1 --gas-adjustment 1.4 --gas auto --gas-prices 0uaxone -y
```

# Farklı bir adrese delege

```
/root/go/bin/axoned tx staking delegate axonevaloper1xxxxxxx 1000000uaxone --from wallet --chain-id axone-dentrite-1 --gas-adjustment 1.4 --gas auto --gas-prices 0uaxone -y
```


# Hapishaneden kaçmak istersen

```
/root/go/bin/axoned tx slashing unjail --from <your_wallet_name> --chain-id axone-dentrite-1 --gas-adjustment 1.4 --gas auto --gas-prices 0uaxone -y
```


### Keyifli günler dilerim. 🐅

