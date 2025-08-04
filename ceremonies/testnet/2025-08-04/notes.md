# dcipher testnet ceremony: 2025-07-29

## Version
v0.1.0-testnet.0

## Functionality under test

- building and running binaries from the dcipher repo
- initial distributed key generation

## Participants
- Automata Network
- DIA Data
- QRL Foundation
- Protocol Labs
- Storswift
- Randamu

## Scheme 

```toml
app_name = "dcipher-testnet"
curve_id = "Bn254G1"
hash_id = "Keccak256"
adkg_version = "v0.1"
adkg_scheme_name = "DYX20-Bn254G1-Keccak256"
generator_g = "7YJERmicw+ViR4UEOBNRsutI+TAFzkJvUTw+HcON7zU="
generator_h = "gAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAE="
```

## Group 
Once the network have generated their libp2p and bn254 keys, create a group file with the public key material and put it here
```
adkg_scheme_name = "DYX20-Bn254G1-Keccak256"
genesis_timestamp = 1754299900
group_pk = "x0S2fTdbOYvPRn9bxG/08bS0CndoLLo7dfDsKvX6thw="

[[node_pks]]
id = 1
pk = "nqOHbB8vkV6DT42B7FE6fTD74HIbd1c/cSZAyEejB6w="
peer_id = "12D3KooWNynkcrc6tS9EBQMe2J2BsebZq7bU4mgXMatDRer8eDbb"
multiaddr = "/dns/drand.diadata.org/tcp/4242"

[[node_pks]]
id = 2
pk = "nVE9g/pdzUlrR1yQ31ZIFTI5cuJR/VxjleZTBI5UUUo="
peer_id = "12D3KooWMTwoV3DPt1GUJfZdyDvmX4ankVswnPDK92sB9kePXukr"
multiaddr = "/dns/pl1-rpc.testnet.drand.sh/tcp/8080"

[[node_pks]]
id = 3
pk = "zUC725BhvmwtYPkbzdzeO+usydjv6MG0oA14lH+bQhw="
peer_id = "12D3KooWHZveoayqsNdUymq2ZGg4T3cqRvweEQgHBjHkNB1Mb5DH"
multiaddr = "/dns/testnet-1.drand.theqrl.org/tcp/4421"

[[node_pks]]
id = 4
pk = "2xQBKgJHL0PAeqV6eFV7dMYJJn8LNtMufbY+5dPtuM0="
peer_id = "12D3KooWDGEa22QJC5uoDuKiBDWrXht9PH5fmqbjse15uWywB5Dj"
multiaddr = "/ip4/193.5.234.66/tcp/7777"

[[node_pks]]
id = 5
pk = "jYgBqOXhEZ7TpPcUOnPO6vzHEdwqijdzplbfN02JcDw="
peer_id = "12D3KooWT3SmyBzmT2RY5pZhr5wHSZ5WkA8EwN8UvgUKZxhivcvw"
multiaddr = "/ip4/47.104.170.105/tcp/7890"

[[node_pks]]
id = 6
pk = "3x/9n9NfqwCXom3KgVuopJGJ5+gJdrLlMo1lRjtkUuI="
peer_id = "12D3KooWBvRVuRLWRhyka1APsNg74gry6K9MSYPg4HmNPAjGcSte"
multiaddr = "/dns/dciphertest.ata.network/tcp/7777"
```

## Log
- can't use `/dns/example.org/tcp/1234` as `listen-address` to daemon
    - operators were instructed to use `/ip4/0.0.0.0` instead
- start time was set as `2025-08-04T09:30:00Z` but the nodes slept until `09:31:40`
- drand node was unreachable and failed to join
    - initially it was on testnet1 itself
    - moved it to http-relay.testnet1
    - due to slow build, it started slightly later than other nodes (after the start time) and didn't catch up
- Randamu node used the wrong port for ATA network (443 vs 7777) but still got all the messages fine
    - got a NetworkError wrapping `InvalidData`
