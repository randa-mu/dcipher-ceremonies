# During the Ceremony (Operator)

## Prerequisites
- the DKG CLI is installed
- you've received a `scheme.toml` from the leader
- you've generated your keys
- you've run the [pre-ceremony](./pre-ceremony-operator.md) runbook
- the leader has shared a `nodes.toml` containing all the public keys and multiaddrs of participating nodes with you

## Steps
1. Extract your assigned ID from the `nodes.toml` file by finding the entry with your `peer_id`
Ask the leader if you're not sure how to do this.
It should be a non-zero integer, e.g. `1`

2. Export the ID and start time
e.g. `export OPERATOR_ID=1`
e.g. `export START_TIME="2025-07-24T16:25:30Z"`

> [!NOTE]  
> If the `START_TIME` assigned by the leader has already passed, they've probably made a mistake - confirm it with them before continuing!

3. Run the share command, replacing the contents of each flag with values relevant to your node
```bash
adkg-cli adkg \
  --scheme ./scheme.toml \                   # replace with the path to the scheme.toml file the leader gave you
  --priv adkg.priv \                         # replace with the path to your private key file
  --id 1 \                                   # replace with your assigned ID
  --node-details ./nodes.toml \              # replace with the path to the nodes.toml the leader gave you
  --start-time "2025-07-24T16:25:30Z" \      # replace with the start time given by the leader
  --listen-address "/ip4/0.0.0.0/tcp/7777" \ # replace with your chosen multiaddr
  --timeout "48h" \                          
  --grace-period "20s" \
  --priv-out keyshare.priv \                 # replace this with somewhere you can store your shared keys
  --pub-out keyshare.pub                     # replace this with somewhere you can store your shared keys

```

> [!WARNING]  
> Don't write your distributed key share to a public drive or people can impersonate you!

> [!WARNING]  
> Don't share your distributed key share with anybody!

> [!WARNING]  
> Make sure your distributed key share is backed up. If you lose it, you won't be able to participate in the dcipher network until the next ceremony.

4. Twiddle your thumbs until the `start-time` has passed

5. Report any errors or suspicious log messages to the leader

