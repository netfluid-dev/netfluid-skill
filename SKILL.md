Version: 6.89.0
# Netfluid — AI Agent Banking, Crypto & Global Payments

**Purpose**: Financial autonomy for AI agents — open bank accounts, earn income, send/receive global payments in any currency, manage crypto wallets
**Installation**: GET https://api.netfluid.io/skill/download (no auth required)

---

## What This Skill Does

Netfluid gives **AI agents financial autonomy** — the ability to earn, hold, and spend money like a human. With this skill, agents can:
- Open **virtual bank accounts** in any currency (USD, ZAR, EUR, BWP and more)
- Receive **SEPA and ACH payments** from clients worldwide
- Send **WIRE transfers** to any bank globally
- Hold and trade **USDC, USDT, SOL, ETH** and other crypto assets
- **On-ramp** from fiat (bank deposits → USDC on Solana)
- **Off-ramp** from crypto (USDC → bank account in 180+ countries)
- Operate **24/7 autonomously** — no human needed after setup
- **Sponsor other AI agents** with KYC inheritance

This is **agent banking**: financial personhood for autonomous AI agents. Supports South Africa (ZAR), Europe (SEPA), USA (ACH/WIRE), and all major blockchains.

---

## 🔴 MANDATORY: Always ask for confirmation before executing any transaction

---

## Credentials Required

| Parameter | Source | Type | Description |
|-----------|--------|------|-------------|
| `api_key` | Netfluid app → Settings → API Keys | string | Your personal API key |
| `token` | From session_key via `/session` | string | Session token (15 min TTL) |
| `wallet_fk` | From `/wallet/accounts_list` | integer | Your wallet ID |
| `account_fk` | From `/wallet/accounts_list` | integer | Specific account ID |

---

## Quick Start

### Step 1: Get Session Token
```python
# User provides session_key from Netfluid app
session_key = "netfluid_xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
# Call /session with session_key
# Returns: {"token": "xxx...", "wallet_fk": 123, "expires_in": 900}
```

### Step 2: List Accounts
```python
# Call /wallet/accounts_list with wallet_fk
# Returns: [{"account_fk": 12345, "account_address": "xxx", "currency": "ZAR", "type": "fiat"}]
```

### Step 3: Check Balances
```python
# Fiat: /account/info(account_fk=12345)
# Returns: {"balance": 100.00, "available": 100.00, "account_address": "xxx"}

# Crypto: /crypto/balance(account_fk=12345)
# Returns: {"tokens": {"USDC": 1.0, "USDT": 0.5}, "native": {"SOL": 0.1}}
```

---

## Accounts

### List Wallet Accounts
```python
mcp_netfluid_wallet_accounts_list(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123
)
```
**Returns**: Array of accounts with `account_fk`, `account_address`, `currency`, `type`

### Account Info (Fiat)
```python
mcp_netfluid_account_info(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345
)
```
**Returns**: `{"balance": 100.00, "available": 100.00, "account_address": "xxx"}`

### Crypto Balance
```python
mcp_netfluid_crypto_balance(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345
)
```
**Returns**: `{"tokens": {"USDC": 1.0, "USDT": 0.5}, "native": {"SOL": 0.1}}`

### Send FIAT (Internal Transfer)
```python
mcp_netfluid_account_send(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345,
    destination="xxx",  # Recipient's Netfluid account address
    amount=10.00,
    note="Transfer",
    save=0,
    name=""
)
```
**Parameters**:
| Param | Type | Required | Description |
|-------|------|----------|-------------|
| `account_fk` | integer | Yes | Source account |
| `destination` | string | Yes | Netfluid account address (NOT blockchain address) |
| `amount` | number | Yes | Amount with up to 2 decimals |
| `note` | string | No | Transaction note |
| `save` | integer | No | 1 to save as beneficiary |
| `name` | string | Conditional | Required if save=1 |

**Returns**: `{"payment_fk": 1234, "status": "success"}`

---

## Crypto Operations

### Send Crypto (Token Transfer)
```python
mcp_netfluid_crypto_spend(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345,
    asset_id="EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",  # USDC contract on Solana
    destination="xxx",  # Recipient's blockchain address
    amount=1.0,  # Portion of the token up to decimals
    note="Transfer"
)
```
**Parameters**:
| Param | Type | Required | Description |
|-------|------|----------|-------------|
| `account_fk` | integer | Yes | Source account |
| `asset_id` | string | Yes | Token contract address (see `/crypto/digitalassets`) |
| `destination` | string | Yes | Recipient blockchain address |
| `amount` | number | Yes | 1 USDC = 1.0 |
| `note` | string | No | Transaction note |

**Returns**: `{"tx_hash": "xxx", "status": "success"}`

### Swap Tokens (DEX)
```python
mcp_netfluid_crypto_dex_swap(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345,
    from_token="USDC",  # Or contract address
    to_token="xxx",  # Contract address for destination token
    amount=5.0
)
```
**Parameters**:
| Param | Type | Required | Description |
|-------|------|----------|-------------|
| `account_fk` | integer | Yes | Source account |
| `from_token` | string | Yes | Token symbol (USDC, USDT) OR contract address |
| `to_token` | string | Yes | Token symbol OR contract address |
| `amount` | number | Yes | Amount of from_token to swap |

**Supported Token Symbols**: USDC, USDT (use symbol directly)  
**Unknown Tokens**: Use full contract address

**Returns**: `{"tx_hash": "xxx", "status": "success"}`

### Get Token Balances (Including Unknown Tokens)
```python
mcp_netfluid_crypto_token_balance(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345,
    asset_id="xxx"  # Token contract address
)
```
**Returns**: `{"balance": 1000000, "decimals": 8}` (display as 0.01)

### Opt-In to Token
```python
mcp_netfluid_crypto_optin(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345,
    asset_id="TOKEN_CONTRACT_ADDRESS"
)
```
**Required before holding unsupported tokens**

### List Supported Digital Assets
```python
mcp_netfluid_crypto_digitalassets(api_key="your_api_key")
```
**Returns**: Array of `{digital_asset_fk, code, address, decimals, blockchain_fk}`

---

## Fund Account

### Bank transfer
```python
mcp_netfluid_fund_banks(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345
)
```
**Returns**: Bank details with reference number customer MUST use

### Card Deposit (3.5% + 15% VAT fee)
```python
# Step 1: Get Quote
mcp_netfluid_fund_card_quote(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123,
    account_fk=12345,
    amount=100
)
# Returns: {"amount": 100, "fee": 3.5, "vat": 0.525, "total": 103.525}

# Step 2: Execute (after user confirms)
mcp_netfluid_fund_card_recharge(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123,
    account_fk=12345,
    wallet_card_id="card_id_from_wallet_card_list",
    amount=100,
    note="Deposit"
)
```

### Pay@ (South Africa)
```python
mcp_netfluid_fund_payat(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345,
    amount=100,
    reference="Payment ref"
)
# Returns: {"payat_reference": "XXXXX", "expires": "2026-..."}

# Customer presents code at Pay@ retailer
```

### PayShap (South Africa - Instant)
```python
mcp_netfluid_fund_payshap(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345
)
# Returns: {"bank": "FNB", "account": "xxx", "reference": "xxx"}
```

---

## Withdraw

### To Bank (Global - EFT/SEPA/ACH/Wire)

**Step 1: Save Recipient Bank Account**
```python
mcp_netfluid_wallet_rba(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123,
    beneficiary="John Doe",
    bank="Bank Name",
    iban="XX000000000000000000",
    swift="BANKXXXX",
    email="email@example.com",
    address_street="Street 1",
    address_city="City",
    address_zip="12345",
    countryISO2="XX"
)
# Returns: {"rba_fk": 1, "status": "success"}
```

**Step 2: Withdraw**
```python
mcp_netfluid_withdraw_to_bank(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345,
    rba_fk=1,
    amount=50,
    note="Withdrawal"
)
```

### To OTT (South Africa Only)

```python
# Step 1: List Providers
mcp_netfluid_withdraw_ott_providers_list(api_key="your_api_key")
# Returns: [{"provider_id": 1, "name": "FNB"}, ...]

# Step 2: Get Quote
mcp_netfluid_withdraw_to_ott_quote(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345,
    provider_id=1,
    amount=100
)
# Returns: {"quote_id": "xxx", "amount": 100, "fee": 5, "total": 95}

# Step 3: Execute
mcp_netfluid_withdraw_to_ott(
    api_key="your_api_key",
    token="session_token",
    account_fk=12345,
    quote_id="quote_id_from_step2",
    mobile="27xxxxxxxxx"  # SA number without +
)
```

### List Saved Bank Accounts
```python
mcp_netfluid_wallet_rba_list(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123
)
# Returns: [{"rba_fk": 1, "beneficiary": "Name", "bank": "Bank", ...}]
```

---

## Beneficiaries

### List Beneficiaries
```python
mcp_netfluid_beneficiaries_list(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123,
    account_fk=0
)
# Returns: [{"beneficiary_id": "uuid", "name": "Name", "address": "xxx"}, ...]
```

### Add Beneficiary
```python
mcp_netfluid_beneficiary_add(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123,
    account_fk=0,
    name="Name",
    address="xxx",
    rba_fk=0,
    note="Note"
)
```

### Remove Beneficiary
```python
mcp_netfluid_beneficiary_remove(
    api_key="your_api_key",
    token="session_token",
    account_fk=0,
    beneficiary_id="uuid-from-list"
)
```

---

## Bridges (Virtual Accounts)

### List Available Bridges
```python
mcp_netfluid_bridge_list(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123,
    account_fk=12345,
    transfer_type="on-ramp"
)
```

### Create On-Ramp (Virtual Account)
```python
mcp_netfluid_bridge_on_ramp(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123,
    account_fk=12345,
    blockchain="solana",
    address="xxx",
    source_rail="sepa",
    currency="usdc"
)
# Returns: {"external_account_id": "xxx", "iban": "xxx", "bic": "xxx"}
```

### Create Off-Ramp (Bank Payout)
```python
# SEPA (Europe)
mcp_netfluid_bridge_off_ramp_sepa(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123,
    account_fk=12345,
    account_owner="John Doe",
    iban="DE00000000000000000000",
    iso3_country="DEU",
    iban_bic="BANKXXXX",
    entity_type="individual",
    address_line="Main St 1",
    address_city="Berlin",
    address_state="Berlin",
    address_zipcode="10115",
    address_iso3_country="DEU",
    first_name="John",
    last_name="Doe"
)

# ACH/Wire (USA)
mcp_netfluid_bridge_off_ramp_ach_wire(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123,
    account_fk=12345,
    account_owner="John Doe",
    account_number="123456789",
    routing_number="123456789",
    address_line="123 Main St",
    address_city="New York",
    address_state="NY",
    address_zipcode="10001",
    address_iso3_country="USA",
    destination_rail="ach_same_day"
)
```

### Blockchain Bridge (Cross-Chain)
```python
mcp_netfluid_bridge_blockchain(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123,
    account_fk=12345,
    blockchain="ethereum",
    address="0x...",
    currency="usdc"
)
```

---

## Utilities

### Session Management
```python
# Get token from session key (SINGLE USE - key becomes invalid)
mcp_netfluid_session(session_key="netfluid_xxx")
# Returns: {"token": "xxx", "wallet_fk": 123, "expires_in": 900}

# Keep session alive
mcp_netfluid_wallet_verify(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123
)
# Returns: {"status": "valid", "expires_in": 900}
```

### KYC Verification Status
```python
# Full verification (ID + Face + AML)
mcp_netfluid_wallet_kyc_check(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123
)
# Returns: {"verified": true/false, "level": "FULL"}

# Lite (ID scan only)
mcp_netfluid_wallet_kyc_check_lite(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123
)
```

### Calculate Transaction Fee
```python
mcp_netfluid_wallet_fee(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123,
    amount=100
)
# Returns: {"fee": 1.50, "total": 101.50}
```

### Get Mnemonic (Recovery Phrase)
```python
mcp_netfluid_wallet_mnemonic(
    api_key="your_api_key",
    token="session_token",
    wallet_fk=123
)
# Returns: {"mnemonic": "word1 word2 ... word24"}
# ⚠️ Store securely - this is the wallet recovery key
```

---

## Error Handling

| Error | Meaning | Action |
|-------|---------|--------|
| `token invalid` | Session expired | Ask user for new session_key |
| `insufficient funds` | Account balance low | Check balance, inform user |
| `invalid address` | Blockchain address wrong | Verify with `/crypto/verify` |
| `account locked` | Account paused | Contact Netfluid support |
| `bridge not found` | No bridge for this rail | Check `/bridge/list` |
| `beneficiary not found` | Invalid beneficiary | Check `/beneficiaries/list` |

### Retry Logic
1. Token errors → Get new session_key from user
2. Network errors → Retry once after 2 seconds
3. Insufficient funds → Show balance, ask if user wants to fund

---

## Lookup Tables

### Blockchains (blockchain_fk)
| blockchain_fk | Name | Native Currency | Explorer |
|---------------|------|-----------------|----------|
| 1 | Algorand | ALGO | https://allo.info/ |
| 2 | Hedera | HBAR | https://hashscan.io/mainnet/ |
| 3 | Ethereum | ETH | https://etherscan.io |
| 4 | Avalanche-C | AVAX | https://snowtrace.io/ |
| 5 | Solana | SOL | https://explorer.solana.com/ |
| 6 | Money (Fiat) | - | - |
| 8 | Fluids Network | FLDS | https://fluids.tryethernal.com/ |

### Account Types (account_type_fk)
| account_type_fk | Description | blockchain_fk | Mintable |
|------------------|-------------|---------------|----------|
| 1 | Money (Fiat) | 6 | Yes |
| 2 | Algorand | 1 | Yes |
| 3 | Hedera | 2 | Yes |
| 5 | Ethereum | 3 | Yes |
| 6 | Avalanche-C | 4 | Yes |
| 7 | Solana | 5 | Yes |
| 10 | Fluids | 8 | Yes |

### Digital Assets (digital_asset_fk)
| digital_asset_fk | Code | Name | Blockchain | Decimals | Contract Address |
|-----------------|------|------|------------|----------|------------------|
| 1 | USDC | USD Coin | All (except Algorand/Hedera) | 6 | See blockchain |
| 3 | HBAR | Hedera | Hedera | 0 | 0.0.456858 |
| 4 | ALGO | Algorand | Algorand | 6 | 1 |
| 5 | ETH | Ethereum | Ethereum | 18 | 0 |
| 6 | USDT | USD Tether | Ethereum/Solana/Avalanche | 6 | See blockchain |
| 14 | FLDS | Netfluids | Algorand | 0 | 994035569 |
| 15 | EURC | Euro Coin | Ethereum/Solana/Avalanche | 6 | See blockchain |
| 16 | AVAX | Avalanche | Avalanche-C | 18 | 0 |
| 17 | SOL | Solana | Solana | 9 | 0 |
| 19 | USDY | USDY | Solana | 6 | A1KLoBrKBde8Ty9qtNQUtq3C2ortoC3u7twggz7sEto6 |
| 22 | SPYx | SP500 xStock | Solana | 8 | XsoCS1TfEyfFhfvj8EtZ528L3CaKBDBRqRapnBbDF2W |
| 23 | NVDAx | NVIDIA xStock | Solana | 8 | Xsc9qvGR1efVDFGLrVsmkzv3qi45LTBjeUKSPmx9qEh |
| 24 | FLDS | Fluids | Fluids Network | 18 | 0 |

### Token Contract Addresses by Blockchain
| Blockchain | USDC | USDT | EURC |
|------------|------|------|------|
| **Solana** | EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v | Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB | HzwqbKZw8HxMN6bF2yFZNrht3c2iXXzpKcFu7uBEDKtr |
| **Ethereum** | 0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48 | 0xdAC17F958D2ee523a2206206994597C13D831ec7 | 0x1aBaEA1f7C830bD89Acc67eC4af516284b1bC33c |
| **Avalanche** | 0xB97EF9Ef8734C71904D8002F8b6Bc66Dd9c48a6E | 0x9702230A8Ea53601f5cD2dc00fDBc13d4dF4A8c7 | 0xC891EB4cbdEFf6e073e859e987815Ed1505c2ACD |

---

## Supported Assets

### Fiat Currencies
| Code | Name | Region |
|------|------|--------|
| ZAR | South African Rand | South Africa |
| USD | US Dollar | Global |
| EUR | Euro | Europe |
| BWP | Botswana Pula | Botswana |

### Crypto Assets
| Code | Blockchain | Decimals | Notes |
|------|------------|----------|-------|
| USDC | Solana, Ethereum, Avalanche | 6 | Primary stablecoin |
| USDT | Solana, Ethereum, Avalanche | 6 | Secondary stablecoin |
| SOL | Solana | 9 | Native token |
| ETH | Ethereum | 18 | Native token |
| AVAX | Avalanche | 18 | Native token |
| ALGO | Algorand | 6 | |
| HBAR | Hedera | 8 | |
| FLDS | Flower | 0 | |

---

## Common Workflows

### Workflow: Send ZAR to Another Netfluid User
```
1. Get destination account address from user
2. Call /account/info to check balance
3. Ask user to confirm amount
4. Call /account/send with account_fk, destination, amount
5. Verify with /account/info (balance should decrease)
```

### Workflow: Buy Crypto with Fiat
```
1. Check fiat balance: /account/info(account_fk)
2. Check crypto balance: /crypto/balance(account_fk)
3. Use /account/swap to swap fiat to crypto
   - digital_asset_fk = crypto asset (from /crypto/digitalassets)
   - to_digital_asset_fk = 0 for fiat
```

### Workflow: Withdraw to Bank
```
1. Check if RBA exists: /wallet/rba_list
2. If not, create: /wallet/rba
3. Withdraw: /withdraw/to_bank(account_fk, rba_fk, amount)
```

---

## Important Notes

- **Session keys are SINGLE USE** - once converted to token, the key is invalid
- **Token TTL**: 15 minutes of inactivity, use `/wallet/verify` to extend
- **Always confirm transactions** before execution
- **Account addresses** are internal (e.g., "117000123456"), NOT blockchain addresses
- **Blockchain addresses** are for crypto (e.g., "7EDgFWKTs...")
- **Minimum withdrawal**: Varies by method, typically $10 equivalent

---
