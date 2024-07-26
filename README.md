
![0G-bg-2](https://github.com/user-attachments/assets/f0812d9c-e476-4aef-a28f-c33fe7d0f926)

# Guide_0g_storage_node
This guide will help you install storage node project 0g. 

## Hardware Requirement
```bash
- Memory: 16 GB RAM
- CPU: 4 cores
- Disk: 500GB / 1T NVME SSD
- Bandwidth: 500 MBps for Download / Upload
```

## Install dependencies
```bash
sudo apt-get update
sudo apt-get install clang cmake build-essential
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
```

## Download The source code
```bash
git clone -b v0.3.3 https://github.com/0glabs/0g-storage-node.git
```

## Build The source code
```bash
cd 0g-storage-node
git submodule update --init
cargo build --release
```

## Check and update the `run/config.toml` if necessary.
```toml
# enr address, must fill your instance's public ip to support peer discovery
network_enr_address

# peer nodes, we provided 3 nodes with last one being HK region, you can also modify to your own ips
network_boot_nodes = ["/ip4/54.219.26.22/udp/1234/p2p/16Uiu2HAmTVDGNhkHD98zDnJxQWu3i1FL1aFYeh9wiQTNu4pDCgps","/ip4/52.52.127.117/udp/1234/p2p/16Uiu2HAkzRjxK2gorngB1Xq84qDrT4hSVznYDHj6BkbaE4SGx9oS","/ip4/18.167.69.68/udp/1234/p2p/16Uiu2HAm2k6ua2mGgvZ8rTMV8GhpW71aVzkQWy7D37TTDuLCpgmX"]

# flow contract address
log_contract_address

# mine contract address
mine_contract_address

# layer one blockchain rpc endpoint
blockchain_rpc_endpoint

# block number to start the sync
log_sync_start_block_number

# your private key with 64 length
# do not include leading 0x
# do not omit leading 0
# must fill if you want to participate in the pora and get mining reward
miner_key

# The max number of chunk entries to store in db.
# Each entry is 256B, so the db size is roughly limited to
# `256 * db_max_num_chunks` Bytes.
# If this limit is reached, the node will update its `shard_position`
# and store only half data.
db_max_num_chunks
```

## Run
```bash
cd run

# consider using tmux in order to run in background
../target/release/zgs_node --config config-testnet.toml --miner-key <your_private_key> --blockchain-rpc-endpoint <blockchain_rpc> --db-max-num-chunks <max_chunk_num>
```
