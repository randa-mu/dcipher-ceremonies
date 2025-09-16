# dcipher testnet ceremony: 2025-09-16

## Version
enter a tag, commit, or branch here

## Functionality under test
- ADKG-CLI
- ONLYswaps configuration generator

## Participants
- Randamu
- ATA Network
- Storswift
- IPFS Force
- Emerald Onion

## Scheme 
Once the leader has created the scheme for the ceremony, put it here

```toml
app_name = "dcipher-testnet"
curve_id = "Bn254G1"
hash_id = "Keccak256"
adkg_version = "v0.1"
generator_g = "7YJERmicw+ViR4UEOBNRsutI+TAFzkJvUTw+HcON7zU="
generator_h = "pkFRH8H+G1oQ8QwBySV9EXRStnDxC5BpzpQudrXcLLw="
adkg_scheme_name = "DXKR23-Bn254G1-Keccak256"
output_generator = "mY6Tk5INSDpyYL+3MftdJfGqSTM1qecSl+SFt67zEsIYAN7vEh8edkJqAGZeXER5Z0Mi1Pde2t1G3r1c2ZL27Q=="

```

## Group 
Once the network have generated their libp2p and bn254 keys, create a group file with the public key material and put it here

```toml
n = 7
t = 2
t_reconstruction = 3
start_time = "2025-09-16T14:00:00Z"

[[nodes]]
id = 1
multiaddr = "/dns/dkg-testnet1.dcipher.network/tcp/8080"
adkg_pk = "yQw9eEkBFnU+C6m56oxOzoFMUsLdmwW+cwxQW3Yw+FQ="
peer_id = "12D3KooWCrQpXmCUgY3vFWhhVVnFAeDvYWr7zdYK8jE91Y9yBkuc"

[[nodes]]
id = 2
multiaddr = "/dns/dkg-testnet2.dcipher.network/tcp/8080"
adkg_pk = "nsxiwSTOl40ngje5NjYYRcZ+89Hbd9wHRfGgZTQ6Nyk="
peer_id = "12D3KooWNhSwgtWhSZQfqtNdFce2jsnXfpF4EB6YDpGoZJFm6aJJ"

[[nodes]]
id = 3
multiaddr = "/dns/dciphertest.ata.network/tcp/7777"
adkg_pk = "xISn+nUM+cWMXXolDSg9pNmkzWItyXS0X6qE7QvutSg="
peer_id = "12D3KooWKouthHQeuYF4PgorkF1fEYsdYhEC9sLPapR2sHZ5Pomh"

# IPFSFORCE
[[nodes]]
id = 4
multiaddr = "/ip4/47.97.251.33/tcp/7777"
adkg_pk = "2jhVMLwlDypzszZnN4Pi4aaNMMz6ayMAnyu0yRkbuks="
peer_id = "12D3KooWRJxAn5DWmSa67UcComW5tBNERVmjmnnCFUYrAAxhXajZ"

# Storswift
[[nodes]]
id = 5
multiaddr = "/ip4/47.104.170.105/tcp/7890"
adkg_pk = "x5tOBOX27WmfW1BVGNROMEPddxHZOcKwhEB5SparvQE="
peer_id = "12D3KooWAbgcdgZZDQbMC2WN6ZvuKmNRLTpo52QLfMVHqtsjzi5h"

# Emerald Onion
[[nodes]]
id = 6
multiaddr = "/ip4/5.42.201.106/tcp/7777"
adkg_pk = "ovcshiBUOTYyeQChFB8g+AF30sn38D5KQspGKyBpH8g="
peer_id = "12D3KooWHeGyx3cUfkdbBCzUytTRUKuxdJ8j2V3Xf2Qcxghd8UkF"

# DIA Data
[[nodes]]
id = 7
multiaddr = "/dns/drand.diadata.org/tcp/4242"
adkg_pk = "4H97WjJsuKRnNACoGbnIVrYs4kzfB2VmlmK4bhDDThg="
peer_id = "12D3KooWCpsdsUvGkqajWcgU6v62R5edN8UG5QrDvfKMTZiwBBV6"
```

## Log
Log any timings, actions, issues, or points to note here


