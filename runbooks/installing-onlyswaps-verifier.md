# Installing the onlyswaps-verifier CLI

## Prerequisites
- git 
- rustup

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

**3. (Optional) Copy the binary somewhere accessible**
```bash
mv target/release/onlyswaps-verifier /usr/local/bin/
```
