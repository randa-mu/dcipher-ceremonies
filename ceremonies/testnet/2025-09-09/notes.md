# dcipher testnet ceremony: 2025.09.09

## Version
tag/v0.1.0-testnet.1
commit/50ad036c5add4e73c20529effa43c305a4d7fb38

## Functionality under test
- high threshold ADKG 
- onlyswaps verifier

## Participants
- ATA network
- Storswift
- Randamu

## Scheme 
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
n = 5
t = 1
t_reconstruction = 3
start_time = "2025-09-09T14:10:00Z"

[[nodes]]
id = 1
multiaddr = "/dns/dkg-testnet1.dcipher.network/tcp/80"
adkg_pk = "3ywr0W6ZEscpJYJ4yznY+Ic1lCLJZJxOhK9iz/8r53M="
peer_id = "12D3KooWKBZgjNMhZhb7aKTTbSFM91QeGec6KQTNfJaKqqw6DzJ3"

[[nodes]]
id = 2
multiaddr = "/dns/dkg-testnet2.dcipher.network/tcp/80"
adkg_pk = "0Lz3gnmDryH21FT7V039AVnOuPIWxiaPE6XhV8bsICA="
peer_id = "12D3KooWPRv6Rizhyr94UaT5WXsrZydE8mFkjitR7bF95tCWV657"

[[nodes]]
id = 3
multiaddr = "/dns/dkg-testnet3.dcipher.network/tcp/80"
adkg_pk = "6YMJVN9nsNCht9eOJrmU0JlZhPAJM+dm7yvbnM6kF+A="
peer_id = "12D3KooWDAwFMox7uzJ9yRzSdNesgbUhNhXoWXFGUuYSFA24jk9t"

[[nodes]]
id = 4
multiaddr = "/dns/dciphertest.ata.network/tcp/7777"
adkg_pk = "kgIm7IgwJYktWZ24ym1x/nL9KLaGWw1cT5QuYOZLlyQ="
peer_id = "12D3KooWP5191tQRaMGbKXVC5Aja1wPNdiZfRbAv5FKi5ZfszDfF"

# storswift
[[nodes]]
id = 5
multiaddr = "/ip4/47.104.170.105/tcp/7890"
adkg_pk = "ySf1t0UZx6ol5L9iLy7icddHtiKBb8kFuKWpNWa1w6g="
peer_id = "12D3KooWDLZZZrLeQ5AgY9x3mmkpbThB8XVyyrhTXyJUDPeD9pUQ"
```

## Notes
- Randamu nodes not running on drand nodes anymore, but behind a cloudflare tunnel

## Log
Timings all in UTC:

- 13:10 ceremony version finally released... a bit behind >.>
- 14:07 Storswift reports timeouts for PartyIds 1 and 2 (Randamu nodes)
- 14:09 Randamu seeing `AddrInUse` in logs - advises ATA to update firewall to include another IP
```
Failed to dial peer error=Transport([(/dns/dciphertest.ata.network/tcp/7777/p2p/12D3KooWP5191tQRaMGbKXVC5Aja1wPNdiZfRbAv5FKi5ZfszDfF, Other(Custom { kind: Other, error: Other(Transport(Left(Left(Os { code: 48, kind: AddrInUse, message: "Address already in use" })))) }))]) peer_id=12D3KooWP5191tQRaMGbKXVC5Aja1wPNdiZfRbAv5FKi5ZfszDfF short_id=PartyId(4)
```
- 14:10 ceremony runs, output looks promising:
```
2025-09-09T14:10:24.726928Z  INFO adkg_cli::adkg_dxkr23: ADKG has terminated with an Ok output
2025-09-09T14:10:24.726947Z  INFO adkg_cli::adkg_dxkr23: Running ADKG until grace period of 5m
```
- 14:10 Storswift reports connectivity problems with all Randamu nodes:
```
2025-09-09T14:10:03.490879Z  WARN adkg::rbc::r4::handlers: Node `5` refused proposal proposal_sender=PartyId(3)
2025-09-09T14:10:03.496263Z  WARN adkg::rbc::r4::handlers: Node `5` refused proposal proposal_sender=PartyId(2)
2025-09-09T14:10:03.496374Z  WARN adkg::rbc::r4::handlers: Node `5` refused proposal proposal_sender=PartyId(1)
2025-09-09T14:10:26.135011Z  INFO adkg_cli::adkg_dxkr23: ADKG has terminated with an Ok output
2025-09-09T14:10:26.135044Z  INFO adkg_cli::adkg_dxkr23: Running ADKG until grace period of 5m
```
- 14:10 ATA reporting various failures connecting to Randamu nodes:
```
2025-09-09T14:10:24.223061Z ERROR dcipher_network::transports::libp2p::events_handler: Point to point outbound failure sender_peer_id=12D3KooWKBZgjNMhZhb7aKTTbSFM91QeGec6KQTNfJaKqqw6DzJ3 sender_short_id=Some(PartyId(1)) point_to_point_request_id=OutboundRequestId(41) error=DialFailure
2025-09-09T14:10:24.223082Z  WARN dcipher_network::transports::libp2p::dialer: Failed to dial peer error=NoAddresses peer_id=12D3KooWPRv6Rizhyr94UaT5WXsrZydE8mFkjitR7bF95tCWV657 short_id=PartyId(2)
2025-09-09T14:10:24.223089Z ERROR dcipher_network::transports::libp2p::events_handler: Point to point outbound failure sender_peer_id=12D3KooWPRv6Rizhyr94UaT5WXsrZydE8mFkjitR7bF95tCWV657 sender_short_id=Some(PartyId(2)) point_to_point_request_id=OutboundRequestId(42) error=DialFailure
2025-09-09T14:10:24.223097Z  WARN dcipher_network::transports::libp2p::dialer: Failed to dial peer error=NoAddresses peer_id=12D3KooWDAwFMox7uzJ9yRzSdNesgbUhNhXoWXFGUuYSFA24jk9t short_id=PartyId(3)
2025-09-09T14:10:24.223104Z ERROR dcipher_network::transports::libp2p::events_handler: Point to point outbound failure sender_peer_id=12D3KooWDAwFMox7uzJ9yRzSdNesgbUhNhXoWXFGUuYSFA24jk9t sender_short_id=Some(PartyId(3)) point_to_point_request_id=OutboundRequestId(43) error=DialFailure
```
- 14:13 ATA network reporting `InvalidData` from Randamu 2:
```
Failed to dial peer error=Transport([(/dns/dkg-testnet2.dcipher.network/tcp/80/p2p/12D3KooWPRv6Rizhyr94UaT5WXsrZydE8mFkjitR7bF95tCWV657, Other(Custom { kind: Other, error: Other(Transport(Left(Right(Apply(Io(Kind(InvalidData))))))) }))]) 
```

- 14:15 grace period is over, and Randamu nodes have finished the DKG but are still hanging:
```
2025-09-09T14:15:24.730327Z  WARN adkg_cli::adkg_dxkr23: Stopping ADKG...
```
- 14:16 public/private keyshares all empty for Randamu nodes
- 14:20 Storswift successfully got a valid committee output:
```
adkg_scheme_name = "DXKR23-Bn254G1-Keccak256"
genesis_timestamp = 1757427000
group_pk = "plpM2M2rfYvd0ZFkN+6SUTymphKWJ90VKMhJZBYffKQXPSHnW1zPom6Vjy7yH/iV8mRP+OGN2ZNG+7GP3C49fQ=="
[[node_pks]]
id = 1
pk = "y6huf6EmWAua8nig8VHU1+Hbwmt9CLnBVuAr1kSc5P0r39x+07NfNyNGlpAbvzj4M8opGmaFO3C5Hbyp1W6yiQ=="
peer_id = "12D3KooWKBZgjNMhZhb7aKTTbSFM91QeGec6KQTNfJaKqqw6DzJ3"
multiaddr = "/dns/dkg-testnet1.dcipher.network/tcp/80"
[[node_pks]]
id = 2
pk = "xoT85ifH00N8zOOVmd/jYwfQT5JDsggguo/M4dYVkQoTtsPmEIR1f69yH5CcQ8vHgwEG8EHHaL7Y1FOdQSewvA=="
peer_id = "12D3KooWPRv6Rizhyr94UaT5WXsrZydE8mFkjitR7bF95tCWV657"
multiaddr = "/dns/dkg-testnet2.dcipher.network/tcp/80"
[[node_pks]]
id = 3
pk = "wZLWvwtFh7IJH0xmNo9w/0brpbhaNxMvWjXfW3Vw1hoYayLPBfcVg435ZGbH6JfbrbA5cTRCt40O2lUSVmZ3aA=="
peer_id = "12D3KooWDAwFMox7uzJ9yRzSdNesgbUhNhXoWXFGUuYSFA24jk9t"
multiaddr = "/dns/dkg-testnet3.dcipher.network/tcp/80"
[[node_pks]]
id = 4
pk = "yibUbzQ+223kEbFHNqAoguATUOcymSvvaUyXISpJtPMFwQZy1VrgeQNZM29p1plwHS9i+5ZUz4A4NPcZkMuEXw=="
peer_id = "12D3KooWP5191tQRaMGbKXVC5Aja1wPNdiZfRbAv5FKi5ZfszDfF"
multiaddr = "/dns/dciphertest.ata.network/tcp/7777"
[[node_pks]]
id = 5
pk = "xGd5ScPKsIECOH1Nt61VOOIGPoXBCoQGac6m4pyH6iIPEbIhb78fFfU0n6AmOPJJsuXfsoEGdqQnJAD9uNwEIA=="
peer_id = "12D3KooWDLZZZrLeQ5AgY9x3mmkpbThB8XVyyrhTXyJUDPeD9pUQ"
multiaddr = "/ip4/47.104.170.105/tcp/7890"
```

## Ceremony Result
💥 Failure

## Post-mortem
Randamu nodes were running over Cloudflare tunnels, which caused libp2p to report `AddrInUse`, hence why Storswift could connect to 2 out of the 3 nodes. 
Storswift were able to receive incoming packets from other nodes to build a final committee output.
