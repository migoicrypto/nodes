# Subspace 


## About The Project
[Homepage](https://subspace.network/) | [Official Doc](https://github.com/subspace/subspace/blob/main/docs/farming.md)

Subspace is a fourth generation blockchain built for the next wave of crypto creators.

----------
## Summary 
- Status: 
    - [x] **Dev Testnet**
    - [x] **Non-incentived Community Testnet**
    - [ ] **Incentived Community Testnet** (Apr/2022 - TBC)

- Rating: Positive
- Date: Apr/2022 (TBC)
- Rewards: N/A
- Vesting: N/A


----------
## System Requirements 
- OS: If you don't know what you're doing, Ubuntu 20.04+
- CPU: 2 GHz | 2+ Cores
- RAM: 2 GB
- SSD: N/A
- Bandwidth: N/A

----------
## Setup Guide

### I. Preparation
1. Need a server to run. You can use either your local server or buy a VPS/VDS from some public cloud services:  
    [Contabo](https://contabo.com/en/vps/) | [Vultr](https://www.vultr.com/?ref=9092122) | [DigitalOcean](https://www.digitalocean.com/?refcode=6c9338218bd0) | [Hetzner](https://www.hetzner.com/cloud)

    Note: I am using Ubuntu 20.04 (64 Bit) in this guide so if you are using other OS, please find a alternative commands if needed.

2. After startup your server, then you need to use ssh potocol to access your server. If you don't know how to use ssh, you can refer to use Putty from [this tutorial](https://www.hostinger.com/tutorials/how-to-use-putty-ssh).


### II. Install & Sync Node  
You need to install and sync your node before run a validator.  

1. Download
    ```sh
    wget https://github.com/subspace/subspace/releases/download/snapshot-2022-mar-09/subspace-node-ubuntu-x86_64-snapshot-2022-mar-09

    wget https://github.com/subspace/subspace/releases/download/snapshot-2022-mar-09/subspace-farmer-ubuntu-x86_64-snapshot-2022-mar-09

    chmod +x subspace-*
    ```
2. Run to sync node

    ```sh
    # I used nohub to run command as background process, so it will not be terminated when you close your ssh connection to the server.
    # Replace `INSERT_YOUR_ID` with a nickname you choose
    # Copy all of the lines below, they are all part of the same command
    nohup ./subspace-node-ubuntu-x86_64-snapshot-2022-mar-09 \
    --chain testnet \
    --wasm-execution compiled \
    --execution wasm \
    --bootnodes "/dns/farm-rpc.subspace.network/tcp/30333/p2p/12D3KooWPjMZuSYj35ehced2MTJFf95upwpHKgKUrFRfHwohzJXr" \
    --rpc-cors all \
    --rpc-methods unsafe \
    --ws-external \
    --validator \
    --telemetry-url "wss://telemetry.polkadot.io/submit/ 1" \
    --name INSERT_YOUR_ID  > node.log &

    ```

    View log file:

    ```sh
    # You should see something similar in the **node.log** file.  
    tail -f node.log

    # For example, The **best: #345816** from the log is the current block on your node, and the **target=#412043** is the latest block. If both of them are same, so your node is in synchronized.   

    2022-02-03 10:52:23 Subspace
    2022-02-03 10:52:23 ✌️  version .....
    2022-02-03 10:52:23 ❤️  by Subspace Labs <https://subspace.network>, 2021-2022
    2022-02-03 10:52:23 📋 Chain specification: Subspace testnet
    2022-02-03 10:52:23 🏷  Node name: YOUR_FANCY_NAME
    2022-02-03 10:52:23 👤 Role: AUTHORITY
    ....
    2022-04-04 05:26:55 ⚙️  Syncing 130.9 bps, target=#412043 (47 peers), best: #345816 (0xc9bf…2e5c), finalized #0 (0x332e…4a74), ⬇ 1.9MiB/s ⬆ 2.0kiB/s

    # Press ctr + C to exit view log file.
    ```

    Then wait until your node is fully synchronized. You can check the latest block at [Subspace exploere](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Ffarm-rpc.subspace.network#/explorer).


3. Create a test wallet to receive reward:  
    During this waiting time, you can go to [https://polkadot.js.org/extension/](https://polkadot.js.org/extension/), install polkadot extention and create your own wallet for the testnet.  
    When you do farm, the reward will be sent to this wallet.

    **Importance**: Please save your wallet address and seed phrase if you don't want to loose your rewards.  

4. After your node is fully synced, Now run your farmer to farm reward.  
    ```sh
    # Replace `WALLET_ADDRESS` below with your account address from Polkadot.js wallet (Step II.3)
    nohup ./subspace-farmer-ubuntu-x86_64-snapshot-2022-mar-09 farm --reward-address WALLET_ADDRESS  > farm.log &

    # View log file
    tail -f farm.log

    ```  
  
  
The original document from project at [here](https://github.com/subspace/subspace/blob/main/docs/farming.md).


