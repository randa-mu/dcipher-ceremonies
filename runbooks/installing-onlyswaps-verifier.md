# Installing the onlyswaps-verifier CLI

## Prerequisites
- git 
- rustup
- protoc > 3.15: through package manager or [binaries](https://protobuf.dev/installation/)
  > ⚠️ Warning  
  > Ubuntu releases prior to 24.04 (noble) have an older version of protoc and require a manual installation.
- build dependencies, for Debian/Ubuntu: build-essential libssl-dev pkg-config

## Steps
**1. Install the latest rust stable toolchain**
```bash
rustup install stable
``` 

**2. Clone and Build the CLI**

```
git clone https://github.com/randa-mu/dcipher
cd dcipher
git submodule update --init --recursive
cargo build --release -p onlyswaps-verifier
```
> [!WARNING]
> If protoc is not in path, or an older version is in path, you may need to use the following build command instead:
> ```bash
> PROTOC=path/to/protoc cargo build --release -p onlyswaps-verifier
> ```

**3. (Optional) Copy the binary somewhere accessible**
```bash
mv target/release/onlyswaps-verifier /usr/local/bin/
```
