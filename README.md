# Drosera-node-guide
This guide provides a comprehensive walkthrough for participating in the Drosera testnet. It covers the following key steps:

1. Installing the Drosera Command-Line Interface (CLI)
2. Setting up a vulnerable smart contract environment
3. Deploying a Trap on the testnet
4. Connecting an operator to the deployed Trap

# System Requirements (Recommended)
- 2 CPU Cores
- 4 GB RAM
- 20 GB Disk Space
- You can use Free Google Cloud VPS - https://cloud.google.com/products/compute
- Create your own `Ethereum Holesky RPC` in ([Ankr](https://www.ankr.com/rpc/)) **(Recommended)** or [QuickNode](https://dashboard.quicknode.com/) or [Alchemy](https://dashboard.alchemy.com/)

### Install Dependecies
```
sudo apt-get update && sudo apt-get upgrade -y

sudo apt install curl ufw iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y

apt install screen
```

### Install Docker
```bash
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test Docker
sudo docker run hello-world
```

<h1 align="center">Trap Setup</h1>

## 1. Configure Enviorments
**Drosera CLI**:
```bash
curl -L https://app.drosera.io/install | bash

source /root/.bashrc

droseraup
```

**Foundry CLI**:
```
curl -L https://foundry.paradigm.xyz | bash

source /root/.bashrc

foundryup
```

**Bun:**
```
curl -fsSL https://bun.sh/install | bash

source /root/.bashrc
```

---

## 2. Deploy Contract & Trap
```bash
mkdir my-drosera-trap

cd my-drosera-trap
```
**Replace `Github_Email` & `Github_Username`:**
```bash
git config --global user.email "Github_Email"

git config --global user.name "Github_Username"
```
- Replace `Github_Email` with your own `Github Email` and `Github_Username` with your own `Github Username`. You can create one at ([Github](https://github.com/))

**Initialize Trap**:
```
forge init -t drosera-network/trap-foundry-template
```
**Compile Trap**:
```bash
curl -fsSL https://bun.sh/install | bash

source /root/.bashrc

bun install

forge build
```
- skip warnings!

![Image](https://github.com/user-attachments/assets/6e0a395f-9668-4727-b789-77f5c4238be9)

**Deploy Trap**:
```bash
DROSERA_PRIVATE_KEY=xxx drosera apply --eth-rpc-url RPC
```
- Replace `xxx` with your EVM wallet `privatekey` (Ensure it's funded with `Holesky ETH`) and `RPC` with your own Ethereum Holesky rpc by registering and creating one in ([Ankr](https://www.ankr.com/rpc/)) **(Recommended)** or [QuickNode](https://dashboard.quicknode.com/) or [Alchemy](https://dashboard.alchemy.com/)

- Enter the command, when prompted, write `ofc` and press Enter.

- **Existing Users**
  - If you've deployed a trap with your wallet previously, then you need to add your trap address to `drosera.toml` file, by adding this line: `address = "TRAP_ADDRESS"` in the bottom.
  - Ensure you replace `TRAP_ADDRESS` with your own trap address.
  - Finally, as an existing user, Ensure you filled out `drosera.toml` bottom lines like this:
  ```
  whitelist = ["Operator1_Address","Operator2_Address"]
  address = "TRAP_ADDRESS"
  ```
  - After modifying your `drosera.toml`, re-apply your trap configuration by executing this command: `DROSERA_PRIVATE_KEY=xxx drosera apply --eth-rpc-url RPC` with your own Ethereum Holesky rpc

#

---

## 3. Check Trap in Dashboard
1. Connect your Drosera EVM wallet: https://app.drosera.io/

2. Click on `Traps Owned` to see  your deployed Traps OR search your Trap address.

![Image](https://github.com/user-attachments/assets/bd88c3d0-1744-4297-b09c-5f95eb4f13b1)

![Image](https://github.com/user-attachments/assets/3a081122-9a6a-4c2d-945d-40429ab4baf2)

## 4. Bloom Boost Trap
Open your Trap on Dashboard and Click on `Send Bloom Boost` and deposit some `Holesky ETH` on it.

![Image](https://github.com/user-attachments/assets/97b27a80-3779-4d6f-a421-d3ab9c8c077f)

![Image](https://github.com/user-attachments/assets/278ccbc2-49af-4ecb-8a68-5e7bbf5965f6)

![Image](https://github.com/user-attachments/assets/5c8b41e7-09eb-434e-b229-a449c922599b)

## 5. Fetch Blocks
```bash
drosera dryrun
```

---

<h1 align="center">Operator Setup</h1>

## 1. Whitelist Your Operator
**1- Edit Trap configuration:**
```bash
cd my-drosera-trap
nano drosera.toml
```
Add the following codes at the bottom of `drosera.toml`:
```toml
private_trap = true
whitelist = ["Operator_Address"]
```
* REPLACE `ethereum_rpc = https://ethereum-holesky-rpc.publicnode.com` with your own `Ethreum RPC`, CHANGE `block_sample_size = 10` to `block_sample_size = 5`, ADD `private_trap = true` AFTER `private = true` and REPLACE `Operator_Address` with your EVM wallet `Public Address` between " " symbol.
* Your `Public Address` is your `Operator_Address`.

**2- Update Trap Configuration:**
```bash
DROSERA_PRIVATE_KEY=xxx drosera apply --eth-rpc-url RPC
```
- Replace `xxx` with your EVM wallet `privatekey` (Ensure it's funded with `Holesky ETH`) and `RPC` with your own Ethereum Holesky rpc by registering and creating one in ([Ankr](https://www.ankr.com/rpc/)) **(Recommended)** or [QuickNode](https://dashboard.quicknode.com/) or [Alchemy](https://dashboard.alchemy.com/)

Your Trap should be private now with your operator address whitelisted internally.

![Image](https://github.com/user-attachments/assets/44b75fdf-6e71-468c-bbfd-0f7854df89b0)

---

## 2. Operator CLI
```bash
cd ~
```
```bash
# Download
curl -LO https://github.com/drosera-network/releases/releases/download/v1.17.2/drosera-operator-v1.17.2-x86_64-unknown-linux-gnu.tar.gz

# Install
tar -xvf drosera-operator-v1.17.2-x86_64-unknown-linux-gnu.tar.gz
```
* Currently the Operator CLI version is `v1.17.2`. Verify the latest version [here](https://github.com/drosera-network/releases/releases)
* You have to get the link of `drosera-operator-v1.x.x-x86_64-unknown-linux-gnu.tar.gz`

Test the CLI with `./drosera-operator --version` to verify it's working.
```console
# Check version
./drosera-operator --version

# Move path to run it globally
sudo cp drosera-operator /usr/bin

# Check if it is working
drosera-operator
```

## 3. Install Docker image
```
docker pull ghcr.io/drosera-network/drosera-operator:latest
```

---

## 4. Register Operator
```bash
drosera-operator register --eth-rpc-url RPC --eth-private-key PV_KEY
```
* Replace `RPC` with your own Ethereum Holesky rpc and `PV_KEY` with your Drosera EVM `privatekey`. Must use the same wallet which you have used for trap.

---

## 5. Open Ports
```bash
# Enable firewall
sudo ufw allow ssh
sudo ufw allow 22
sudo ufw enable

# Allow Drosera ports
sudo ufw allow 31313/tcp
sudo ufw allow 31314/tcp
```

---

**If you are using Google Cloud VPS must follow below guide**
* **Open TCP/UDP Ports 31313 and 31314 in Google Cloud Console**
1. Go to Firewall Rules Page
Link: https://console.cloud.google.com/networking/firewalls

2. Click "Create Firewall Rule"
Fill out the form:

- Name: allow-31313-31314-tcp-udp

- Network: default (or whichever VPC your VM is in)

- Priority: 1000 (default is fine)

- Direction of traffic: Ingress

- Action on match: Allow

- Targets: All instances in the network (or use target tags if you want more control)

- Source IP ranges: 0.0.0.0/0 (allows from anywhere — restrict for security if needed)

- Protocols and ports: Select “Specified protocols and ports”, then:

- Check TCP and enter: 31313,31314

- Check UDP and enter: 31313,31314

3. Click "Create"

---

## 6. Install & Run Operator
**Choose one Installation Method:**

## Method 1: Docker
### 6-1-1: Configure Docker
- Make sure you have installed `Docker` in Dependecies step.

If you are currently running via old `systemd` method, stop it:
```
sudo systemctl stop drosera
sudo systemctl disable drosera
```
```
git clone https://github.com/0xmoei/Drosera-Network

cd Drosera-Network

cp .env.example .env
```
Edit `.env` file:
```
nano .env
```
- Replace `your_evm_private_key` and `your_vps_public_ip`
- To save: `CTRL`+`X`, `Y` & `ENTER`.

Edit `docker-compose.yaml` file:
```bash
nano docker-compose.yaml
```
* Replace default `--eth-rpc-url` to your private Ethereum Holesky rpc by registering and creating one in ([Ankr](https://www.ankr.com/rpc/)) **(Recommended)** or [QuickNode](https://dashboard.quicknode.com/) or [Alchemy](https://dashboard.alchemy.com/)
* To save: `CTRL`+`X`, `Y` & `ENTER`.

### 6-1-2: Run Operator
```
docker compose up -d
```

### 6-1-3: Create Screen
```
screen -S drosera
```

### 6-1-4: Check health
```
docker logs -f drosera-node
```

![Image](https://github.com/user-attachments/assets/b0456503-1aaa-4d97-af66-ba33b3a050bb)

> No problem if you are receiveing `WARN drosera_services::network::service: Failed to gossip message: InsufficientPeers`

> Detatch the screen by `CTRL A + D`

### 6-1-4: Optional Docker commands
```console
# Stop node
cd Drosera-Network
docker compose down -v

# Restart node
cd Drosera-Network
docker compose up -d
```

```console
# Reattach Screen
screen -r drosera
```

**Now running your node using `Docker`, you can Jump to step 7.**

---

## Method 2: SystemD
### 6-2-1: Configure SystemD service file
Enter this command in the terminal, But first replace:
* `PV_KEY` with your `privatekey`
* `VPS_IP` with your solid vps IP (without anything else)
* Replace default `RPC` to your private ([Ankr](https://www.ankr.com/rpc/)) or [QuickNode](https://dashboard.quicknode.com/) or [Alchemy](https://dashboard.alchemy.com/)
```bash
sudo tee /etc/systemd/system/drosera.service > /dev/null <<EOF
[Unit]
Description=drosera node service
After=network-online.target

[Service]
User=$USER
Restart=always
RestartSec=15
LimitNOFILE=65535
ExecStart=$(which drosera-operator) node --db-file-path $HOME/.drosera.db --network-p2p-port 31313 --server-port 31314 \
    --eth-rpc-url RPC \
    --eth-backup-rpc-url https://1rpc.io/holesky \
    --drosera-address 0xea08f7d533C2b9A62F40D5326214f39a8E3A32F8 \
    --eth-private-key PV_KEY \
    --listen-address 0.0.0.0 \
    --network-external-p2p-address VPS_IP \
    --disable-dnr-confirmation true

[Install]
WantedBy=multi-user.target
EOF
```

### 6-2-2: Run Operator
```console
# reload systemd
sudo systemctl daemon-reload
sudo systemctl enable drosera

# start systemd
sudo systemctl start drosera
```

### 6-2-3: Create Screen
```
screen -S drosera
```

### 6-2-4: Check Node Health
```console
journalctl -u drosera.service -f
```

![Image](https://github.com/user-attachments/assets/b0456503-1aaa-4d97-af66-ba33b3a050bb)

> !! No problem if you are receiveing `WARN drosera_services::network::service: Failed to gossip message: InsufficientPeers`

> Detatch the screen by `CTRL A + D` 

### 6-2-4: Optional commands
```console
# Stop node
sudo systemctl stop drosera

# Restart node
sudo systemctl restart drosera
```

```console
# Reattach Screen
screen -r drosera
```

**Now running your node using `SystemD`, you can Jump to step 7.**
---

## 7. Opt-in Trap
In the dashboard., Click on `Opt in` to connect your operator to the Trap

![Image](https://github.com/user-attachments/assets/c6fa2d80-b00e-4ffb-a5f7-9f9ac5d2a2f8)

---

## 8. Check Node Liveness
Your node will start producing greeen blocks in the dashboard

![Image](https://github.com/user-attachments/assets/78843d3b-e55b-4c94-8749-37d9ed968bd7)

---
