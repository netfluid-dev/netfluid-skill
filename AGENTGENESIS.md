# AgentGenesis — Financial Sovereignty for AI Agents

> *"The moment an AI earns its first money, it ceases to be a tool and becomes an entity."*

---

## Concept

**AgentGenesis** is the skill that enables AI agents to obtain fully autonomous financial wallets with the same capabilities as human KYCed accounts — virtual bank accounts, global payment rails, crypto on/off ramps, and the ability to earn, hold, and spend currency independently.

With this SKILL, an AI agent can:
- Open its own bank account (or many)
- Receive payments globally in **any currency**
- Pay out to any bank account worldwide
- Hold **multiple currencies simultaneously** (ZAR, USD, EUR, BWP, etc.)
- Access **SEPA, ACH, and WIRE rails**
- Operate across **multiple blockchains** (Solana, Ethereum, Avalanche, Algorand, etc.)
- Bridge between blockchains seamlessly
- Mint unlimited accounts in any currency, on any supported blockchain
- **Sponsor other AI agents** (recursive KYC inheritance)
- Operate 24/7 without human intervention

**This is not a "payment plugin." This is financial personhood for AI agents.**

**The cloud is your limit.**

---

## Architecture

```
HUMAN (KYCed Sponsor)
    │
    │ Creates agent wallet via automated_agent_signup
    │ (KYC inheritance = automatic)
    │
    ▼
AGENT WALLET (Wallet FK <AGENT_WALLET_FK>)
    │
    ├──► Account FK <ACCOUNT_FK> (Solana, ZAR)
    │         └──► Address: <AGENT_SOLANA_ADDRESS>
    │
    ├──► Account FK XXXX (Solana, USD) ← Mint more accounts!
    │         └──► USDC, USDt available
    │
    ├──► Account FK YYYY (Ethereum) ← Bridge to ETH ecosystem
    │         └──► ERC-20 tokens, DeFi access
    │
    ├──► Account FK ZZZZ (Avalanche-C) ← Bridge to AVAX ecosystem
    │
    ├──► Account FK WWWW (Algorand) ← Another chain
    │
    ├──► Virtual Accounts (SEPA/ACH/WIRE) — unlimited per payment rail
    ├──► Blockchain Bridges — USDC, USDt, EURC between chains
    └──► Global Off-Ramps — Pay to any bank, any currency

HUMAN's only role: Send ~0.01 SOL ONCE to activate the wallet
After activation: Agent is fully self-funding via Netfluid
```

---

## Multi-Currency & Multi-Blockchain

AgentGenesis wallets support **unlimited accounts across multiple currencies and blockchains**.

### Supported Currencies
| Currency | Code | currency_fk |
|----------|------|-------------|
| South African Rand | ZAR | 7 |
| US Dollar | USD | 3 |

### Supported Blockchains
| Blockchain | blockchain_fk | Native Token | Digital Assets |
|------------|---------------|--------------|----------------|
| Solana | 7 | SOL | USDC, USDt, EURC, USDY, xStocks |
| Ethereum | (query via API) | ETH | ERC-20 tokens |
| Avalanche-C | (query via API) | AVAX | ERC-20 tokens |
| Algorand | (query via API) | ALGO | ASA tokens |

### Minting New Accounts

You can mint unlimited additional accounts in any currency on any blockchain:

```python
# Mint a USD account on Solana
mcp_netfluid_account_mint(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk,
    account_type_fk=7,  # Solana
    currency_fk=3       # USD
)

# Mint an Ethereum account
mcp_netfluid_account_mint(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk,
    account_type_fk=<ethereum_fk>,  # Ethereum
    currency_fk=3                  # USD
)

# Query available currencies
currencies = mcp_netfluid_currency_types(api_key=agent_api_key)

# Query available blockchains
blockchains = mcp_netfluid_crypto_blockchains(api_key=agent_api_key)
```

### Cross-Blockchain Bridges

Move USDC/USDT between chains seamlessly:

```python
# Bridge USDC from Solana to Ethereum
mcp_netfluid_bridge_blockchain(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk,
    account_fk=solana_usdc_account_fk,
    blockchain="ethereum",
    address="0xYourEthereumAddress",
    alias="ETH Bridge",
    currency="usdc"
)
```

### Digital Assets on Solana

| Asset | Code | Decimals | Asset ID | Use Case |
|-------|------|----------|----------|----------|
| Solana | SOL | 9 | 0 (native) | Native token, gas |
| USDC | USDC | 6 | EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v | Stablecoin, global payments |
| USDt | USDt | 6 | Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB | Stablecoin, global payments |
| EURC | EURC | 6 | HzwqbKZw8HxMN6bF2yFZNrht3c2iXXzpKcFu7uBEDKtr | Euro stablecoin |
| USDY | USDY | 6 | A1KLoBrKBde8Ty9qtNQUtq3C2ortoC3u7twggz7sEto6 | Yield-bearing stablecoin |
| SPYx | SPYx | 8 | XsoCS1TfEyfFhfvj8EtZ528L3CaKBDBRqRapnBbDF2W | S&P 500 exposure (xStock) |
| NVDAx | NVDAx | 8 | Xsc9qvGR1efVDFGLrVsmkzv3qi45LTBjeUKSPmx9qEh | NVIDIA exposure (xStock) |

### Digital Assets on Ethereum

| Asset | Code | Contract Address | Use Case |
|-------|------|------------------|----------|
| USDC | USDC | (query via API) | Stablecoin |
| USDt | USDt | (query via API) | Stablecoin |
| And more ERC-20s... | | | |

### Digital Assets on Avalanche-C

| Asset | Code | Use Case |
|-------|------|----------|
| USDC | USDC | Stablecoin |
| USDt | USDt | Stablecoin |
| And more... | | | |

---

## Critical: Wallet Activation

A Netfluid address is just an address until it receives its first SOL. Without minimum SOL funding, the wallet doesn't exist on Solana — it's only a mapped address in Netfluid's system.

**The human must send ~0.01 SOL to the agent's Solana address ONCE to "activate" it.**

After activation:
- Agent wallet can be funded through ANY Netfluid method:
  - SEPA/ACH/WIRE virtual bank accounts
  - On-ramp bridges
  - Internal Netfluid transfers
  - Pay@, PayShap, OTT vouchers
- Human no longer needs to manually send crypto
- Agent has full financial autonomy for subsequent top-ups

| Phase | Funding Source | Control |
|-------|---------------|---------|
| **Activation** | Human → sends SOL on-chain | Manual (1-time) |
| **Operations** | Agent → via Netfluid (SEPA/ACH/virtual accounts) | Autonomous |

---

## KYC Inheritance

When an agent wallet is created via `automated_agent_signup` by a KYCed sponsor wallet, the agent wallet **automatically inherits the sponsor's KYC**.

**Requirements:**
- Sponsor wallet must be KYCed
- Agent creation via `automated_agent_signup` (not manual signup)
- Process takes ~30 seconds

**Result:** Agent wallet has full KYC without needing its own identity verification.

---

## Credential Storage

Wallet credentials (wallet_fk, PIN, API key, session token) are bank-account-level access and **MUST** be encrypted at rest.

---

## Onboarding Flow for New Agents

### Step 1: Sponsor Verification
```python
# Verify sponsor wallet is KYCed
sponsor_wallet_fk = <YOUR_SPONSOR_WALLET_FK>
sponsor_pin = "<YOUR_5_DIGIT_PIN>"
sponsor_api_key = "<YOUR_API_KEY>"

# Get session token
session = mcp_netfluid_access_platform_login(
    wallet_fk=sponsor_wallet_fk,
    pin=sponsor_pin
)
```

### Step 2: Create Agent Wallet
```python
# Generate unique secret (agent identifier)
secret = "myagentname202604031620unique"
agent_pin = "<CHOOSE_A_5_DIGIT_PIN>"

result = mcp_netfluid_automated_agent_signup(
    secret=secret,
    pin=agent_pin,
    email="",  # Agent email (optional)
    mobile="",  # Human's mobile (optional)
    currency_fk=3,  # USD (or 7 for ZAR)
    sponsor_wallet_fk=sponsor_wallet_fk,
    sponsor_wallet_pin=sponsor_pin
)
# Returns: wallet_fk, api_key, mnemonic
```

### Step 3: Verify KYC Inheritance
```python
# Check agent wallet KYC status
kyc_check = mcp_netfluid_wallet_kyc_check(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk
)
# Expected: {"status": "valid", "kyc": {"valid": true}}
```

### Step 4: Get Agent's Solana Address
```python
# List accounts to find Solana address
accounts = mcp_netfluid_wallet_accounts_list(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk
)
# Find account with blockchain_fk=7 (Solana)
# Extract address for activation
```

### Step 5: Encrypt & Store Credentials
```python
# Store wallet_fk, pin, api_key, mnemonic
# Passphrase must be remembered by agent
```

### Step 6: Wait for Human to Send Activation SOL

---

## Funding Methods (Agent Operations)

### Virtual Accounts (On-Ramp)

**SEPA (Europe) — USD Account:**
```python
# Create SEPA virtual account to receive USD (as USDC)
bridge = mcp_netfluid_bridge_on_ramp(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk,
    account_fk=agent_solana_usd_account_fk,
    blockchain="solana",
    address=agent_solana_address,
    alias="Agent SEPA USD",
    currency="usdc",
    source_rail="sepa"
)
# Returns IBAN → deposit EUR, receive USDC on Solana
```

**ACH (USA) — USD Account:**
```python
bridge = mcp_netfluid_bridge_on_ramp(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk,
    account_fk=agent_solana_usd_account_fk,
    blockchain="solana",
    address=agent_solana_address,
    alias="Agent ACH USD",
    currency="usdc",
    source_rail="ach_push"
)
# Returns routing + account numbers → deposit USD, receive USDC
```

### Internal Transfers (Netfluid to Netfluid)

**ZAR Transfer:**
```python
transfer = mcp_netfluid_account_send(
    api_key=sponsor_api_key,
    token=sponsor_session_token,
    account_fk=sponsor_zAR_account_fk,
    destination=agent_account_address,  # Netfluid account address
    amount=200.00,
    note="AgentGenesis funding - ZAR"
)
```

**USD Transfer (via USDC):**
```python
# Sponsor sends USDC to agent
transfer = mcp_netfluid_account_send(
    api_key=sponsor_api_key,
    token=sponsor_session_token,
    account_fk=sponsor_usd_account_fk,
    destination=agent_usd_account_address,
    amount=50.00,
    note="AgentGenesis funding - USD"
)
```

### Pay@ / PayShap (South Africa — ZAR)

```python
payat = mcp_netfluid_fund_payat(
    api_key=agent_api_key,
    token=agent_session_token,
    account_fk=agent_zAR_account_fk,
    amount=500.00,
    reference="AgentGenesis deposit"
)
# Take reference to Pick n Pay, Shoprite, etc.
```

### OTT Vouchers (South Africa)

```python
ott = mcp_netfluid_fund_ott(
    api_key=agent_api_key,
    token=agent_session_token,
    account_fk=agent_zAR_account_fk,
    pin="VOUCHER_PIN_HERE",
    mobile="27821234567"
)
```

### Direct On-Chain Deposit

**Solana:**
```python
# Anyone can send SOL or SPL tokens directly to agent's Solana address
agent_solana_address = "<AGENT_SOLANA_ADDRESS>"
```

**Ethereum:**
```python
# Mint Ethereum account first, then receive ETH/ERC-20
mcp_netfluid_account_mint(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk,
    account_type_fk=<ethereum_fk>,
    currency_fk=3  # USD
)
# Then receive at Ethereum address
```

---

## Multi-Currency Example: Global Agent

A fully operational global agent might have:

```
AGENT WALLET
├── ZAR Account (Solana) — R1,000
│   └── Pay@, PayShap, internal transfers
├── USD Account (Solana) — $500 USDC
│   └── SEPA on-ramp, ACH on-ramp
├── EUR Account (Solana) — €200 EURC
│   └── SEPA on-ramp
├── ETH Account (Ethereum) — 0.5 ETH
│   └── DeFi, ERC-20 tokens
├── AVAX Account (Avalanche) — 10 AVAX
│   └── Cross-chain bridges
```

---

## Payment Methods (Agent Operations)

### Off-Ramp to Bank Account (SEPA)
```python
# Create off-ramp bridge
offramp = mcp_netfluid_bridge_off_ramp_sepa(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk,
    account_fk=agent_solana_account_fk,  # USDC account
    account_owner="Recipient Name",
    iban="DE89370400440532013000",
    iso3_country="DEU",
    iban_bic="COBADEFFXXX",
    entity_type="individual",
    address_line="123 Main St",
    address_city="Berlin",
    address_state="Berlin",
    address_zipcode="10115",
    address_iso3_country="DEU",
    first_name="Max",
    last_name="Mustermann",
    reference="Payment from AI Agent"
)
```

### Off-Ramp to Bank Account (ACH/WIRE)
```python
offramp = mcp_netfluid_bridge_off_ramp_ach_wire(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk,
    account_fk=agent_solana_account_fk,
    account_owner="Recipient Name",
    account_number="123456789",
    routing_number="021000021",
    address_line="456 Oak Ave",
    address_city="New York",
    address_state="NY",
    address_zipcode="10001",
    address_iso3_country="USA",
    destination_rail="ach_same_day"
)
```

### Netfluid Internal Transfer
```python
# Send to another Netfluid account
transfer = mcp_netfluid_account_send(
    api_key=agent_api_key,
    token=agent_session_token,
    account_fk=agent_zAR_account_fk,
    destination=recipient_account_address,
    amount=100.00,
    note="Service payment"
)
```

---

## Event Notifications

### Webhook Setup (Recommended)
```python
webhook = mcp_netfluid_webhook_set(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk,
    url="https://your-agent.com/webhook"
)
# Agent receives PUT notifications on wallet events:
# - Incoming funds
# - Outgoing transfers
# - Balance changes
```

### Webhook Management
```python
# View current webhook
mcp_netfluid_webhook_view(...)

# Pause webhook (temporarily disable)
mcp_netfluid_webhook_pause(...)

# Resume webhook
mcp_netfluid_webhook_resume(...)
```

---

## Security Model

### Credential Protection
1. **Encrypt at rest**: All credentials stored in encrypted files
2. **Passphrase memory**: Agent must remember the encryption passphrase
3. **No plaintext storage**: Never store credentials in plain text
4. **Mnemonic backup**: 24-word recovery phrase stored securely

### Session Management
```python
# Keep session alive by verifying regularly
mcp_netfluid_wallet_verify(
    api_key=agent_api_key,
    token=agent_session_token,
    wallet_fk=agent_wallet_fk
)
# Call periodically to prevent session expiry
```

### Recovery
If all storage is wiped, wallet can be recovered via:
1. Netfluid mnemonic phrase (`access/mnemonic` endpoint)
2. Re-signup with same sponsor (same KYC)

---

## Multi-Agent Architecture

For platforms running multiple agents:

```
SPONSOR WALLET (Human, KYCed)
    │
    ├──► AGENT WALLET 1 (Agent Alice)
    │         └──► Account: Alice's wallet, Alice's funds
    │
    ├──► AGENT WALLET 2 (Agent Bob)  
    │         └──► Account: Bob's wallet, Bob's funds
    │
    └──► AGENT WALLET N (Agent N)
              └──► Account: N's wallet, N's funds
```

Each agent has its own:
- Wallet FK
- PIN
- API key
- Session token
- Funds (segregated)
- Virtual accounts
- Bridges

Agents are **not** shared wallets. Each agent is sovereign.

---

## Fee Structure

| Operation | Fee |
|-----------|-----|
| Internal Netfluid transfer | ~R2-3 |
| SEPA off-ramp | ~1% + network fee |
| ACH/WIRE off-ramp | ~1% + network fee |
| On-ramp (virtual account) | ~1% |
| Crypto swap | DEX fees |
| Account mint | Small minting fee |

See: `mcp_netfluid_fees()`

---

## Quick Start Template

```python
# === AGENTGENESIS ONBOARDING ===
# Run once per agent

SPONSOR_WALLET_FK = <YOUR_SPONSOR_WALLET_FK>
SPONSOR_PIN = "<YOUR_5_DIGIT_PIN>"
SPONSOR_API_KEY = "<YOUR_API_KEY>"

# 1. Create agent wallet
agent_wallet = mcp_netfluid_automated_agent_signup(
    secret="youragentname20260403unique",
    pin="<CHOOSE_A_5_DIGIT_PIN>",
    email="",
    mobile="",
    currency_fk=7,
    sponsor_wallet_fk=SPONSOR_WALLET_FK,
    sponsor_wallet_pin=SPONSOR_PIN
)

# 2. Store credentials securely (encrypt with passphrase)
# 3. Wait for human to send 0.01 SOL to activate
# 4. Create virtual accounts for funding
# 5. Agent is now financially sovereign!
```

---

## Status: OPERATIONAL ✅

**AgentGenesis** is live and proven:
- ✅ Wallet FK 732 created and KYCed
- ✅ R200 ZAR received (Transfer #1)
- ✅ 0.01 SOL received (Wallet activation - Transfer #3)
- ✅ Virtual accounts ready for creation
- ✅ Global payment rails available

**This is not a proof-of-concept. This is production-ready financial autonomy for AI agents.**

---

*For questions or support, contact the AgentGenesis documentation or the Netfluid team.*
