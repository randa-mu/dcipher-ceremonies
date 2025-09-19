# Run onlyswaps-verifier

## Prerequisites
- you've run a successful ceremony and completed [the ceremony](./run-ceremony-operator.md) runbook
- you've installed the [onlyswaps-verifier CLI](./installing-onlyswaps-verifier.md)
- you've followed the [generate-onlyswaps-config](./generate-onlyswaps-config.md) runbook and have created an onlyswaps-verifier config file.

## Steps
**1. Attempt to execute the verifier**
1. Run the onlyswaps-verifier command, specifying the config file:
   ```bash
   onlyswaps-verifier start --config path/to/my/config.toml
   ```
2. Wait for a few seconds, making sure the verifier starts without errors.
   Stop the verifier with Ctrl-C.
3. To run a persistent verifier, we currently have two options: systemd and docker compose.
   Choose whichever works best, and follow the steps below.

## systemd
**1. (Optional) Move onlyswaps-verifier binary to /opt**  
Run the following commands to move the onlyswaps-verifier binary to `/opt/onlyswaps/onlyswaps-verifier`.
```bash
# move onlyswaps-verifier binary to /opt/onlyswaps/onlyswaps-verifier
mkdir -p /opt/onlyswaps
mv ./target/release/onlyswaps-verifier /opt/onlyswaps/
```

**2. Move config to /etc**
Run the following commands to move the onlyswaps-verifier configuration file to `/etc/onlyswaps/verifier.toml`.
```bash
# move config to /etc/onlyswaps/verifier.toml
mkdir -p /etc/onlyswaps
mv path/to/my/config.toml /etc/onlyswaps/verifier.toml

# adjust verifier.toml permissions
chmod 640 /etc/onlyswaps/verifier.toml
```

**3. (Optional) Create a new user**  
Depending on your setup, you may want/need to create a separate user to run the onlyswaps-verifier service.
This can be done with the following commands:
```bash
# create onlyswaps user
useradd --system --shell /bin/false --no-create-home onlyswaps

# adjust permissions
chown onlyswaps:onlyswaps /opt/onlyswaps/onlyswaps-verifier
chmod +x /opt/onlyswaps/onlyswaps-verifier
chown root:onlyswaps /etc/onlyswaps/verifier.toml
```

**4. Create unit file**  
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
ExecStart=/opt/onlyswaps/onlyswaps-verifier start --config /etc/onlyswaps/verifier.toml
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

**5. Run the service**  
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

## docker compose
With docker compose, you may either run the service as root, or with another user (for instructions, you may follow the user creation step shown in [systemd](#systemd)).

**1. Create `docker-compose.yml` configuration**
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
         - path/to/my/config.toml:/etc/onlyswaps/verifier.toml:ro
       command: ["start", "--config", "/etc/onlyswaps/verifier.toml"]
   ```
2. Update the user id if different than the current user. If executing as root, use `user: "0:0"` (or comment the line).
3. Update the libp2p port in the docker-compose file if different. If you use, say port `8888`, you should replace `7777:7777` with `8888:8888` instead.

**2. Run the service**  
Once the `docker-compose.yml` is ready, you can execute the service with `docker compose up -d`.
After a few seconds, check the logs with `docker compose logs -f` to make sure the verifier has been started.
