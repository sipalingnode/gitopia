<p style="font-size:14px" align="right">
<a href="https://t.me/autosultan_group" target="_blank">Join our telegram <img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/></a>
</p>

<p align="center">
  <img height="150" height="auto" src="https://user-images.githubusercontent.com/38981255/201691739-df61f26e-5c05-46b5-8ce6-0981a6615d60.PNG">
</p>

# Tutorial Gitopia Node

## Explorer : https://gitopia.explorers.guru/

## Spek VPS
|  Komponen |  Persyaratan Minimum |
| ------------ | ------------ |
| CPU  | 4 or more physical CPU cores  |
| RAM | 8GB RAM |
| Penyimpanan  | At least 100GB of SSD disk storage |

## Install Node
```
wget -O gitopia.sh https://raw.githubusercontent.com/sipalingnode/gitopia/main/gitopia.sh && chmod +x gitopia.sh && ./gitopia.sh
```
```
source $HOME/.bash_profile
```
## Add Snapshot
```
sudo systemctl stop gitopiad
cp $HOME/.gitopia/data/priv_validator_state.json $HOME/.gitopia/priv_validator_state.json.backup
gitopiad tendermint unsafe-reset-all --home $HOME/.gitopia

STATE_SYNC_RPC=https://gitopia-testnet.rpc.kjnodes.com:443
STATE_SYNC_PEER=d5519e378247dfb61dfe90652d1fe3e2b3005a5b@gitopia-testnet.rpc.kjnodes.com:41656
LATEST_HEIGHT=$(curl -s $STATE_SYNC_RPC/block | jq -r .result.block.header.height)
SYNC_BLOCK_HEIGHT=$(($LATEST_HEIGHT - 2000))
SYNC_BLOCK_HASH=$(curl -s "$STATE_SYNC_RPC/block?height=$SYNC_BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -e "s|^enable *=.*|enable = true|" $HOME/.gitopia/config/config.toml
sed -i.bak -e "s|^rpc_servers *=.*|rpc_servers = \"$STATE_SYNC_RPC,$STATE_SYNC_RPC\"|" \
  $HOME/.gitopia/config/config.toml
sed -i.bak -e "s|^trust_height *=.*|trust_height = $SYNC_BLOCK_HEIGHT|" \
  $HOME/.gitopia/config/config.toml
sed -i.bak -e "s|^trust_hash *=.*|trust_hash = \"$SYNC_BLOCK_HASH\"|" \
  $HOME/.gitopia/config/config.toml
sed -i.bak -e "s|^persistent_peers *=.*|persistent_peers = \"$STATE_SYNC_PEER\"|" \
  $HOME/.gitopia/config/config.toml
mv $HOME/.gitopia/priv_validator_state.json.backup $HOME/.gitopia/data/priv_validator_state.json

sudo systemctl start gitopiad && journalctl -u gitopiad -f --no-hostname -o cat
```
***kalo udah jalan langsung CTRL+C aja***
## Create Wallet
```
gitopiad keys add $WALLET
```
## Save Info Wallet
```
GITOPIA_WALLET_ADDRESS=$(gitopiad keys show $WALLET -a)
GITOPIA_VALOPER_ADDRESS=$(gitopiad keys show $WALLET --bech val -a)
echo 'export GITOPIA_WALLET_ADDRESS='${GITOPIA_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export GITOPIA_VALOPER_ADDRESS='${GITOPIA_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```
## Claim Faucet
- Import Wallet Kalian ke Kepler
- Open Web : https://gitopia.com/
- Connect Wallet
- Get 10 Tlore Done
## Create Validator
***Sebelum buat validator cek status node dulu kalo dah false boleh lanjut kalo masih true, nunggu false dulu***
```
gitopiad status 2>&1 | jq .SyncInfo
```
***Lanjut buat validator***
```
gitopiad tx staking create-validator \
  --amount 1000000utlore \
  --from $WALLET \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.2" \
  --commission-rate "0.07" \
  --min-self-delegation "1" \
  --pubkey  $(gitopiad tendermint show-validator) \
  --moniker $NODENAME \
  --chain-id $GITOPIA_CHAIN_ID
  ```
  ## Check Validator Key
  ```
  [[ $(gitopiad q staking validator $GITOPIA_VALOPER_ADDRESS -oj | jq -r .consensus_pubkey.key) = $(gitopiad status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
  ```
  ## Get List Validator
  ```
  gitopiad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
  ```
  ## Get currently connected peer list with ids
  ```
  curl -sS http://localhost:${GITOPIA_PORT}657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
  ```
  ## Perintah Berguna bang
  Check Log Node
  ```
  journalctl -fu gitopiad -o cat
  ```
  Start Node
  ```
  sudo systemctl start gitopiad
  ```
  Stop Node
  ```
  sudo systemctl stop gitopiad
  ```
  Restart Node
  ```
  sudo systemctl restart gitopiad
  ```
  ## Node Info
  Synchronization info
  ```
  gitopiad status 2>&1 | jq .SyncInfo
  ```
  Validator info
  ```
  gitopiad status 2>&1 | jq .ValidatorInfo
  ```
  Node info
  ```
  gitopiad status 2>&1 | jq .NodeInfo
  ```
  Show node id
  ```
  gitopiad tendermint show-node-id
  ```
  ## Wallet operations
  List of wallets
  ```
  gitopiad keys list
  ```
  Import Wallet
  ```
  gitopiad keys add $WALLET --recover
  ```
  Delete Wallet
  ```
  gitopiad keys delete $WALLET
  ```
  Cek Saldo Wallet
  ```
  gitopiad query bank balances $GITOPIA_WALLET_ADDRESS
  ```
  Transfer Saldo
  ```
  gitopiad tx bank send $GITOPIA_WALLET_ADDRESS <TO_GITOPIA_WALLET_ADDRESS> 10000000utlore
  ```
  ## Vote
  ```
  gitopiad tx gov vote 1 yes --from $WALLET --chain-id=$GITOPIA_CHAIN_ID
  ```
  ## Staking, Delegation and Rewards
  Delegate stake
  ```
  gitopiad tx staking delegate $GITOPIA_VALOPER_ADDRESS 10000000utlore --from=$WALLET --chain-id=$GITOPIA_CHAIN_ID --gas=auto
  ```
  Redelegate stake from validator to another validator
  ```
  gitopiad tx staking redelegate <srcValidatorAddress> <destValidatorAddress> 10000000utlore --from=$WALLET --chain-id=$GITOPIA_CHAIN_ID --gas=auto
  ```
  Withdraw all rewards
  ```
  gitopiad tx distribution withdraw-all-rewards --from=$WALLET --chain-id=$GITOPIA_CHAIN_ID --gas=auto
  ```
  Withdraw rewards with commision
  ```
  gitopiad tx distribution withdraw-rewards $GITOPIA_VALOPER_ADDRESS --from=$WALLET --commission --chain-id=$GITOPIA_CHAIN_ID
  ```
  ## Validator management
  Edit validator
  ```
  gitopiad tx staking edit-validator \
  --moniker=$NODENAME \
  --identity=<your_keybase_id> \
  --website="<your_website>" \
  --details="<your_validator_description>" \
  --chain-id=$GITOPIA_CHAIN_ID \
  --from=$WALLET
  ```
  Unjail validator
  ```
  gitopiad tx slashing unjail \
  --broadcast-mode=block \
  --from=$WALLET \
  --chain-id=$GITOPIA_CHAIN_ID \
  --gas=auto
  ```
  ## Delete Node
  ```
sudo systemctl stop gitopiad
sudo systemctl disable gitopiad
sudo rm /etc/systemd/system/gitopia* -rf
sudo rm $(which gitopiad) -rf
sudo rm $HOME/.gitopia* -rf
sudo rm $HOME/gitopia -rf
sed -i '/GITOPIA_/d' ~/.bash_profile
```
