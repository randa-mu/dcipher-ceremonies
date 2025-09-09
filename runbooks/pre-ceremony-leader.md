# Before the Ceremony (Leader)

## Steps

**1. Update to the latest DKG CLI**

```bash
rustup install stable && \
cd /path/to/dcipher && \
git pull && \
git submodule update --init --recursive && \
cargo build --release -p adkg-cli && \
cp target/release/adkg-cli /usr/local/bin
```

**2. Generate a scheme for the network**

```bash
adkg-cli new-scheme --app-name dcipher-testnet --scheme-out scheme.toml
```

**3. Share the newly created scheme with the participants**

`cat` the scheme.toml and share its contents via slack.

**4. Prompt dcipher operators to generate their long-term keys**

- message the dcipher operators on slack and prompt them to generate keys using the [relevant steps](./operator-key-generation.md)
- gather the chosen multiaddrs and generated peerIDs of all ceremony participants

**5. Bundle multiaddrs and peerIDs into a `group.toml`**

Aggregate the multiaddrs and peerIDs from other participants into a file, giving them arbitrary indexes.
Create a toml file like the following:

```toml
n = 7
t = 2
t_reconstruction = 4
start_time = "2025-07-24T16:25:30Z"

[[nodes]]
id = 1
multiaddr = "/ip4/127.0.0.1/tcp/9991"
adkg_pk = "71zT8Vib8vHG3vX2cktMyFl5VngXFi8RLOkIw9a5ulI="
peer_id = "12D3KooWMSH17hbmMBSbEtCeyYBkFT7phd6PVaA2fdxakaTAuXMx"

[[nodes]]
id = 2
multiaddr = "/dns4/my.domain.com/tcp/1005"
adkg_pk = "354LSCLd/rCf8bX/vN+nWNfO2G2ZoLs/v054IAgiDFk="
peer_id = "12D3KooWMAADWEWNMFCNBadQfNFLeHHcYeZr9tNJBekY4opdCzDM"

[...]

[[nodes]]
id = 7
multiaddr = "/ip4/127.0.0.1/tcp/7777"
adkg_pk = "7dpgwvtWiLAw/TweCrzBeRuWahRbxqMwACwiiulYkfA="
peer_id = "12D3KooWGjQdQ6B3LazUw2EVbhakN3P5931e1UV76vJUNoV73Dd4"
```

> [!NOTE]
> `t` is the _malicious threshold_, i.e. the maximum number of malicious nodes the network can tolerate. 
>
> **This is different to drand**. 
>
> Under the current asynchronous DKG protocol, it can only be as high as $f$ where the total node count is $3f + 1$.
> `t_reconstruction` is the `t` you may be used to from drand-land

> [!NOTE]
> `n` should be the same as the number of node entries.

> [!WARNING]
> If the nodes aren't running by the `start_time` of the ceremony, they may be excluded

**6. Share the `group.toml` file with the other dcipher network participants**

Tell them their index for convenience, though they ought to check this themselves against their peerID

