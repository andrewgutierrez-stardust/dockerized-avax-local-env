Originally forked from [Avalanchego-kurtosis](https://github.com/ava-labs/avalanchego-kurtosis) repo, this has been modifed to **just work.**

## Docker Compose

_Configuration that will bootstrap a local Avalanche network using Docker
Compose_

```shell
cd docker-compose
docker-compose pull
docker-compose up
```
NOTE: you can use the '-d' flag, but you will need to wait ~20s after spinning up the nodes to allow bootstrapping to complete.


It will:
* Create 5 instances of `avalanchego:latest` and hook them together to bootstrap a local network
* Ensure you have the `avalanchego:latest` by doing a `docker-compose pull`
* Expose the API ports of the nodes on:

```
localhost:9661 -> node1:9650
localhost:9662 -> node2:9650
localhost:9663 -> node3:9650
localhost:9664 -> node4:9650
localhost:9665 -> node5:9650
```

* Expose the Staking ports of the nodes on:

```
localhost:9671 -> node1:9651
localhost:9672 -> node2:9651
localhost:9673 -> node3:9651
localhost:9674 -> node4:9651
localhost:9675 -> node5:9651
```


# Useful Stuff to do after deploying:
- request bootstrap status of node
- set up metamask with local network and import prefunded private key
- get balance of prefunded key

## ping a node for bootstrap status
```shell
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.isBootstrapped",
    "params": {
        "chain":"X"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9661/ext/info
```


## set up metamask w local network, and import prefunded private key (C Chain)
Add a new network to metamask with following params:
New RPC URL: 		
```shell
http://127.0.0.1:9661/ext/bc/C/rpc
```

ChainID: 					
```shell
43112
```


Symbol: 					
```shell
AVAX
```

Now import the following prefunded key into metamask account to allow signing of transactions:
```shell
0x56289e99c94b6912bfc12adc093c9b51124f0dc54ac7a766b2bc5ccf558d8027
```

\#whalelife

## get balance of prefunded key
```shell
curl --location --request POST '127.0.0.1:9661/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBalance",
    "params": [
        "0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC",
        "latest"
    ],
    "id": 1
}'
```