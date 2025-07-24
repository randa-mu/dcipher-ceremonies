# Installation the DKG CLI

## Prerequisites
- git 
- rustup

## Steps
1. Install the latest rust stable toolchain
```bash
rustup install stable
``` 

2. Clone and Build the CLI

```
git clone https://github.com/randa-mu/dcipher
cd dcipher/crates/adkg-cli
cargo build --release
```

3. (Optional) Copy the binary somewhere accessible
```bash
mv target/release/adkg-cli /usr/local/bin/
```


