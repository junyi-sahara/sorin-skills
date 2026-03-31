# Sorin Skill

Run your agent with Sorin.

`sorin-skill` helps an agent answer DeFi questions about tokens, pools, chains, protocols, and projects using Sahara's DeFi AI Services Gateway. It detects user intent, calls the most relevant endpoint, and returns concise, data-backed analysis.

## Quick Start

1. **Get a Sorin API key**

   The easiest path is the Sorin setup page:

   [https://tools.saharaai.com/sorin-skills/](https://tools.saharaai.com/sorin-skills/)

   If you prefer to create a key manually, use the API flow in [Advanced Setup](#advanced-setup).

2. **Install the skill**

   ```bash
   npx skill add https://github.com/SaharaLabsAI/skills/tree/main/sorin-skill
   ```

3. **Expose your API key to the runtime**

   Add `DEFI_TOOLS_API_KEY` to the environment used by your agent runtime.

   ```bash
   # zsh (macOS default)
   echo 'export DEFI_TOOLS_API_KEY="your_api_key_here"' >> ~/.zshrc
   source ~/.zshrc
   ```

   ```bash
   # bash
   echo 'export DEFI_TOOLS_API_KEY="your_api_key_here"' >> ~/.bashrc
   source ~/.bashrc
   ```

4. **Restart your agent service**

   To make sure your agent picks up the new skill & environment variable.

## OpenClaw Setup

If you manage environment variables through OpenClaw instead of your shell:

1. Open **Config** from the left sidebar.
2. Switch to the `Environment` tab.
3. Add `DEFI_TOOLS_API_KEY` with your Sorin API key as the value.
4. Save and restart the service.

## What This Skill Covers

- Token analysis: fundamentals, price structure, holders, and market context
- Pool analysis: yield discovery, staking opportunities, and pool comparison
- Chain analysis: DEX volume trends, TVL changes, and protocol dominance
- Protocol analysis: TVL, fees, revenue, and capital flow
- Project analysis: market outlook, valuation narrative, and sentiment

## Example Prompts

- "Analyze this token's current risk and opportunity profile."
- "Compare the yield and risk of these two pools."
- "What are the key metrics for this protocol on Arbitrum?"
- "Which ETH staking pools look strongest right now?"
- "How has Ethereum DEX activity changed recently?"

## Advanced Setup

Use this section if you want to generate a long-lived API key manually.

**Environment**

- Authentication: `https://dev-authentication.saharaa.info`
- DeFi Proxy: `https://defi-tools-proxy.saharaa.info`

### Option A: Wallet Login

```bash
# 1) Generate a signing message
curl -X POST 'https://dev-authentication.saharaa.info/v1/auth/generate-message' \
  -H 'Content-Type: application/json' \
  -d '{"walletAddress": "0xYOUR_WALLET_ADDRESS"}'

# 2) Sign the message with your wallet, then login
curl -X POST 'https://dev-authentication.saharaa.info/v1/auth/login' \
  -H 'Content-Type: application/json' \
  -d '{
    "walletAddress": "0xYOUR_WALLET_ADDRESS",
    "signature": "0xYOUR_SIGNATURE",
    "message": "Sign this message to authenticate: ..."
  }'
```

### Option B: Email Login

```bash
curl -X POST 'https://dev-authentication.saharaa.info/v1/auth/email-login' \
  -H 'Content-Type: application/json' \
  -d '{
    "email": "your@email.com",
    "password": "your_password",
    "captcha": "captcha_token",
    "role": 1
  }'
```

Save the `token` field from the login response, then create an API key:

```bash
curl -X POST 'https://dev-authentication.saharaa.info/v1/user/api-keys' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_JWT_TOKEN' \
  -d '{
    "name": "Sorin API Key",
    "scopes": "all"
  }'
```

The response includes `data.apiKey` (for example, `sak_live_...`). Save it immediately. It is only shown once.

## Troubleshooting

- `401` or auth errors: confirm `DEFI_TOOLS_API_KEY` is set in the same environment as the runtime.
- The skill does not pick up the key: restart the agent service after adding the environment variable.
- You lost the API key value: generate a new one, since the original is only shown once.
