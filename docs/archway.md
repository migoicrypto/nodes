# Archway 


## About The Project
[Homepage](https://archway.io/) | [Official Doc](https://docs.archway.io/docs/overview/about)

Archway is an incentivized smart contract platform that rewards developers — ushering in next gen dApps.

The protocol gives developers the tools to quickly build and launch scalable cross-chain dApps and get rewarded for the value the dApps contribute to the network.

----------
## Summary 
- Status: 
    - [x] **Dev Testnet**
    - [x] **Non-incentived Community Testnet**
    - [x] **[Incentived Community Testnet](https://philabs.notion.site/philabs/Archway-Incentivized-Testnet-Torii-1-9e70a8f431c041618c6932e70d46ccdd)**

- Rating: Guarantee
- Date Start: April 11 2022
- Winners Notified: May 30
- Identity Form Available: Identity Form Available
- Identity Verification Deadline: June 20
- Rewards Distribution: Mainnet Launch
- Vesting: N/A


----------
## System Requirements 
- Linux distribution
- x86_64 processor
- 16 GB RAM
- 500 GB to 2 TB storage*
- Storage size for validators depends on level of pruning.

----------
## Augusta Setup Guide

### I. Preparation
1. Need a server to run. You can use either your local server or buy a VPS/VDS from some public cloud services:  
    [Contabo](https://contabo.com/en/vps/) | [Vultr](https://www.vultr.com/?ref=9092122) | [DigitalOcean](https://www.digitalocean.com/?refcode=6c9338218bd0) | [Hetzner](https://www.hetzner.com/cloud)

    Note: I am using CentOS 7 (or higher) in this guide so if you are using other OS, please find a alternative commands if needed.

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

1. Pull docker image  
    ```sh
    docker pull archwaynetwork/archwayd:augusta
    ```
2. Configure environment.  
    **Run one by one below commands, line by line!**

    ```sh
    # Setup required variables
    echo 'export NODENAME='\"INPUT_YOUR_NODE_NAME\" >> $HOME/.bash_profile

    echo 'export WALLET='\"INPUT_YOUR_WALLET_NAME\" >> $HOME/.bash_profile

    echo 'export CHAIN_ID='augusta-1 >> $HOME/.bash_profile

    source ~/.bash_profile

    # Cleanup: Remove old genesis.json if existed
    rm -f ~/.archway/config/genesis.json

    docker rm -f archway 2&>/dev/null

    # Run docker image augusta
    docker run --rm -it -v $HOME/.archway:/root/.archway archwaynetwork/archwayd:augusta init $NODENAME --chain-id $CHAIN_ID

    docker run --rm -it -v $HOME/.archway:/root/.archway archwaynetwork/archwayd:augusta config chain-id $CHAIN_ID

    # Configure required variables
    perl -i -pe 's/^minimum-gas-prices = .+?$/minimum-gas-prices = "0august"/' $HOME/.archway/config/app.toml

    SEEDS="2f234549828b18cf5e991cc884707eb65e503bb2@34.74.129.75:31076,c8890bcde31c2959a8aeda172189ec717fef0b2b@95.216.197.14:26656"

    PEERS="332dea7332a0c4647a147a08bf50bb2038931e4c@81.30.158.46:26656,4e08eb9d62607d05e3fa3fa52d98a00014c8040b@162.55.90.254:26656,4a701d399a0cd4a577e5b30c9d3cc5d75854936e@95.214.53.132:26456,0c019ac4e4f39d95355926435e50a25ed589915f@89.163.151.226:26656,b65efc14137a426a795b5e78cf34def7e5240231@89.163.164.211:26656,33baa872768e12d4100bce5eb875b90b8739a1d4@185.214.134.154:46656,76862fd5ee017b7b46f65a7ac15da12bba12f7f1@49.12.215.72:26656"

    sed -i.bak -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.archway/config/config.toml

    sed -i.bak -e "s/prometheus = false/prometheus = true/" $HOME/.archway/config/config.toml

    curl -s "https://nodes.migoi.io/en/latest/_static/archway/archway_genesis.json" | jq '.result.genesis' > ~/.archway/config/genesis.json

    docker run --rm -it -v $HOME/.archway:/root/.archway archwaynetwork/archwayd:augusta unsafe-reset-all

    wget -O addrbook.json https://nodes.migoi.io/en/latest/_static/archway/addrbook_archway.json

    mv addrbook.json $HOME/.archway/config/

    docker run --restart=always -d -it --network host --name archway -v $HOME/.archway:/root/.archway archwaynetwork/archwayd:augusta start --x-crisis-skip-assert-invariants

    echo "alias archwayd='docker exec -it archway archwayd'" >> $HOME/.bash_profile

    source ~/.bash_profile
    ```

    Now your node is running and in sync with the blockchain to get the latest block heigh. You need to wait few hours to fully synchronized-up.  

    **Carefully: Do not go to step-III if your node is not fully synchronized!**

    There are 2 ways to check is your node fully-synchronized:  
    a. Run below command, if results false – node is fully synchronized  
    ```sh
    curl -s localhost:26657/status | jq .result.sync_info.catching_up
    ```

    b. Go to [Archway Explorer](https://explorer.augusta-1.archway.tech/) and check the current *Latest Block Height*, then run below command on your server to get current Block Height on your node. If these 2 Block Heights are same, so your node is full-synchronized. Otherwise, just wait!
    ```sh
    # Run this command
    docker ps

    # Then you will get CONTAINER ID, copy it into this command `docker logs -n 10 CONTAINER_ID`.
    # Sample: 
    docker logs -n 10 b7cc2b4ab8a

    # you will get something like `indexed block height=553688 module=txindex`
    ```

3. Create a wallet, **don’t forget to save the mnemonic***.  

    **Run only 1 either of below commands**:
    ```sh
    # Run this command if you run node first time to CREATE NEW wallet
    archwayd keys add $WALLET
    # ***Backup the Archway wallet address, mnemonic and password carefully***
    # The mnemonic seed phrase are the line below of line 
    # "**Important** write this mnemonic phrase in a safe place. It is the only way to recover your account if you ever forget your password."

    # Run this command if you already have wallet and mnemonic to RESTORE OLD wallet 
    archwayd keys add $WALLET --recover
    ```

    Now if you don't have $august token, you can join Archway discord server at [https://discord.gg/ztXFAqhPSN](https://discord.gg/ztXFAqhPSN), go to channel #validator-chat, provide your Archway wallet addressand and ask mod to send you some $august token.  

    ```sh
    # Run this command to check your wallet balance
    archwayd q bank balances INPUT_YOUR_ARCHWAY_WALLET_ADDRESS
    ```

### III. Create a Validator  
If your node is fully-synchronized, then you can continue this step.

```sh
# After you get some $august token and node is fully-synchronized, run this command to delegate some token from your wallet to your node.
archwayd tx staking create-validator \
    --amount 9000000uaugust \
    --from $WALLET \
    --commission-max-change-rate "0.01" \
    --commission-max-rate "0.1" \
    --commission-rate "0.01" \
    --min-self-delegation "1" \
    --pubkey  $(archwayd tendermint show-validator) \
    --moniker $NODENAME \
    --chain-id $CHAIN_ID \
    --gas 300000 \
    --fees 3uaugust

# Run this command to get YOUR_ARCHWAY_VALIDATOR_ADDRESS
archwayd keys show $WALLET --bech val -a

# If you wan to stake more token to your node, run this command:
archwayd tx staking delegate INPUT_YOUR_ARCHWAY_VALIDATOR_ADDRESS 10000000uaugust --from $WALLET --chain-id $CHAIN_ID --fees 5000uaugust
```

----------
## Torii Setup Guide
This tutorial is used for the [Run a Genesis Validator](https://philabs.notion.site/Run-a-Genesis-Validator-f54af825e48f488cb12c0440f738e9fb) challenge.  
For other challenges, you can check it on https://philabs.notion.site/Testnet-Challenges-9449524d909c4831ac303ed485194b16

### I. Preparation
1. Need a server to run. You can use either your local server or buy a VPS/VDS from some public cloud services:  
    [Contabo](https://contabo.com/en/vps/) | [Vultr](https://www.vultr.com/?ref=9092122) | [DigitalOcean](https://www.digitalocean.com/?refcode=6c9338218bd0) | [Hetzner](https://www.hetzner.com/cloud)

    Note: I am using CentOS 7 (or higher) in this guide so if you are using other OS, please find a alternative commands if needed.

2. After startup your server, then you need to use ssh potocol to access your server. If you don't know how to use ssh, you can refer to use Putty from [this tutorial](https://www.hostinger.com/tutorials/how-to-use-putty-ssh).

3. Install some libraries/services   
    3.1 Install needed library
    ```sh
    yum install epel-release -y

    yum update -y

    yum install jq -y

    yum install git

    yum group install "Development Tools"

    ```

### II. Install & Sync Node  
You need to install and sync your node before run a validator.  
**Note**: Must finish step 1 and 2 durring 12PM 11/Apr - 12PM 12/Apr.

1. [Install Archway binary](https://www.notion.so/Install-Archway-binary-3340b0901cff4bd099288d309a63c28f)
    1. Retrieve the source code
    
        ```bash
        git clone https://github.com/archway-network/archway
        cd archway
        git checkout main
        ```
    2. Install archwayd binary
        ```bash
        make install

        # Wait few minutes to install archwayd binary, then you can try `archwayd --help` to check. if ok, will show something as "Archway Daemon (server)"
        archwayd --help
        ```

2. [Set up your node pre-genesis](https://www.notion.so/Configure-your-validator-pre-genesis-6fcdbd34302c489dbd4c2611ebbc63d5)

    1. Initialize data in daemon’s home directory:
        
        ```bash
        mkdir $HOME/.archway
        echo 'export HOMEDIR='$HOME/.archway >> $HOME/.bash_profile
        echo 'export NODE_NAME='INPUT_YOUR_NODE_NAME >> $HOME/.bash_profile
        echo 'export WALLET_NAME='INPUT_YOUR_WALLET_NAME >> $HOME/.bash_profile
        source $HOME/.bash_profile

        archwayd init $NODE_NAME --chain-id torii-1 --home $HOMEDIR
        ```
        
    2. Download genesis from [https://raw.githubusercontent.com/archway-network/testnets/main/torii-1/penultimate_genesis.json](https://raw.githubusercontent.com/archway-network/testnets/main/torii-1/penultimate_genesis.json) and copy it in the `$HOMEDIR` 
        
        ```bash
        wget https://raw.githubusercontent.com/archway-network/testnets/main/torii-1/penultimate_genesis.json
        cp penultimate_genesis.json $HOMEDIR/config/genesis.json
        ```
    
    **Add key to your client keystore and fund account:**
    1. Create a new key via the following command. Please note that the key will be stored in keystore configured by you earlier.
        
        ```bash
        archwayd keys add $WALLET_NAME --home $HOMEDIR
            #Enter keyring passphrase:
            ## input the passphrase in the terminal
        ```
        
    2. Add your account in genesis with some funds:
    **Note:** Validator tokens for genesis should be 10000000utorii
        ```bash
        archwayd add-genesis-account $(archwayd keys show $WALLET_NAME -a --home $HOMEDIR)  10001000utorii --home $HOMEDIR
        ```

    **Add yourself as validator in genesis:**
    1. Create a `gentx` which contains the `MsgCreateValidator`:
        
        ```bash
        archwayd gentx $WALLET_NAME 10000000utorii \
        --commission-rate 0.1 \
        --commission-max-rate 0.1 \
        --commission-max-change-rate 0.1 \
        --note 'MsgCreateValidator' \
        --pubkey $(archwayd tendermint show-validator --home $HOMEDIR) \
        --home $HOMEDIR \
        --chain-id torii-1
            #Enter keyring passphrase: [input your passphrase when you add the wallet above]
            #Genesis transaction written to "/root/.archway/config/gentx/gentx-xxxxxxxxxxxxx.json" <- we will need to submit this file to the testnet github repo
        ```
        
        
    2.  Submit PR to [archway-network/testnets](https://github.com/archway-network/testnets) repository  
        now go to https://github.com/, login to your account and go to https://github.com/archway-network/testnets.git, click on ***Fork*** button.  
        Now you go back to your github account, you can see the Archway testnets repo inside your github account.   
        ```bash
        # On your node server, run command to create new ssh key
        ssh-keygen -t ed25519 -C "your_mail@xyz.mail"
        
        # Replace id_XXXXXXX.pub with your ssh file
        cat /root/.ssh/id_XXXXXXX.pub

        # copy the content then paste to next step `ssh-ed25519 XXXXXXXXYYYYYYYYYZZZZZZZZZ your_mail@xyz.mail`

        ```
        Then you need to add ssh key to your github account to submit file. Go to https://github.com/settings/keys, click ***New SSH Key*** button and paste the ssh key content that you get above step.

        ```bash
        # on node server:
        # Replace id_XXXXXXX with your ssh file
        eval "$(ssh-agent -s)"
        ssh-add /root/.ssh/id_XXXXXXX

        # do change CHANGE_TO_YOUR_GITHUB_ACCOUNT
        git clone git@github.com:CHANGE_TO_YOUR_GITHUB_ACCOUNT/testnets.git testnets
        mkdir -p testnets/torii-1/gentx/
        cp $HOMEDIR/config/gentx/*.json testnets/torii-1/gentx/
        cd ./testnets
        git add .
        git commit -m 'add gentx file'
        git push
        ```
        
    3. Open up a PR to the [archway-network/testnets](https://github.com/archway-network/testnets) repository and wait for it to be merged.  
    Go to the archway testnet repo on your github account, click on Pull Requests tab,  click ***New Pull Request*** button, you can see there is a gentx file (the name is same as your in above steps), click ***Create Pull Request*** button, put some note and click ***Create Pull Request*** button to submit PR.  
    ***Go to [form](https://docs.google.com/forms/d/e/1FAIpQLScAWscjXibUoBoyua7GLSUFIfhhWGRoRAgLHsSfQHejPyMSgQ/viewform) to fillout the form.  
    Now you can wait until 12PM 12/Apr to continue on next step.

    4. After the `gentx` submission window is passed, you need to get `final_genesis.json` from  [archway-network/testnets](https://github.com/archway-network/testnets) repository and replace the `genesis.json` in `$HOMEDIR` Please run below commands in same directory as Step 2.
            
        ```bash
        rm -rf testnets
        git clone git@github.com:archway-network/testnets.git
        cp testnets/torii-1/final_genesis.json $HOMEDIR/config/genesis.json
        ```
            
3. [Start your node](https://www.notion.so/Start-your-full-node-b0ce46f1228648bc99eaedeff5dd3fd5)
