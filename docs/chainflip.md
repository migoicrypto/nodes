# Chainflip 


## About The Project
[Homepage](https://chainflip.io/) | [Official Doc](https://chainflip.gitbook.io/soundcheck-validator-documentation/)

Chainflip is a decentralised, trustless protocol that enables cross chain swaps between different blockchains.

----------
## Summary 
- Status: 
    - [x] **Incentived Community Testnet**

- Rating: Guarantee
- Date: Dec/2021
- Rewards: N/A
- Vesting: N/A


----------
## System Requirements 
- OS: If you don't know what you're doing, Ubuntu 20.04+ or Debian 11+
- CPU: 4 GHz | 4+ Cores, Dedicated is better
- RAM: 8 GB
- SSD: 50 GB (this may increase over time)
- Bandwidth: Recommended 1GBps connection, 100 GB bandwidth combined up/down per month

----------
## Setup Guide

### I. Preparation
1. Need a server to run. You can use either your local server or buy a VPS/VDS from some public cloud services:  
    [Contabo](https://contabo.com/en/vps/) | [Vultr](https://www.vultr.com/?ref=9092122) | [DigitalOcean](https://www.digitalocean.com/?refcode=6c9338218bd0) | [Hetzner](https://www.hetzner.com/cloud)

    Note: I am using Ubuntu 20.04 (64 Bit) in this guide so if you are using other OS, please find a alternative commands if needed.

2. After startup your server, then you need to use ssh potocol to access your server. If you don't know how to use ssh, you can refer to use Putty from [this tutorial](https://www.hostinger.com/tutorials/how-to-use-putty-ssh).

3. Install some libraries/services   
    3.1 Install jq
    ```sh
    yum install epel-release -y

    yum update -y

    yum install jq -y
    ```
    3.1 Install docker to run node -> [Install Docker Guide](https://docs.docker.com/engine/install/centos/)  
    3.2 [Post-Docker-Install Steps](https://docs.docker.com/engine/install/linux-postinstall/)

### II. Install & Sync Node  
You need to install and sync your node before run a validator.  

1. https://chainflip.gitbook.io/soundcheck-validator-documentation/validator-setup/installation
    ```sh
    mkdir ~/chainflip

    chown $USER ~/chainflip

    cd ~/chainflip
    ```
2. Download Via Latest Github Release
First we need to figure out the name of the github release. head to https://github.com/chainflip-io/chainflip-bin/releases to see the latest name. For example, the first release is v0.1.0-soundcheck.  
Replace INSERTTAGHERE with the name of the latest release (as of 06-01-2022 this is v0.1.1):

    ```sh
    wget https://github.com/chainflip-io/chainflip-bin/releases/download/INSERTTAGHERE/chainflip.tar.gz

    tar -xvf chainflip.tar.gz

    chmod +x ~/chainflip/chainflip-v0.1.1/chainflip-*

    ```

    To Be Cont!
