![0G Github Banner](https://github.com/BlockchainsHub/Testnet/assets/77204008/34a32724-b411-41e4-8696-e390dfa01cab)

# 0G Node Guide
This guide will help you in the 0G node installation process.

-----------------------------------------------------------------

## Required Specification
| Required | Specification |
|-|-
| CPU | 4 Cores |
| Memory | 8 GB |
| Storage | 500 GB NVMe SSD |
| Bandwidth | 100mbps |
| OS | Linux |

-----------------------------------------------------------------

## Auto Node Installation
Run the following command to install node automatically.
```bash
curl -o 0g.sh https://gist.githubusercontent.com/botxx15/b122ad56138a9805f54a7dc12c25059f/raw/103f41bf764676a4232df2c51d57311cb4890149/0g.sh && bash 0g.sh
```

> [!NOTE]
> Note: You can clik `ctrl+c` to quit from logs

### Check Sync Status
If the status is `"catching_up": false` it means the node has been synchronized, whereas if the status is `"catching_up": true` it means the node has not finished synchronizing.
```bash
evmosd status | jq .SyncInfo
``` 

### Create Wallet For Validator
When creating a new wallet you will be given a **seed phrase**. **Don't forget to save your seed phrase!**. You will too
immediately given a wallet address in EVM (0x) format, make sure to save this address because it will be used when you
make a faucet request.
```bash
evmosd keys add $WALLET_NAME
echo "0x$(evmosd debug addr $(evmosd keys show $WALLET_NAME -a) | grep hex | awk '{print $3}')"
```
> [!CAUTION]
> **DON'T FORGET TO SAVE YOUR SEED PHRASE!**

### Request Faucet
Please click the button below to request faucet.

<a href="https://faucet.0g.ai/" target="_blank">
  <img src="https://github.com/BlockchainsHub/Testnet/assets/77204008/12866a81-cac7-451a-96b5-4b202e8f1194" alt="Faucet Button" width="150" height="36.94" border="10" />
</a>

### Check Wallet Balance
```bash
evmosd q bank balances $(evmosd keys show $WALLET_NAME -a) 
```
> [!NOTE]
> Note: You will get *100000000000000000aevmos* from the faucet. To make a validator join the active set, you need at least *1000000000000000000aevmos* (**10 times more**).

### Create a Validator
```bash
evmosd tx staking create-validator \
  --amount=10000000000000000aevmos \
  --pubkey=$(evmosd tendermint show-validator) \
  --moniker=$MONIKER \
  --chain-id=$CHAIN_ID \
  --commission-rate=0.05 \
  --commission-max-rate=0.10 \
  --commission-max-change-rate=0.01 \
  --min-self-delegation=1 \
  --from=$WALLET_NAME \
  --identity="" \
  --website="" \
  --details="0G to the moon!" \
  --gas=500000 \
  --gas-prices=99999aevmos \
  -y
```
> [!CAUTION]
> Don't forget to backup the `priv_validator_key.json` file located at $HOME/.evmosd/config/