# dcipher testnet ceremony: 2025-09-16

## Version
v0.1.0-testnet.2

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
adkg_pk = "oxNFSu+vFO8/u8ZuVIpcwI85vt7EYRvVLjB37Nkkbcs="
peer_id = "12D3KooWT1acekTAQNuQT39N95dndESfDwWDHFMQLBQntBjQft9M"
```

## Log
All times in UTC

- 09:29 group file shared
- 11:24 most parties running their share command - general connection issues with Randamu nodes and DIA
- 12:05 DIA reporting `NoAddress` error for some nodes (later identified to be because other nodes wouldn't route to their invalid PeerId)
- 12:45 we identify that the DIA peerID is incorrect and a new group file is issued
```
2025-09-16T12:24:42.340494Z  WARN dcipher_network::transports::libp2p::dialer: Failed to dial peer error=WrongPeerId { obtained: PeerId("12D3KooWT1acekTAQNuQT39N95dndESfDwWDHFMQLBQntBjQft9M"), endpoint: Dialer { address: /dns/drand.diadata.org/tcp/4242/p2p/12D3KooWCpsdsUvGkqajWcgU6v62R5edN8UG5QrDvfKMTZiwBBV6, role_override: Dialer, port_use: Reuse } } peer_id=12D3KooWCpsdsUvGkqajWcgU6v62R5edN8UG5QrDvfKMTZiwBBV6 short_id=PartyId(7)
```
- 12:50 DIA asking for list of IPs for allowlist - probably should have prepared that in advance
- 13:18 DIA connecting to every node except Storswift
- 13:26 Storswift afk and unable to restart with the updated group file - let's see what happens!
- 14:00 DKG kicked off; Randamu nodes successfully claim to have received a secret key, but with some interesting errors
- 14:00 priv and pub shares both have data in them
Funny errors from Randamu 1:
```
2025-09-16T14:00:00.000989Z  INFO adkg_cli::adkg_dxkr23: Executing ADKG with a timeout of 1h
2025-09-16T14:00:01.643451Z  WARN adkg::vss::acss::hbacss0::handlers: Node `1` cannot validate implicate from node `7` (invalid proof)
2025-09-16T14:00:03.019706Z  WARN adkg::rbc::r4::handlers: Node `1` refused proposal proposal_sender=PartyId(3)
2025-09-16T14:00:03.036496Z  WARN adkg::rbc::r4::handlers: Node `1` refused proposal proposal_sender=PartyId(4)
2025-09-16T14:00:03.126695Z  WARN adkg::rbc::r4::handlers: Node `1` refused proposal proposal_sender=PartyId(5)
2025-09-16T14:00:10.093919Z  INFO adkg_cli::adkg_dxkr23: Successfully obtained secret key & output from ADKG used_sessions=[SessionId(4), SessionId(7), SessionId(1), SessionId(6), SessionId(2), SessionId(3)]
2025-09-16T14:00:10.095442Z  INFO adkg_cli::adkg_dxkr23: Running ADKG until grace period of 5m
```




