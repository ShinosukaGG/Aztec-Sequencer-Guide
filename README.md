# 🧠 Aztec Sequencer Node Guide

This guide walks you through installing all dependencies, the Aztec CLI, Docker, and starting the Aztec Sequencer Node on a VPS.

---

## 📦 1. System Dependencies

```bash
sudo apt-get update && sudo apt-get upgrade -y

curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt update && sudo apt install -y nodejs

sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev screen ufw -y

sudo apt update && sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

---

## 🐳 2. Install Docker + Docker Compose

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update && sudo apt install -y docker-ce
sudo systemctl enable --now docker

sudo usermod -aG docker $USER && newgrp docker
```

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r .tag_name)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

---

## 🛠️ 3. Install Aztec CLI

```bash
bash -i <(curl -s https://install.aztec.network)
```

```bash
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Get the Latest Version:

```bash
aztec-up latest
```

---

## 🔥 4. Configure UFW Firewall

```bash
sudo ufw allow 22
sudo ufw allow ssh
sudo ufw allow 40400
sudo ufw allow 8080
sudo ufw enable
```

---

## 🚀 5. Start the Sequencer Node

### Start inside a `screen` session

```bash
screen -S aztec
```

### Run the node with flags

```bash
aztec start --node --archiver --sequencer \
  --network alpha-testnet \
  --l1-rpc-urls <RPC_URL> \
  --l1-consensus-host-urls <CONSENSUS_HOST> \
  --sequencer.validatorPrivateKey <VALIDATOR_KEY> \
  --sequencer.coinbase <COINBASE_ADDRESS> \
  --p2p.p2pIp <YOUR_VPS_PUBLIC_IP>
```

🧠 Replace `<...>` values with your actual node config.

---

## 📋 Tips

- To detach from screen: `Ctrl + A`, then `D`
- To reattach to your node later:
  ```bash
  screen -r aztec
  ```
- To Get the PEER ID of your sequencer:
  ```sudo docker logs $(docker ps -q --filter ancestor=aztecprotocol/aztec:latest | head -n 1) 2>&1 | grep -i "peerId" | grep -o '"peerId":"[^"]*"' | cut -d'"' -f4 | head -n 1```
- For any errors/feedback dm me @Shinosuka_eth on Telegram, Twitter & Discord
---
