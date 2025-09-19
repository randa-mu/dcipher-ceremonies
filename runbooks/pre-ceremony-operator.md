# Before the Ceremony (Operator)

## Prerequisites
- DKG CLI is installed
- operator keys have been generated

## Steps

**1. Update to the latest DKG CLI**

Pull the latest commit on the `main` branch from the [dcipher repo](https://github.com/randa-mu/dcipher) and follow the steps in the [installation guide](./installing-adkg-cli.md).

**2. Generate your keys**

[Generate your keys](./operator-key-generation.md) and share the public material with the leader (if you haven't already)

**3. Open your firewall**  

Ensure the TCP port in your multiaddr is publicly reachable. For example:
```bash
sudo ufw allow 7777/tcp
```

**4. Ask the leader for a `group.toml` file detailing all the participants in the upcoming ceremony.**

