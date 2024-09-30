![2](https://github.com/user-attachments/assets/8381330e-2d35-4e14-a549-dababb0aa4bf)



[Website](https://axone.xyz/)<br>
[Faucet](https://faucet.vana.org/moksha/)<br>
[Discord](https://discord.gg/UgW9xu92/)<br>
[Docs](https://docs.axone.xyz/technical-documentation/overview/)<br>


# System Requirements
| Components	 | Minimum Requirements | 
| ------------ | ------------ |
| CPU | 4 Core |
| RAM | 8 GB RAM |
| Storage | 200 GB Nvme |



# Update the Server

```
sudo apt update && sudo apt upgrade -y
```

```
sudo apt install curl git jq lz4 build-essential
```



# Install the latest version of GO

```
sudo rm -rvf /usr/local/go/
wget https://golang.org/dl/go1.23.1.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.23.1.linux-amd64.tar.gz
rm go1.23.1.linux-amd64.tar.gz
```

# Set The Configurations

```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```

# Install Cosmovisor

```
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.6.0
```



# Pull the Axone binary

```
git clone https://github.com/axone-protocol/axoned.git
cd axoned
git checkout v10.0.0
make build
```



# Your Moniker Name?

```
axoned init MONIKER --chain-id axone-dentrite-1
```


# Download the Genesis file

```
curl -L -o genesis.json 'https://drive.google.com/uc?id=1VUOkhRY2kVWFsbQbo8RrK87_H1E3km-s'
```


# SEED
```
sed -i 's/seeds = ""/seeds = "ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@testnet-seeds.polkachu.com:17656"/' ~/.axoned/config/config.toml
```






