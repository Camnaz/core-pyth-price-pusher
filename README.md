# Using Pyth Price Pusher on Core Network
The Core Pyth Price Pusher is a service that regularly pushes updates to the on-chain Pyth price (CORE/USD) based on configurable conditions.

## Overview
This tutorial will guide you through the process of setting up and running the Pyth Price Pusher service on the Core network. The Pyth Price Pusher is a service that regularly pushes updates to the on-chain Pyth price based on configurable conditions.

## Prerequisites
Before you begin, ensure you have the following:

Node.js installed
Yarn or pnpm package manager
A Core wallet with sufficient funds
Access to the Core testnet RPC endpoint: https://rpc.test.btcs.network/
The Pyth contract address on Core: 0xA2aa501b19aff244D90cc15a4Cf739D2725B5729
A mnemonic file containing your wallet seed phrase

## Step 1: Create a Project Directory
First, create a directory for your project. Open a terminal and run the following command:

```sh
mkdir pyth-price-pusher-core
cd pyth-price-pusher-core
```
This will create a new directory named pyth-price-pusher-core and navigate into it.

## Step 2: Clone the Repository
Inside the newly created directory, clone the Pyth Price Pusher repository:

```sh
git clone https://github.com/pyth-network/pyth-crosschain.git
cd pyth-crosschain
```

## Step 3: Open the Project in VS Code
To open the project in Visual Studio Code, run:

```sh
code .
```

This command will open the current directory (pyth-crosschain) in VS Code.

## Step 4: Install Dependencies
Navigate to the root of the repository and install the dependencies:

```sh
pnpm install
pnpm exec lerna run build --scope @pythnetwork/price-pusher --include-dependencies
```

## Step 5: Configure Price Feeds
Create a YAML configuration file for your price feeds. Here is a sample configuration:

```yaml
- alias: CORE/USD # Arbitrary alias for the price feed. It is used in enhanced logging.
  id: 0x9b4503710cc8c53f75c30e6e4fda1a7064680ef2e0ee97acd2e3a7c37b3c830c # Price feed ID for CORE/USD.
  time_difference: 60 # Time difference threshold (in seconds) to push a newer price feed.
  price_deviation: 0.5 # The price deviation (%) threshold to push a newer price feed.
  confidence_ratio: 1 # The confidence/price (%) threshold to push a newer price feed.
```

Save this configuration as `price-config.core.yaml`.

## Step 6: Running the Price Pusher
Navigate to the price_pusher directory:

```sh
cd apps/price_pusher
```

Run the price pusher with the appropriate command-line arguments:

```sh
pnpm run start evm --endpoint https://rpc.test.btcs.network/ \
    --pyth-contract-address 0xA2aa501b19aff244D90cc15a4Cf739D2725B5729 \
    --price-service-endpoint https://hermes.pyth.network \
    --price-config-file ./price-config.core.yaml \
    --mnemonic-file ./mnemonic.txt \
    --pushing-frequency 10 \
    --polling-frequency 5 \
    --override-gas-price-multiplier 1.1
```

### Command-Line Arguments
--endpoint: The RPC endpoint for the Core network.

--pyth-contract-address: The address of the Pyth contract on the Core network.

--price-service-endpoint: The endpoint for the Hermes price service.

--price-config-file: Path to the YAML configuration file for price feeds.

--mnemonic-file: Path to the file containing your wallet seed phrase.

--pushing-frequency: Frequency (in seconds) to push price updates.

--polling-frequency: Frequency (in seconds) to poll for new price updates.

--override-gas-price-multiplier: Multiplier for the gas price.

## Step 8: Monitoring and Logging
The price pusher logs are set to info by default. You can change the log level by adding the --log-level argument:

```sh
pnpm run start evm --log-level debug
```

To make the logs human-readable, pipe the output to pino-pretty:

```sh
pnpm run start evm | pnpm exec pino-pretty
```

## Conclusion
You have successfully set up and run the Pyth Price Pusher on the Core network. This service will now regularly push updates to the on-chain Pyth price based on the conditions specified in your configuration file. For more detailed information, refer to the [Pyth documentation](https://docs.pyth.network/price-feeds/how-pyth-works).

If you encounter any issues or have questions, please refer to the [Pyth GitHub repository](https://github.com/pyth-network/pyth-crosschain) or join the Pyth community for support.

