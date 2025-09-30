# dcipher mainnet ceremony: 2025.09.30

## Version
enter a tag, commit, or branch here

## Purpose
- Initial key sharing ceremony

## Participants
- ATA Network
- DIA Data
- Emerald Onion
- IPFS Force
- Randamu
- Storswift

## Scheme 
Once the leader has created the scheme for the ceremony, put it here

```toml
app_name = "dcipher-mainnet"
curve_id = "Bn254G1"
hash_id = "Keccak256"
adkg_version = "v0.1"
generator_g = "jQq6k7FT4wDe6xUhFDT/ugVNw6wZons33ljhfXMIxrg="
generator_h = "7G86aBjXNwcx16jxxbeyO1hNNPB4BqL6w9vLw35wk2U="
adkg_scheme_name = "DXKR23-Bn254G1-Keccak256"
output_generator = "mY6Tk5INSDpyYL+3MftdJfGqSTM1qecSl+SFt67zEsIYAN7vEh8edkJqAGZeXER5Z0Mi1Pde2t1G3r1c2ZL27Q=="
```

## Group 
Once the network have generated their libp2p and bn254 keys, create a group file with the public key material and put it here

```toml
n = 7
t = 2
t_reconstruction = 3
start_time = "2025-09-30T14:00:00Z"

# Randamu
[[nodes]]
id = 1
multiaddr = "/dns/mainnet1.dkg.dcipher.network/tcp/8080"
adkg_pk = "hw5NTuNWxYL/JBKLz55pjjnhThCpcu1CtX9JxjVcoG0="
peer_id = "12D3KooWJvYaMqERGdUagqka5YwCpN7FA9enHbonXNhdnifCLRXm"

# Randamu
[[nodes]]
id = 2
multiaddr = "/dns/mainnet2.dkg.dcipher.network/tcp/8080"
adkg_pk = "15QtyW0mCFRzZNw5y/M19JAxhh/V20a1mHjaDp7OCh4="
peer_id = "12D3KooWBzwNW9WKAGyn1nBkEoMQA86go3xCYVf9Sn8rxfQTC7HD"

# ATA
[[nodes]]
id = 3
multiaddr = "/dns/dcipher.ata.network/tcp/7778"
adkg_pk = "k++eRlBlg6L2lcGcQp/sWcF6ejgOJ82rwtFJu+o+ubc="
peer_id = "12D3KooWFjbRVYTGRDLMFCXJukiTjGY2h8yA53SNg7EoPDdXzg9r"

# IPFSForce
[[nodes]]
id = 4
multiaddr = "/dns/dci-mainnet.ipfsforce.com/tcp/7777"
adkg_pk = "i36ck1eJ8dzwMpnuq94oTd9qBenuTWlI0kgK1ILR7m4="
peer_id = "12D3KooWJX9TK3zWadqv2DeG1fsUbrmfxVLJqzGfPr23QGetvDrx"

# Storswift
[[nodes]]
id = 5
multiaddr = "/dns/labs.secureweb3.com/tcp/8890"
adkg_pk = "m6GO4Sq+CMRB2D/IpibC59E+fiN+1Nnv+G8NhskQey8="
peer_id = "12D3KooWSBJZ3Z3Qr5GsdNTRC3ePbNdApURWK35N4iAXzjv6rKh9"

# Grüne Zwiebel
[[nodes]]
id = 6
multiaddr = "/dns/misc.quimian.com/tcp/5050"
adkg_pk = "iVtJMj47NRCvjdjlediL8u3IX7Flh2sSjP7DSBs8AAs="
peer_id = "12D3KooWDkCVCePZYXrHYhjVM9d7CLKSMHvUgpon8EPZKp341nDU"

# DIA Data
[[nodes]]
id = 7
multiaddr = "/dns/drand.diadata.org/tcp/4243"
adkg_pk = "2Am7AFGsLths8uYtNFLI+iQ6Yz3K3Y9q1+LKdMcrSkA="
peer_id = "12D3KooWB8nSysj9DNLgeWdS3aTbb2htiPHn99LA5jUuDrreMadH"

```

## Log
Log any timings, actions, issues, or points to note here
- 0815: group file compiled :partyparrot:
- 0824: operators instruct to start the ceremony at their leisure
- 0846: Randamu logs show ATA and EO online (aligning with their slack emojis)
- 0902: DIA join the fray
- 1200: ceremony goes off without a hitch and everybody gets a keyshare!!


