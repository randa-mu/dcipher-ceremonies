# During the Ceremony (Operator)

## Prerequisites
- you've run the [pre-ceremony](./pre-ceremony-operator.md) runbook
- you've received a `scheme.toml` from the leader
- the leader has shared a `group.toml` containing all the public keys and multiaddrs of participating nodes with you

## Steps
1. Extract your assigned ID from the `group.toml` file by finding the entry with your `peer_id`
Ask the leader if you're not sure how to do this.
It should be a non-zero integer, e.g. `1`

2. Export the ID 
e.g. `export OPERATOR_ID=1`

3. Run the share command, replacing the contents of each flag with values relevant to your node
```bash
adkg-cli run \
  --scheme ./scheme.toml \                   # replace with the path to the scheme.toml file the leader gave you
  --group ./group.toml \                     # replace with the path to the group.toml the leader gave you
  --priv adkg.priv \                         # replace with the path to your private key file
  --id $OPERATOR_ID \                                   # replace with your assigned ID
  --listen-address "/ip4/0.0.0.0/tcp/7777" \ # replace with your chosen multiaddr
  --priv-out keyshare.priv \                 # replace this with somewhere you can store your shared keys
  --pub-out keyshare.pub \                   # replace this with somewhere you can store your shared keys
  --transcript-out transcript.json           # replace this with somewhere you can store a transcript in case somebody fails to join

```

> [!WARNING]  
> Don't write your distributed key share to a public drive or people can impersonate you!

> [!WARNING]  
> Don't share your distributed key share with anybody!

> [!WARNING]  
> Make sure your distributed key share is backed up. If you lose it, you won't be able to participate in the dcipher network until the next ceremony.

4. Twiddle your thumbs until the start time detailed in the `group.toml` has been reached.

5. Monitor your CLI output and report any errors or suspicious log messages to the leader

6. Wait until the ceremony ends and the grace period is compelte

You should see log messages such as the following:
```
2025-09-09T14:10:24.726928Z  INFO adkg_cli::adkg_dxkr23: ADKG has terminated with an Ok output
2025-09-09T14:10:24.726947Z  INFO adkg_cli::adkg_dxkr23: Running ADKG until grace period of 5m
```
This is a good sign, though your keyshares will be empty until the end of the grace period. This period allows other members of the committee to catch up.
