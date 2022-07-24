# Near Stake Wars 


## About The Project
[Homepage](https://near.org/stakewars/) | [Official Doc](https://github.com/near/stakewars-iii)

NEAR is a collective, a foundation, and a development platform built on a new layer-one blockchain.

Participate in Stake Wars III and join NEAR’s mission to bring ‘Chunk-Only Producers’ to Mainnet.  

Chunk-Only Producers are introduced by reaching Sharding Phase 1, and bring a more accessible role for those who may not have the $NEAR required to run a Block Producer node. This new role will allow the validator number to grow, creating more opportunities to earn rewards and secure the NEAR Ecosystem.

----------
## Summary 
- Status: 
    - [RUNNING] **[NEAR Stake Wars: Episode III Incentivized Testnet](https://near.org/stakewars/)**

- Rating: Guarantee
- Date Start: Jul 2022
- End Start: Sep 2022
- Rewards Distribution: 
    - Delegated NEAR Points (DNP): at the end of the Stake Wars program, each Delegated NEAR Point (DNP) will be translated into 500 NEAR tokens delegated to your mainnet account for 1 year.
    - Unlocked NEAR Points (UNP): at the end of the Stake Wars program, each Unlocked NEAR Point (UNP) will be translated into 1 unlocked NEAR token, granted to your mainnet account.


----------
## System Requirements 
- Linux distribution: Ubuntu is referred
- 4-Core CPU with AVX support
- 8 GB RAM
- 500 GB SSD

----------
## A. Preparation
1. Need a server to run. You can use either your local server or buy a VPS/VDS from some public cloud services:  
    [Contabo](https://contabo.com/en/vps/) | [Vultr](https://www.vultr.com/?ref=9092122) | [DigitalOcean](https://www.digitalocean.com/?refcode=6c9338218bd0) | [Hetzner](https://www.hetzner.com/cloud) | AWS | AZURE | CGP

    Note: I am using Ubuntu 20 in this guide so if you are using other OS, please find a alternative commands if needed.

2. After startup your server, then you need to use ssh potocol to access your server. If you don't know how to use ssh, you can refer to use Putty from [this tutorial](https://www.hostinger.com/tutorials/how-to-use-putty-ssh).

## B. STAKE WARS III challenges
There are 9 [challenges](https://github.com/near/stakewars-iii/blob/main/challenges/challenge-summary.md) at this time.  
We will go through these challenges together then submit the result.

### I. Challenge 001 
[Ogirinal Document](https://github.com/near/stakewars-iii/blob/main/challenges/001.md)  
Create your Shardnet wallet & deploy the NEAR CLI. This is designed to be your very first challenge: use it to understand how staking on NEAR works.  
As my experience, you can install Near CLI on the Node that you are planning to install the validator node.   

#### 1. Reward & Submission
- *Rewards: No reward but need to do to unlocked at the end of Challenge 002.*  
- *No submission si required for this challenge; it will be evaluated together with the next one (002).*

#### 2. Challenge Step By Step
- Create NEAR Wallet:  
Go to https://wallet.shardnet.near.org/, click **Create Account** button, input the name of your account into Account ID text box.  
Then click **Reserve My Account ID** button, select **Secure Passphrase** and click Continue button. Now you need to backout the 12 seed words (passpharse).   
Now input word as request to verify the account.  
Finally, you will get your near account. We will need this account to use in challenge 002.  
For example in my case:  
Account ID: tonyxoac  
Wallet: tonyxoac.shardnet.near  
Seedphrase: WORD1-..-....-..-WORD12  

- Setup NEAR-CLI: We will use this near CLI to run command for staking, check validator node's status and so on.  
    ```sh
    # Update apt
    sudo apt update && sudo apt upgrade -y

    # Install Node.js and npm
    curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
    sudo apt install build-essential nodejs
    PATH="$PATH"

    # Check version of node and npm to verify the installation
    node -v
    v18.6.0

    npm -v
    8.13.2

    # Install near-cli
    sudo npm install -g near-cli

    # The environment will need to be set to select the correct network.
    # For this chunk-only producer, we'll be using shardnet network
    echo 'export NEAR_ENV=shardnet' >> ~/.bashrc && source .bashrc
    ```
- Near CLI Commands.  
Full list of Near CLI commands: https://github.com/near/near-cli  
Some useful commands: 

    ```sh
    # A proposal by a validator indicates they would like to enter the validator set, in order for a proposal to be accepted it must meet the minimum seat price.
    near proposals

    # This shows a list of active validators in the current epoch, the number of blocks produced, number of blocks expected, and online rate. Used to monitor if a validator is having issues.
    near validators current

    # This shows validators whose proposal was accepted one epoch ago, and that will enter the validator set in the next epoch.
    near validators next
    ```

**near proposal list**
![near proposals](_static/near/near_proposals.png "near proposal list")

**current near proposal list**
![near proposals current](_static/near/near_validators_current.png "near proposal current")

**next near proposal list**
![near proposals next](_static/near/near_validators_next.png "near proposal next")

### II. Challenge 002 
[Ogirinal Document](https://github.com/near/stakewars-iii/blob/main/challenges/002.md)  
This challenge is focused on deploying a node (nearcore), downloading a snapshot, syncing it to the actual state of the network, then activating the node as a validator.   

#### 1. Reward & Submission
- *Rewards: 30 Unlocked NEAR Points (UNP)*  
- *No submission si required for this challenge; it will be evaluated together with the next one (002).*

#### 2. Challenge Step By Step
- Create NEAR Wallet:  
Go to https://wallet.shardnet.near.org/, click **Create Account** button, input the name of your account into Account ID text box.  
Then click **Reserve My Account ID** button, select **Secure Passphrase** and click Continue button. Now you need to backout the 12 seed words (passpharse).   
Now input word as request to verify the account.  
Finally, you will get your near account. We will need this account to use in challenge 002.  
For example in my case:  
Account ID: tonyxoac  
Wallet: tonyxoac.shardnet.near  
Seedphrase: WORD1-..-....-..-WORD12  

- Setup NEAR-CLI: We will use this near CLI to run command for staking, check validator node's status and so on.  
    ```sh
    # Update apt
    sudo apt update && sudo apt upgrade -y

    # Install Node.js and npm
    curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
    sudo apt install build-essential nodejs
    PATH="$PATH"

    # Check version of node and npm to verify the installation
    node -v
    v18.6.0

    npm -v
    8.13.2

    # Install near-cli
    sudo npm install -g near-cli

    # The environment will need to be set to select the correct network.
    # For this chunk-only producer, we'll be using shardnet network
    echo 'export NEAR_ENV=shardnet' >> ~/.bashrc && source .bashrc
    ```
- Near CLI Commands.  
Full list of Near CLI commands: https://github.com/near/near-cli  
Some useful commands: 

    ```sh
    # A proposal by a validator indicates they would like to enter the validator set, in order for a proposal to be accepted it must meet the minimum seat price.
    near proposals

    # This shows a list of active validators in the current epoch, the number of blocks produced, number of blocks expected, and online rate. Used to monitor if a validator is having issues.
    near validators current

    # This shows validators whose proposal was accepted one epoch ago, and that will enter the validator set in the next epoch.
    near validators next
    ```

### II. Challenge 003 
[Ogirinal Document](https://github.com/near/stakewars-iii/blob/main/challenges/003.md)  
Create your Shardnet wallet & deploy the NEAR CLI. This is designed to be your very first challenge: use it to understand how staking on NEAR works.  
As my experience, you can install Near CLI on the Node that you are planning to install the validator node.   

#### 1. Reward & Submission
- *Rewards: unlocked at the end of Challenge 002.*  
- *No submission si required for this challenge; it will be evaluated together with the next one (002).*

#### 2. Challenge Step By Step
- Create NEAR Wallet:  
Go to https://wallet.shardnet.near.org/, click **Create Account** button, input the name of your account into Account ID text box.  
Then click **Reserve My Account ID** button, select **Secure Passphrase** and click Continue button. Now you need to backout the 12 seed words (passpharse).   
Now input word as request to verify the account.  
Finally, you will get your near account. We will need this account to use in challenge 002.  
For example in my case:  
Account ID: tonyxoac  
Wallet: tonyxoac.shardnet.near  
Seedphrase: WORD1-..-....-..-WORD12  

- Setup NEAR-CLI: We will use this near CLI to run command for staking, check validator node's status and so on.  
    ```sh
    # Update apt
    sudo apt update && sudo apt upgrade -y

    # Install Node.js and npm
    curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
    sudo apt install build-essential nodejs
    PATH="$PATH"

    # Check version of node and npm to verify the installation
    node -v
    v18.6.0

    npm -v
    8.13.2

    # Install near-cli
    sudo npm install -g near-cli

    # The environment will need to be set to select the correct network.
    # For this chunk-only producer, we'll be using shardnet network
    echo 'export NEAR_ENV=shardnet' >> ~/.bashrc && source .bashrc
    ```
- Near CLI Commands.  
Full list of Near CLI commands: https://github.com/near/near-cli  
Some useful commands: 

    ```sh
    # A proposal by a validator indicates they would like to enter the validator set, in order for a proposal to be accepted it must meet the minimum seat price.
    near proposals

    # This shows a list of active validators in the current epoch, the number of blocks produced, number of blocks expected, and online rate. Used to monitor if a validator is having issues.
    near validators current

    # This shows validators whose proposal was accepted one epoch ago, and that will enter the validator set in the next epoch.
    near validators next
    ```

### IV. Challenge 004 
[Ogirinal Document](https://github.com/near/stakewars-iii/blob/main/challenges/004.md)  
Create your Shardnet wallet & deploy the NEAR CLI. This is designed to be your very first challenge: use it to understand how staking on NEAR works.  
As my experience, you can install Near CLI on the Node that you are planning to install the validator node.   

#### 1. Reward & Submission
- *Rewards: unlocked at the end of Challenge 002.*  
- *No submission si required for this challenge; it will be evaluated together with the next one (002).*

#### 2. Challenge Step By Step
- Create NEAR Wallet:  
Go to https://wallet.shardnet.near.org/, click **Create Account** button, input the name of your account into Account ID text box.  
Then click **Reserve My Account ID** button, select **Secure Passphrase** and click Continue button. Now you need to backout the 12 seed words (passpharse).   
Now input word as request to verify the account.  
Finally, you will get your near account. We will need this account to use in challenge 002.  
For example in my case:  
Account ID: tonyxoac  
Wallet: tonyxoac.shardnet.near  
Seedphrase: WORD1-..-....-..-WORD12  

- Setup NEAR-CLI: We will use this near CLI to run command for staking, check validator node's status and so on.  
    ```sh
    # Update apt
    sudo apt update && sudo apt upgrade -y

    # Install Node.js and npm
    curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
    sudo apt install build-essential nodejs
    PATH="$PATH"

    # Check version of node and npm to verify the installation
    node -v
    v18.6.0

    npm -v
    8.13.2

    # Install near-cli
    sudo npm install -g near-cli

    # The environment will need to be set to select the correct network.
    # For this chunk-only producer, we'll be using shardnet network
    echo 'export NEAR_ENV=shardnet' >> ~/.bashrc && source .bashrc
    ```
- Near CLI Commands.  
Full list of Near CLI commands: https://github.com/near/near-cli  
Some useful commands: 

    ```sh
    # A proposal by a validator indicates they would like to enter the validator set, in order for a proposal to be accepted it must meet the minimum seat price.
    near proposals

    # This shows a list of active validators in the current epoch, the number of blocks produced, number of blocks expected, and online rate. Used to monitor if a validator is having issues.
    near validators current

    # This shows validators whose proposal was accepted one epoch ago, and that will enter the validator set in the next epoch.
    near validators next
    ```

### V. Challenge 005 
[Ogirinal Document](https://github.com/near/stakewars-iii/blob/main/challenges/005.md)  
Create your Shardnet wallet & deploy the NEAR CLI. This is designed to be your very first challenge: use it to understand how staking on NEAR works.  
As my experience, you can install Near CLI on the Node that you are planning to install the validator node.   

#### 1. Reward & Submission
- *Rewards: unlocked at the end of Challenge 002.*  
- *No submission si required for this challenge; it will be evaluated together with the next one (002).*

#### 2. Challenge Step By Step
- Create NEAR Wallet:  
Go to https://wallet.shardnet.near.org/, click **Create Account** button, input the name of your account into Account ID text box.  
Then click **Reserve My Account ID** button, select **Secure Passphrase** and click Continue button. Now you need to backout the 12 seed words (passpharse).   
Now input word as request to verify the account.  
Finally, you will get your near account. We will need this account to use in challenge 002.  
For example in my case:  
Account ID: tonyxoac  
Wallet: tonyxoac.shardnet.near  
Seedphrase: WORD1-..-....-..-WORD12  

- Setup NEAR-CLI: We will use this near CLI to run command for staking, check validator node's status and so on.  
    ```sh
    # Update apt
    sudo apt update && sudo apt upgrade -y

    # Install Node.js and npm
    curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
    sudo apt install build-essential nodejs
    PATH="$PATH"

    # Check version of node and npm to verify the installation
    node -v
    v18.6.0

    npm -v
    8.13.2

    # Install near-cli
    sudo npm install -g near-cli

    # The environment will need to be set to select the correct network.
    # For this chunk-only producer, we'll be using shardnet network
    echo 'export NEAR_ENV=shardnet' >> ~/.bashrc && source .bashrc
    ```
- Near CLI Commands.  
Full list of Near CLI commands: https://github.com/near/near-cli  
Some useful commands: 

    ```sh
    # A proposal by a validator indicates they would like to enter the validator set, in order for a proposal to be accepted it must meet the minimum seat price.
    near proposals

    # This shows a list of active validators in the current epoch, the number of blocks produced, number of blocks expected, and online rate. Used to monitor if a validator is having issues.
    near validators current

    # This shows validators whose proposal was accepted one epoch ago, and that will enter the validator set in the next epoch.
    near validators next
    ```

### VI. Challenge 006 
[Ogirinal Document](https://github.com/near/stakewars-iii/blob/main/challenges/005.md)  
Create your Shardnet wallet & deploy the NEAR CLI. This is designed to be your very first challenge: use it to understand how staking on NEAR works.  
As my experience, you can install Near CLI on the Node that you are planning to install the validator node.   

#### 1. Reward & Submission
- *Rewards: unlocked at the end of Challenge 002.*  
- *No submission si required for this challenge; it will be evaluated together with the next one (002).*

#### 2. Challenge Step By Step
- Create NEAR Wallet:  
Go to https://wallet.shardnet.near.org/, click **Create Account** button, input the name of your account into Account ID text box.  
Then click **Reserve My Account ID** button, select **Secure Passphrase** and click Continue button. Now you need to backout the 12 seed words (passpharse).   
Now input word as request to verify the account.  
Finally, you will get your near account. We will need this account to use in challenge 002.  
For example in my case:  
Account ID: tonyxoac  
Wallet: tonyxoac.shardnet.near  
Seedphrase: WORD1-..-....-..-WORD12  

- Setup NEAR-CLI: We will use this near CLI to run command for staking, check validator node's status and so on.  
    ```sh
    # Update apt
    sudo apt update && sudo apt upgrade -y

    # Install Node.js and npm
    curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
    sudo apt install build-essential nodejs
    PATH="$PATH"

    # Check version of node and npm to verify the installation
    node -v
    v18.6.0

    npm -v
    8.13.2

    # Install near-cli
    sudo npm install -g near-cli

    # The environment will need to be set to select the correct network.
    # For this chunk-only producer, we'll be using shardnet network
    echo 'export NEAR_ENV=shardnet' >> ~/.bashrc && source .bashrc
    ```
- Near CLI Commands.  
Full list of Near CLI commands: https://github.com/near/near-cli  
Some useful commands: 

    ```sh
    # A proposal by a validator indicates they would like to enter the validator set, in order for a proposal to be accepted it must meet the minimum seat price.
    near proposals

    # This shows a list of active validators in the current epoch, the number of blocks produced, number of blocks expected, and online rate. Used to monitor if a validator is having issues.
    near validators current

    # This shows validators whose proposal was accepted one epoch ago, and that will enter the validator set in the next epoch.
    near validators next
    ```
### VII. Challenge 007 (TBD)
### VIII. Challenge 008  (TBD)
### IX. Challenge 009  (TBD)