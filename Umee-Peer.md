<h1 align="center"> State-Sync Peer for Umee. </h1>
To synchronize, you can use our peer by adding it to the config.toml file.

```
ef6486c7cb39aa8b33a7f72ba9953d4ed039a624@188.172.228.225:26626
```
To add the peer, you can use the following instructions:
```
PEERS=ef6486c7cb39aa8b33a7f72ba9953d4ed039a624@188.172.228.225:26626
sed -i.bak -e "s/^persistent_peers =./persistent_peers = "$PEERS"/" $HOME/.umee/config/config.toml
```

Alternatively, you can add it manually.
Open the config.toml file with the nano editor.
```
nano .umee/config/config.toml
```
Add the peer YTWOFUND to the "persistent_peers" line.
