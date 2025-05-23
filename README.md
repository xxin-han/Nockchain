
# 🧠 What is Nockchain?

Nockchain is a lightweight blockchain for heavyweight compute.
It introduces a new consensus mechanism called ZK-Proof of Work (zkPoW) — combining zero-knowledge proofs with mining.



## 💰 Tokenomics
Token: $NOCK

Distribution: Earned by miners submitting valid ZKPs

No Pre-Mine: 100% fair launch — rewards go to those who compute.

## Mainnet Network Details
- Mainnet to be launched in 21th May.
- 10-min block times (like Bitcoin)
- Once Mainnet live, public peers are published, so we just need to run Miner alone. It - - connects to public peers.

 ## Hardware Requirements
| Miner CPU-based initially        

|RAM   | CPU  | DISK  |
|:-:|:-:|:-:|
| 32GB  | 6 + | 200 +  |

* more CPU = more hashrate = more chances
* I'm currently using [Virtarix](https://my.virtarix.com/aff.php?aff=59) VPS. You can choose between a Cloud VPS or a VDS, but for mining purposes, I highly recommend using a VDS for better performance.


## CLI Installation Steps
### Step 1: Install Dependecies
* Update Packages
```bash
  sudo apt-get update && sudo apt-get upgrade -y
  ``` 

* Install Packages:

```bash
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```
* Rust:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

```bash
source $HOME/.cargo/env
```



### Step 2: NockChain Repo
```bash
git clone https://github.com/zorp-corp/nockchain
```

```bash
cd nockchain
```

### Step 3: Build
* Copy the example environment file and rename it to .env:
```bash
cp .env_example .env
```

* Build: (takes time depending on your hardware.)

```bash
make install-hoonc
```

```bash
export PATH="$HOME/.cargo/bin:$PATH"
```

```bash
make build
```

```bash
make install-nockchain-wallet
```

```bash
export PATH="$HOME/.cargo/bin:$PATH"
```

```bash
make install-nockchain
```

```bash
export PATH="$HOME/.cargo/bin:$PATH"
```

### Step 4: Setup Wallet
 * Generate a new key pair:
 ```bash
 nockchain-wallet keygen
 ```
> **N:** Save memo, private key & public key of your wallet.

* Replace the value of MINING_PUBKEY=<public-key> in .env with your own Public Key:
```bash
nano .env
```

To save: Press Ctrl + X, Y & Enter.

### Step 5: Backup/Import Wallet Keys

* Backup wallet keys:
```bash
nockchain-wallet export-keys
```
> **N:** This will save your keys to a file called keys.export in the current directory.

* Import wallet keys:
```bash
nockchain-wallet import-keys --input keys.export
```

Make sure keys.export is in your nockchain directory.

### Step 6: Run Miner

* Enable memory overcommit with this:
```bash
sudo sysctl -w vm.overcommit_memory=1
```

> **N:** Make sure your current directory is nockchain.

* Make new Screen
```bash
screen -S miner
```

* Run Node + Miner
```bash
bash ./scripts/run_nockchain_miner.sh 
```

* Run Node Only 
```bash
bash ./scripts/run_nockchain_node.sh
```

> **N:** To minimize screen: Ctrl + A + D


### FAQ

#### Can I use same pubkey if running multiple miners?
Yes, you can use the same pubkey if running multiple miners.

#### How do I change the mining pubkey?
Run nockchain-wallet keygen to generate a new key pair.

If you are using the Makefile workflow, copy the public key to the .env file.

#### How do I run multiple instances on the same machine?
To run multiple instances on the same machine, you need to:

* Create separate working directories for each instance
* Use different ports for each instance

Here's how to set it up:

```bash

#Create directories for each instance
mkdir node1 node2

#Copy .env to each directory
cp .env node1/
cp .env node2/

#Run each instance in its own directory with .env loaded
cd node1 && bash ../scripts/run_nockchain_miner.sh
cd node2 && bash ../scripts/run_nockchain_miner.sh
```

#### How do I know if it's mining?
If you see a line that looks like:

```bash
[%mining-on 12.040.301.481.503.404.506 17.412.404.101.022.637.021 1.154.757.196.846.835.552 12.582.351.418.886.020.622 6.726.267.510.179.724.279]
```

#### How do I check block height?
If you see a line that looks like:

```bash
block Vo3d2Qjy1YHMoaHJBeuQMgi4Dvi3Z2GrcHNxvNYAncgzwnQYLWnGVE added to validated blocks at 2
```
![Screenshot 2025-05-23 151852](https://github.com/user-attachments/assets/bdd4cac2-3e0b-41ec-aa02-40b628546b39)


#### How do I check wallet balance?

```bash
# List all notes (UTXOs) that your node has seen
nockchain-wallet --nockchain-socket ./nockchain.sock list-notes

# List all notes by pubkey
nockchain-wallet --nockchain-socket ./nockchain.sock list-notes-by-pubkey <your-pubkey>
```

## Useful Commands:
* Restart Miner:
If you ever stopped your Miner, to restart it again, delete old data files first:

```bash
rm -rf ./.data.nockchain .socket/nockchain_npc.sock
```

Screen commands

* Ensure screens do not overlap. Before opening or switching to another screen, minimize or close the current screen.

* Replace miner with your miner's screen name .e.g miner1, miner2, etc.

```bash
# Return screen
screen -r miner

# Minimize screen
Press: CTRL + A + D

# Screens list
screen -ls

# Stop Node (when inside a screen)
Press: Ctrl + C

# Kill and Remove Node (when outside a screen)
screen -XS miner quit
```
