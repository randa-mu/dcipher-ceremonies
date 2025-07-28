# Operator Key Generation

## Prerequisites
- you have installed the ADKG CLI
- you have received a `scheme.toml` file compiled by a ceremony leader

## Steps

1. Copy the contents of the leader's `scheme.toml` file in a readable location on your machine, and export it as `SCHEME_FILE`:
 e.g. `export SCHEME_FILE=/home/myuser/scheme.toml`

2. Choose a writable location for your keys, and export it as `ADKG_OUT`:
 e.g. `export ADKG_OUT=/home/myuser/adkg`

3. Generate long-term keys in your chosen directories:
```bash
adkg-cli generate \
  --scheme $SCHEME_FILE \
  --priv-out $ADKG_OUT/adkg.priv \
  --pub-out $ADKG_OUT/adkg.pub
```

4. Report any errors to the ceremony leader

5. Check the file at `$ADKG_OUT/adkg.pub` cotains similar to the following:
```
adkg_pk = "4H97WjJsuKRnNACoGbnIVrYs4kzfB2VmlmK4bhDDThg="
peer_id = "12D3KooWCpsdsUvGkqajWcgU6v62R5edN8UG5QrDvfKMTZiwBBV6"
```
If not, report it to the ceremony leader.

6. Check the file at `$ADKG_OUT/adkg.priv` contains content similar to the following:
```
adkg_sk = "Jq4uUbMcM6Ssqv9+zyqRHTpk479+uosoxkG6g+B2HJM="
libp2p_sk = "CAESQLDgVgJ2B3dOnUvSNnmRZmP3FeBy5SuO7G2RPirFVrt9LLRhSL+o2KkQqYDpwqzDKPGnpR5jhpg+EUS4mmemaFU="
```
If not, report it to the ceremony leader.

> [!WARNING]
> Don't write your distributed key share to a public drive or people can impersonate you!

> [!WARNING]
> Don't share your distributed key share with anybody!

> [!WARNING]
> Make sure your distributed key share is backed up. If you lose it, you won't be able to participate in the dcipher network until the next ceremony.

7. Choose a publicly accessible multiaddress for operating your DKG CLI.
Typically, this will be an IP/port e.g., `/ip4/192.168.1.5/tcp/7777)` or a DNS entry/port combo, e.g. `/dns/example.org/tcp/443`.

8. Send your `peer_id`, `adkg_pk`, and a chosen multiaddress to the ceremony leader

