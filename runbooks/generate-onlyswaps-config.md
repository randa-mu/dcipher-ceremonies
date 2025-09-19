# Generating an ONLYswaps configuration file

## Prerequisites
- you've successfully completed [the ceremony](./run-ceremony-operator.md) runbook
- you have received a final keyshare.pub file from the ceremony containing your public key (amongst others)
- you've installed the latest [onlyswaps-verifier CLI](./installing-onlyswaps-verifier.md)

## Steps
1. Identify the paths for all your key material, you will need the following files:
- your operator private key, generated during [the key generation runbook](./operator-key-generation.md)
- the group.toml file, shared to you by the leader [before the ceremony](./pre-ceremony-operator.md) 
- the multiaddr your verifier will run on. It should be routable by the multiaddr you chose [before the ceremony](./pre-ceremony-operator.md), but likely will bind a more local interface behind a reverse proxy or similar (i.e. starts with `/ip4/0.0.0.0`)
- your member ID, identified in the [first stages of the ceremony](./run-ceremony-operator.md)
- the public keyshare file, created [during the ceremony](./run-ceremony-operator.md)
- the private keyshare file, created [during the ceremony](./run-ceremony-operator.md)
- (optional) the address of the router contract for ONLYswaps. There is a default one embedded in the configuration process, but if it's changed you may need a new one.

2. Run the following command, replacing with the relevant paths from above:
```bash
onlyswaps-verifier generate-config \
  --private /path/to/longterm/private/key \       
  --group /path/to/group.toml \
  --adkg-public /path/to/public/keyshare \
  --adkg-private /path/to/private/keyshare \
  --multiaddr "/ip4/0.0.0.0/tcp/1234" \
  --member-id $SOME_MEMBER_ID
```

3. Store the output
You should see an output in your console similar to the following:
```
[agent]
healthcheck_listen_addr = "0.0.0.0"
healthcheck_port = 8081
log_level = "debug"
log_json = true

[[networks]]
chain_id = 43113
rpc_url = "wss://avalanche-fuji-c-chain-rpc.publicnode.com/"
private_key = "0x0000000000000000000000000000000000000000000000000000000000000001"
router_address = "0x3dd1a497846d060dce130b67b22e1f9dee18c051"
should_write = false
request_timeout = "5s"

[[networks]]
chain_id = 84532
rpc_url = "wss://base-sepolia-rpc.publicnode.com/"
private_key = "0x0000000000000000000000000000000000000000000000000000000000000001"
router_address = "0x3dd1a497846d060dce130b67b22e1f9dee18c051"
should_write = false
request_timeout = "5s"

[libp2p]
secret_key = "CAESQGRWsEjWcr1qGPz/X3rPomvRPSYXoOEKCBGC19aF3DyAfHkRgFT/HVqC840jXoEM/C34EkpM6VOpvcZvMZI1SxE="
multiaddr = "/ip4/127.0.0.1/tcp/8080"

[committee]
member_id = 1
secret_key = "0x063a308b66e27191366e937538e22fa5b279f3682229c85a68c17b4eb33656f7"
t = 1
n = 4

[[committee.members]]
member_id = 1
bls_pk = "yr9ORCL6qqe1XiJPJ+SkPy5lYo6dhwECo3dK7i0Db0Qd0IM9As1ePHAx88uXgjAT4+xDrKrsW9rjRsErPygiVQ=="
address = "/ip4/127.0.0.1/tcp/8001"
peer_id = "12D3KooWJCFoAiTaoiiqsemhYpFaS9yLb5FW2Jo4c4TViurpVFRz"

[[committee.members]]
member_id = 2
bls_pk = "gJP9lJVz72EycZTVIbNC7fvPulyfwu5XmOxZhtS/r8slXYwMB0O4lUkiT78VTdiAdGA3man8j4hQDI/nRTbvQw=="
address = "/ip4/127.0.0.1/tcp/8002"
peer_id = "12D3KooWJenvgPUcXwZGs311iQH7sNhskZm6EWssdcQGY5RpBZaD"

[[committee.members]]
member_id = 3
bls_pk = "oj3mtJVsf/3Kc/l3G0gB/tQggvFa8o9KnYlsCRSG+JkVi/pxo73hsLtSQ+szT9LH7TBjgfYc5+NuuSDdm6OsUA=="
address = "/ip4/127.0.0.1/tcp/8003"
peer_id = "12D3KooWKKEDrbCXr8uTWUj8sQ3KzafR3A5TkkDBp91ieF6YkkDQ"

[[committee.members]]
member_id = 4
bls_pk = "zSFEDijpgjEJvw8E/8XlkXKAbKTo1aMCp6Zg6pYGpFEZvPbLUr9qzJB0+uRiClI+qSvk0Q2jNX3Gs3nG55GQSg=="
address = "/ip4/127.0.0.1/tcp/8004"
peer_id = "12D3KooWRJPmAZLrUdM7nUsmdisDNgDLWPedMMCEtD2pQosLU31h"
```

Put it in a file for use with the onlyswaps verifier!
