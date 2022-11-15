<p style="font-size:14px" align="right">
<a href="https://t.me/L0vd_staking" target="_blank">Join our telegram <img src="https://raw.githubusercontent.com/L0vd/screenshots/main/Telegram_logo.png" width="30"/></a>
<a href="https://l0vd.com/" target="_blank">Visit our website <img src="https://raw.githubusercontent.com/L0vd/screenshots/main/L0vd.png" width="30"/></a>
</p>

# Nibiru State Sync

## Info
#### Public RPC endpoint: http://135.181.178.53:28657/
#### Public API: http://135.181.178.53:28317/

## Guide to sync your node using State Sync:

### Copy the entire command
```
sudo systemctl stop nibid
SNAP_RPC="http://135.181.178.53:28657"; \
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash); \
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.nibid/config/config.toml

peers="6692cf5cc5ad3568d3195a5b009f5d6b05ccfc82@135.181.178.53:28656" \
&& sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nibid/config/config.toml 

nibid tendermint unsafe-reset-all --home ~/.nibid && sudo systemctl restart nibid && \
journalctl -u nibid -f --output cat
```

### Turn off State Sync Mode after synchronization
```
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1false|" $HOME/.nibid/config/config.toml
```


