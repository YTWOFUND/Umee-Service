<h1 align="center"> State-Sync User Guide </h1>

Let's set variables:
```
SNAP_RPC="https://umeemain-rpc.ytwofund.pro"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)
```

Let's check if the data has been fetched:
```
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
```

You should see the latest block height, a height 2000 blocks lower, and a trusted hash in the response.
```
Example: "4161072 4159072 E363899B6C4F60DB21619380F56D483BC269FC91BF13AEF8859309B233F17730"
```

We put the obtained data into config.toml:
```
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.umee/config/config.toml
```
Stop the node:
```
systemctl stop umeed.service
```
Reset the node's data:
```
umeed tendermint unsafe-reset-all --home $HOME/.umee --keep-addr-book
```
Restart the service:
```
systemctl restart umeed.service
```
If everything was done correctly, the synchronization with the network will start, which may not happen immediately, so you need to wait for some time.

You can also use an alternative method and use the [Snapshot](https://github.com/YTWOFUND/Umee-Service/blob/main/Snapshot.md).

If you encounter any issues, please let us know at ytwofund@gmail.com.
