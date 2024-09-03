# Allora

### 1. System Requirements
Ensure your system meets the following minimum requirements:
- **Operating System**: Linux or macOS (Windows via WSL or Docker)
- **RAM**: 16 GB
- **Storage**: 500 GB SSD
- **CPU**: 4 cores

### 2. Install Dependencies
Install the required software:
- **Docker**: Ensure Docker is installed and running.
- **Go**: Install Go, as it’s needed for compiling the Allora binaries.

### 3. Clone the Allora Repository
Clone the official Allora repository from GitHub and install the necessary binaries:

```bash
git clone https://github.com/allora-network/allora-chain.git
cd allora-chain
make install
```

Alternatively, you can install directly using the following command:

```bash
curl -sSL https://raw.githubusercontent.com/allora-network/allora-chain/main/install.sh | bash
```

Ensure the binary is in your `PATH`.

### 4. Initialize and Configure the Node
Initialize your node and generate the necessary files:

```bash
allorad init <your_node_name> --chain-id=allora-testnet
```

Next, configure the `config.toml` and `app.toml` files in the `$HOME/.allorad/config/` directory to connect to the testnet.

### 5. Start the Node
To start the node, use Docker:

```bash
docker compose up -d
```

Check if the node is syncing by running:

```bash
curl -s http://localhost:26657/status | jq .result.sync_info.catching_up
```
The output should be `false` once your node is fully synced.

### 6. Fund Your Account
After the node is initialized, generate your key and account:

```bash
allorad keys add <your_key_name> --keyring-backend=test
```

For testnet, use the faucet to fund your account with the generated address.

### 7. Set Up and Stake as a Validator
Once your node is fully synced and funded:

1. Access the validator's shell:
   ```bash
   docker compose exec validator0 bash
   ```
2. Prepare the staking transaction:
   ```bash
   allorad tx staking create-validator \
   --amount=1000000uallo \
   --pubkey=$(allorad tendermint show-validator) \
   --moniker=<your_node_name> \
   --chain-id=allora-testnet \
   --commission-rate="0.10" \
   --commission-max-rate="0.20" \
   --commission-max-change-rate="0.01" \
   --min-self-delegation="1" \
   --from=<your_key_name> \
   --keyring-backend=test
   ```
3. Verify your validator setup:
   ```bash
   allorad q staking validators --chain-id=allora-testnet -o json | jq
   ```

### 8. Monitor Your Validator
Check the logs and status regularly to ensure your validator is performing correctly:
```bash
docker compose logs -f
```

### 9. Upgrade and Maintenance
Keep your node software updated by pulling the latest Docker images or recompiling the binaries from the repository as needed.
