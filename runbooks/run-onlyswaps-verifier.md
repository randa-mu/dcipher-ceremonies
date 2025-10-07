# Run onlyswaps-verifier

## Prerequisites
- you've run a successful ceremony and completed [the ceremony](./run-ceremony-operator.md) runbook
- you've installed the [onlyswaps-verifier CLI](./installing-onlyswaps-verifier.md)

---

## Steps

### **1. Adapt config file to your key paths**
Store the following config map in a location where your verifier will be able to access it.  
In the first section, fill in the various paths from the outputs of [keygen](./operator-key-generation.md) and the [ADKG ceremony](./run-ceremony-operator.md) prerequisites.

Choose one of the configs below for adaptation, depending on the ceremony type:

<details>
<summary><strong>Mainnet</strong></summary>  

```toml
# your ID in the adkg_public file
member_id = 1

# this is the address you bind locally, not necessarily the multiaddr others connect to you with
listen_addr = "/ip4/0.0.0.0/tcp/9898"

# you generated this before running the ADKG
longterm_secret_path = "/path/to/longterm/secret/key/longterm.priv"

# you received this before the ADKG to know who to connect to
group_path = "/path/to/group/file/pre/group.toml"

# this was created during the ADKG
adkg_public_path = "/path/to/pub/adkg/keyshare.pub"

# this was created during the ADKG
adkg_secret_path = "/path/to/priv/adkg/keyshare.priv"

# just use the 0 private key for now, unless you want to race Randamu nodes for fulfillment ;)
eth_private_key = "0x00000000000000000000000000000000000000000000000000000000000000"

# `agent` is used for general configuration and monitoring params
[agent]
healthcheck_listen_addr = "0.0.0.0"
healthcheck_port = 9999                     # make sure not to bind the same as the listen_addr!
log_level = "debug"                         # debug, info, trace, error
log_json = true                             # whether the logs should be structured as JSON or plaintext

# `networks` details all the configuration relating to connecting to blockchains. Each can be configured independently.
# Presently all networks must be supported, and skipping verifications for one route (chain -> chain) may cause errors.
# You may configure an RPC of your choice, but only websockets and websockets secure are supported
[[networks]]
chain_id = 43114
rpc_url = "wss://avalanche-c-chain-rpc.publicnode.com"
router_address = "0x4cB630aAEA9e152db83A846f4509d83053F21078"
should_write = false          # controls whether this node writes signatures back to chain; you probably want false

[[networks]]
chain_id = 8453
rpc_url = "wss://base-rpc.publicnode.com"
router_address = "0x4cB630aAEA9e152db83A846f4509d83053F21078"
should_write = false
```
</details>

<details>
<summary><strong>Testnet</strong></summary>  

```toml
# your ID in the adkg_public file
member_id = 1

# this is the address you bind locally, not necessarily the multiaddr others connect to you with
listen_addr = "/ip4/0.0.0.0/tcp/9898"

# you generated this before running the ADKG
longterm_secret_path = "/path/to/longterm/secret/key/longterm.priv"

# you received this before the ADKG to know who to connect to
group_path = "/path/to/group/file/pre/group.toml"

# this was created during the ADKG
adkg_public_path = "/path/to/pub/adkg/keyshare.pub"

# this was created during the ADKG
adkg_secret_path = "/path/to/priv/adkg/keyshare.priv"

# just use the 0 private key for now, unless you want to race Randamu nodes for fulfillment ;)
eth_private_key = "0x00000000000000000000000000000000000000000000000000000000000000"

# `agent` is used for general configuration and monitoring params
[agent]
healthcheck_listen_addr = "0.0.0.0"
healthcheck_port = 9999                     # make sure not to bind the same as the listen_addr!
log_level = "debug"                         # debug, info, trace, error
log_json = true                             # whether the logs should be structured as JSON or plaintext

# `networks` details all the configuration relating to connecting to blockchains. Each can be configured independently.
# Presently all networks must be supported, and skipping verifications for one route (chain -> chain) may cause errors.
# You may configure an RPC of your choice, but only websockets and websockets secure are supported
[[networks]]
chain_id = 43114
rpc_url = "wss://avalanche-fuji-c-chain-rpc.publicnode.com"      
router_address = "0x4cB630aAEA9e152db83A846f4509d83053F21078"
should_write = false               # you probably want this to be false unles you want to pay gas fees racing the randamu nodes ;)

[[networks]]
chain_id = 84532
rpc_url = "wss://base-sepolia-rpc.publicnode.com"
router_address = "0x4cB630aAEA9e152db83A846f4509d83053F21078"
should_write = false
```

</details>

### **2. Attempt to execute the verifier**
1. Run the onlyswaps-verifier command, specifying the config file:
   ```bash
   onlyswaps-verifier start --config path/to/my/config.toml
   ```
2. Wait for a few seconds, making sure the verifier starts without errors.
   Stop the verifier with Ctrl-C.
3. To run a persistent verifier, we currently have two options: systemd and docker compose.
   Choose whichever works best, and follow the steps below.

---

## systemd

### **3. (Optional) Move onlyswaps-verifier binary to /opt**  
Run the following commands to move the onlyswaps-verifier binary to `/opt/onlyswaps/onlyswaps-verifier`.
```bash
# move onlyswaps-verifier binary to /opt/onlyswaps/onlyswaps-verifier
mkdir -p /opt/onlyswaps
mv ./target/release/onlyswaps-verifier /opt/onlyswaps/
```

### **4. Move config to /opt**
Run the following commands to move the onlyswaps-verifier configuration file to `/opt/onlyswaps/verifier.toml`.
```bash
# move config to /opt/onlyswaps/verifier.toml
mkdir -p /opt/onlyswaps/etc
mv path/to/my/config.toml /opt/onlyswaps/etc/verifier.toml

# adjust verifier.toml permissions
chmod 640 /opt/onlyswaps/etc/verifier.toml
```

### **5. (Optional) Create a new user**  
Depending on your setup, you may want/need to create a separate user to run the onlyswaps-verifier service.
This can be done with the following commands:
```bash
# create onlyswaps user
useradd --system --shell /bin/false --no-create-home onlyswaps

# adjust permissions
chown onlyswaps:onlyswaps /opt/onlyswaps/onlyswaps-verifier
chmod +x /opt/onlyswaps/onlyswaps-verifier
chown root:onlyswaps /opt/onlyswaps/etc/verifier.toml
```

### **6. Create unit file**  
Create a new unit file under `/etc/systemd/system/onlyswaps-verifier.service`, with the following content:
```ini
[Unit]
Description=onlyswaps-verifier service
After=network.target
Wants=network.target

[Service]
Type=simple
# TODO: Remove/update it if you didn't create a user
User=onlyswaps
Group=onlyswaps
# TODO: Update path if not using /opt
ExecStart=/opt/onlyswaps/onlyswaps-verifier start --config /opt/onlyswaps/etc/verifier.toml
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal

NoNewPrivileges=yes
PrivateTmp=yes
ProtectSystem=strict
ProtectHome=yes

[Install]
WantedBy=multi-user.target
```

### **7. Run the service**  
Enable and start the service with the following commands:
```bash
# Reload systemd to recognize the new service
systemctl daemon-reload

# Enable the service to start at boot
systemctl enable onlyswaps-verifier

# Start the service now
systemctl start onlyswaps-verifier
```

Make sure that the service successfully started with `systemctl status onlyswaps-verifier`.

---

## docker compose
With docker compose, you may either run the service as root, or with another user (for instructions, you may follow the user creation step shown in [systemd](#systemd)).

### **1. Create `docker-compose.yml` configuration**
1. Save the following `docker-compose.yml` to a location of your choosing:
   ```yaml
   services:
     onlyswaps-verifier:
       image: ghcr.io/randa-mu/dcipher/onlyswaps-verifier:main-latest
       container_name: onlyswaps-verifier
       restart: unless-stopped
       # Run as current user or UID/GID 1000
       # TODO: remove if it should be executed as root
       user: "${UID:-1000}:${GID:-1000}"
       ports:
         # TODO: update libp2p port below if using a different one
         - "7777:7777"
       volumes:
         - path/to/my/config.toml:/opt/onlyswaps/etc/verifier.toml:ro
       command: ["--config", "/opt/onlyswaps/etc/verifier.toml"]
   ```
2. Update the user id if different than the current user. If executing as root, use `user: "0:0"` (or comment the line).
3. Update the libp2p port in the docker-compose file if different. If you use, say port `8888`, you should replace `7777:7777` with `8888:8888` instead.

### **2. Run the service**  
Once the `docker-compose.yml` is ready, you can execute the service with `docker compose up -d`.
After a few seconds, check the logs with `docker compose logs -f` to make sure the verifier has been started.

