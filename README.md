# dcipher ceremonies

This repository contains all material relating to dcipher network ceremonies, including instructions, transcripts and public key material.

## Purpose
The asynchronous distributed key generation (ADKG) ceremony is a decentralized cryptographic setup protocol executed by a set of nodes to jointly generate a shared public/private key pair without relying on a trusted dealer.

In the context of the dcipher network, this ceremony bootstraps a threshold cryptography network that allows nodes to later perform collective operations like decryption or signing, where a subset of nodes (e.g. 3-of-7) can act on behalf of the group.

This is based on the paper [Practical Asynchronous Distributed Key Generation by Das et al](https://eprint.iacr.org/2021/1591.pdf)., and the adkg-cli tool from the dcipher repo implements this process.

## Phases
The ADKG ceremony consists of three stages:

- Scheme Definition: One participant (henceforth referred to as 'the leader') defines and shares a scheme file that outlines the curve parameters, and generators, and shares it with the other participants.
- Operator Key Generation: Each participant generates two long-term keypairs (ADKG and libp2p) and submits their public material to the leader, whereby the leader will create a group file.
- Distributed Key Generation: All nodes run the ADKG protocol simultaneously using synchronized start time, scheme definition and shared group file, producing a distributed public/private keypair suitable for threshold cryptographic operations.

## Runbooks
This directory contains a variety of runbooks for operators and leaders.

### Operators
- [Installing the DKG CLI](runbooks/installing-cli.md)
- [Operator Key Generation](runbooks/operator-key-generation.md)
- [Before the Ceremony (Operator)](runbooks/pre-ceremony-operator.md)
- [During the Ceremony (Operator)](runbooks/run-ceremony-operator.md)

### Leaders
- [Before the Ceremony (Leader)](runbooks/pre-ceremony-leader.md)

## Schemes
A scheme is used to describe an instance of the ADKG.
It contains various parameters, such as the curve, curve parameters, and the version.
Here is an example of a scheme specification used in the context of `dcipher`:  

```toml
app_name = "dcipher"
curve_id = "Bn254G1"
hash_id = "Keccak256"
adkg_version = "v0.1"
adkg_scheme_name = "DYX20-Bn254G1-Keccak256"
generator_g = "qB3/U8RDVn4aF2tUTlmeDQbV0PvHJ8IB0QL/k1Z+5WI="
generator_h = "gAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAE="
```

The `new-scheme` command can be used to obtain a scheme specification as follows:
```bash
adkg-cli new-scheme --app-name dcipher --out scheme.toml
```

By default, we use the `DYX20-Bn254G1-Keccak256` scheme.
This represents the aforementioned ADKG, using the bn254 curve on group G1.
In this configuration, the long-term public keys use a deterministic generator, `H`, obtained by hashing `ADKG_GENERATOR_G` with the DST `ADKG-%adkg_version%-%app_name%_BN254G1_XMD:KECCAK-256_SVDW_RO_GENERATORS_` using [rfc9380](https://datatracker.ietf.org/doc/html/rfc9380).
The public keys after the ADKG, however, are output with respect to the standard generator of bn254, the point `(1, 2)`.

It is preferable that a single participant executes the `new-scheme` command, and sends the generated file to the rest of the participants.
