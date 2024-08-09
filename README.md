
# Chasm Inference Scout Setup Guide

## Entering Chasm

### Introduction to Chasm

Chasm is at the forefront of artificial intelligence innovation, providing a robust ecosystem for creating, deploying, and monetizing AI agents. Central to this ecosystem is **Weave**, an AI agent builder IDE that allows users to craft and deploy AI agents using decentralized compute resources. Complementing Weave is the **Chasm Protocol**, a decentralized network of AI experts designed to evolve into a pseudo-AGI intent network. This protocol is key to managing payments for compute resources and enabling sophisticated exchanges of intents among AI agents, significantly enhancing operational dynamics and connectivity in the AI landscape.

This guide provides detailed instructions on setting up and running the **Chasm Inference Scout**, a tool that executes inference tasks assigned by Chasm's Orchestrator. By running the Inference Scout on your server, you can leverage powerful language models and contribute to the Chasm Network.

## Prerequisites

### API Keys

Before setting up Inference Scout, you will need to obtain the following API keys:

- **Groq API Key**: Sign up and retrieve your API key from the [Groq Console](https://console.groq.com/keys).
- **(Optional) Openrouter API Key**: Obtain this key from [Openrouter](https://openrouter.ai/settings/keys).
- **OpenAI API Key**: Obtain this key from [OpenAI](https://platform.openai.com/api-keys).
- **SCOUT_UID and WEBHOOK_API_KEY**: These keys can be obtained after minting the NFT on the Chasm website by following the instructions under "Obtaining your SCOUT_UID and WEBHOOK_API_KEY."

### Server Specifications

#### Minimum Requirements:
- 1 vCPU
- 1GB RAM
- 20GB Disk
- Static IP

#### Suggested Requirements:
- 2 vCPU
- 4GB RAM
- 50GB SSD
- Static IP

## Obtaining your SCOUT_UID and WEBHOOK_API_KEY

1. Navigate to [Chasm Scout Private Mint](https://scout.chasm.net/private-mint).
   
   ![image](https://github.com/user-attachments/assets/8ed2db5d-34a8-4f22-a239-dc97da0957f4)

3. Click on `_mint(scout)` and log in to the website.
4. Mint the NFT, and retrieve your **webhook API key** and **UID**.

   ![image 2](https://github.com/user-attachments/assets/b334a8c1-8194-4419-9bca-e41af6c2f137)


> **Note:** To mint the NFT, you will need a small amount of MNT on the Mantle Network. You can bridge GAS using [Jumper](https://jumper.exchange/gas/) or another bridge, or send MNT directly via a CEX.

## Software Requirements

### Install Docker

Docker is required to run the Inference Scout. Follow the steps below to install Docker on your server.

#### Set up Docker's apt repository:

1. **Add Docker's official GPG key:**
   ```bash
   sudo apt-get update
   sudo apt-get install ca-certificates curl
   sudo install -m 0755 -d /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   ```

2. **Add the Docker repository to Apt sources:**
   ```bash
   echo      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" |      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   sudo apt-get update
   ```

## Setup Guide

### 1. Get Required Credentials from Chasm



Ensure you have your SCOUT_UID, WEBHOOK_API_KEY, and any other necessary API keys before proceeding.

### 2. Prepare the `.env` File

Create a new folder and prepare your environment file:

```bash
mkdir chasm-scout
cd chasm-scout
nano .env
```

### Configuring the `.env` File

Below are the sections in the `.env` file that you can or should customize based on your server setup and API credentials:

#### `PORT=3001`
- **Description:** This field sets the port on which your Chasm Inference Scout server will listen for incoming connections. The default value is `3001`.
- **Action:** Change this if you prefer or need to use a different port on your server due to conflicts with other services. Ensure that the port you choose is open and not blocked by a firewall.

#### `SCOUT_NAME=`
- **Description:** This is the name of your scout instance, which is used for identification within the Chasm Network.
- **Action:** Set this to a unique and recognizable name for your scout, especially if you manage multiple instances. For example, `SCOUT_NAME=my-scout-instance`.

#### `SCOUT_UID=`
- **Description:** This is your unique Scout ID, which you obtain after minting your NFT on the Chasm website.
- **Action:** You must enter the `SCOUT_UID` that you received from the [Chasm Scout Private Mint](https://scout.chasm.net/private-mint).

#### `WEBHOOK_API_KEY=`
- **Description:** This API key authorizes your scout to interact with the Chasm Orchestrator.
- **Action:** Paste the `WEBHOOK_API_KEY` you obtained along with your `SCOUT_UID` from the Chasm website.

#### `WEBHOOK_URL=`
- **Description:** This URL is where your server will receive webhook requests from the Chasm Orchestrator. It includes your server’s IP address and the port you've specified.
- **Action:** Replace `123.123.123.123` with your server’s actual IP address, and ensure the port matches the one you've set under `PORT`.
  - **Example:** `WEBHOOK_URL=http://your.server.ip:3001/`

#### `PROVIDERS=groq`
- **Description:** This specifies the inference provider your scout will use. The default is `groq`.
- **Action:** If you're using a different provider like OpenAI, change `groq` to `openai`.

#### `MODEL=gemma2-9b-it`
- **Description:** This field specifies the model your scout will use for inference tasks.
- **Action:** You can leave this as `gemma2-9b-it` or change it to a different model name if you have specific instructions or preferences.

#### `GROQ_API_KEY=`
- **Description:** This is the API key required to access Groq’s services.
- **Action:** Enter your Groq API key, which you can obtain from the [Groq Console](https://console.groq.com/keys).

#### `OPENROUTER_API_KEY=` (Optional)
- **Description:** If you're using Openrouter, this is where you input the API key.
- **Action:** Leave blank if not using Openrouter. Otherwise, paste your Openrouter API key, obtainable from [Openrouter](https://openrouter.ai/settings/keys).

#### `OPENAI_API_KEY=` (Optional)
- **Description:** If you're using OpenAI as a provider, this is where you input the API key.
- **Action:** Leave blank if not using OpenAI. Otherwise, paste your OpenAI API key, which you can get from [OpenAI](https://platform.openai.com/api-keys).


Add the following content to the `.env` file:

```env
PORT=3001
LOGGER_LEVEL=debug

# Chasm
ORCHESTRATOR_URL=https://orchestrator.chasm.net
SCOUT_NAME=
SCOUT_UID=
WEBHOOK_API_KEY=
# Scout Webhook Url, update based on your server's IP and Port
# e.g. http://123.123.123.123:3001/
WEBHOOK_URL=

# Chosen Provider (groq, openai)
PROVIDERS=groq
MODEL=gemma2-9b-it
GROQ_API_KEY=

# Optional
OPENROUTER_API_KEY=
OPENAI_API_KEY=
```

> **Important:** Do not use single quotes (`'`) or double quotes (`"`) in your `.env` file, as Docker may have issues processing them.

### 3. Pull the Docker Image

Download the latest Chasm Inference Scout Docker image:

```bash
docker pull chasmtech/chasm-scout:latest
```

### 4. Run the Docker Container

Start the Inference Scout Docker container:

```bash
docker run -d --restart=always --env-file ./.env -p 3001:3001 --name scout chasmtech/chasm-scout
```

### 5. Verify Server Status

Check the logs to verify that the server is running correctly:

```bash
docker logs scout
```

### 6. Test Server Response

Ensure that the server is responding correctly:

```bash
curl localhost:3001
```

Expected response: `OK`

### 7. Test LLM Functionality

Test the language model functionality by sending a request:

```bash
source ./.env
curl -X POST      -H "Content-Type: application/json"      -H "Authorization: Bearer $WEBHOOK_API_KEY"      -d '{"body":"{"model":"gemma2-9b-it","messages":[{"role":"system","content":"You are a helpful assistant."}]}"}'      $WEBHOOK_URL
```

If you encounter any issues with server access, ensure the necessary ports are open:

```bash
sudo ufw allow 3001
sudo ufw allow 3001/tcp
```

### 8. Restart Docker Container (if needed)

If you need to restart the container for any reason:

```bash
docker stop scout
docker rm scout
docker run -d --restart=always --env-file ./.env -p 3001:3001 --name scout chasmtech/chasm-scout
```

### 9. Verify Scout Ranking

Check your scout's ranking on the [Leaderboard](https://chasm.net/leaderboard). Note that it may take up to an hour for your node's status to update.

![image 3](https://github.com/user-attachments/assets/f1edade0-b5af-418e-80cf-43c348cabeb9)

### 10. Monitor Scout Performance

Monitor the performance of your scout using Docker stats:

```bash
docker stats scout
```

## Troubleshooting

### Environment Variables

Double-check that all required environment variables are correctly set in the `.env` file. Incorrect or missing variables can cause the server to fail to start or function incorrectly.

## Additional Resources

- **Optimization Guide**: [Follow the optimization guide](https://chasm.net/optimization) for performance improvements.
- **Update Guide**: [Follow the update guide](https://chasm.net/update) for updating your scout.

This guide is designed to help you set up and run the Chasm Scout server effectively. For more detailed instructions and troubleshooting, refer to the original repository and additional resources provided.
