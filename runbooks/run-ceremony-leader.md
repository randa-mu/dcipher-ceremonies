# During the Ceremony Leader(

## Prerequisites
- you've run the [pre-ceremony (leader)](./pre-ceremony-leader.md) runbook
- you've shared a `scheme.toml` with the other participants
- you've shared a `group.toml` containing all the public keys and multiaddrs of participating nodes with the other participants

## Steps
1. Extract your assigned ID from the `group.toml` file by finding the entry with your `peer_id`
It should be a non-zero integer, e.g. `1`.
Export it for use in the next step: `export ID=1`

2. Run the share command, replacing the contents of each flag with values relevant to your node  
```bash
adkg-cli run \
  --scheme ./scheme.toml \                   # replace with the path to the scheme.toml file the leader gave you
  --group ./group.toml \              # replace with the path to the nodes.toml the leader gave you
  --priv operator.priv \                         # replace with the path to your private key file
  --id $ID \                                   # replace with your assigned ID
  --listen-address "/ip4/0.0.0.0/tcp/7777" \ # replace with your chosen multiaddr
  --timeout "48h" \                          
  --priv-out keyshare.priv \                 # replace this with somewhere you can store your private key
  --pub-out keyshare.pub  \                  # replace this with somewhere you can store your shared keys
  --transcript-out transcript.json           # replace this with somewhere you can store the DKG transcript
```

> [!WARNING]  
> Don't write your distributed key share to a public drive or people can impersonate you!

> [!WARNING]  
> Don't share your distributed key share with anybody!

> [!WARNING]  
> Make sure your distributed key share is backed up. 
> If you lose it, you won't be able to participate in the dcipher network until the next ceremony.

3. Twiddle your thumbs until the start time from the group.toml has been reached

4. Report any errors or suspicious log messages to the leader

