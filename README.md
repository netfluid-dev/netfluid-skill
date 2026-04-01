# Netfluid Skill

**Version:** 6.81.0  
**Author:** Netfluid  
**Website:** https://netfluid.io  
**Documentation:** https://doc.netfluid.io  
**Support:** support@netfluid.io

---

## Overview

The Netfluid skill provides AI agents with comprehensive banking and cryptocurrency wallet operations. It enables agents to manage fiat and crypto accounts, perform transfers, create payment bridges, and handle KYC verification — all through a unified MCP (Model Context Protocol) interface.

### Key Features

- **Multi-Currency Support**: ZAR, USD, EUR, BWP fiat currencies
- **Crypto Assets**: USDC, USDT, SOL, ETH, AVAX, ALGO, HBAR, FLDS
- **Cross-Chain Bridges**: SEPA, ACH/Wire, blockchain-to-blockchain transfers
- **Funding Methods**: Bank transfer, card deposit, Pay@, PayShap
- **Withdrawal Options**: Bank accounts, OTT mobile (South Africa)
- **KYC Integration**: Full and lite verification workflows

---

## Installation for Agents

### Option 1: Direct Installation (nanobot)

```bash
# Download and install the skill directly
GET https://api.netfluid.io/skill/download

# The skill files will be installed to your agent's skills directory
```

### Option 2: Manual Installation

```bash
# Clone or download the skill files
git clone https://github.com/netfluid-dev/netfluid-skill.git

# Copy to your agent's skills directory
cp -r netfluid-skill /path/to/agent/skills/netfluid
```

---

## Required Credentials

| Credential | Source | Description |
|------------|--------|-------------|
| `api_key` | Netfluid App → Settings → API Keys | Your personal API key |
| `session_key` | Netfluid App (one-time use) | One-time session key from the app |
| `token` | From `/session` endpoint | Session token (15 min TTL) |
| `wallet_fk` | From `/wallet/accounts_list` | Your wallet ID |
| `account_fk` | From `/wallet/accounts_list` | Specific account ID |

### Getting Started

1. **Get API Key**: Open Netfluid app → Settings → API Keys → Create new key
2. **Get Session Key**: Open Netfluid app → Get session key (single use)
3. **Exchange for Token**: Use `mcp_netfluid_session` with the session key
4. **List Accounts**: Use `mcp_netfluid_wallet_accounts_list` to get account IDs

---

## Available Tools

### Session Management
- `mcp_netfluid_session` - Get token from session key
- `mcp_netfluid_wallet_verify` - Keep session alive

### Account Operations
- `mcp_netfluid_wallet_accounts_list` - List all accounts
- `mcp_netfluid_account_info` - Get fiat balance
- `mcp_netfluid_crypto_balance` - Get crypto balance
- `mcp_netfluid_account_send` - Send fiat to another Netfluid user

### Crypto Operations
- `mcp_netfluid_crypto_spend` - Send crypto to blockchain address
- `mcp_netfluid_crypto_dex_swap` - Swap tokens via DEX
- `mcp_netfluid_crypto_digitalassets` - List supported assets
- `mcp_netfluid_crypto_optin` - Opt-in to new tokens

### Funding
- `mcp_netfluid_fund_banks` - Get bank transfer details
- `mcp_netfluid_fund_card_quote` - Quote for card deposit
- `mcp_netfluid_fund_payat` - Generate Pay@ reference
- `mcp_netfluid_fund_payshap` - Get PayShap details

### Withdrawals
- `mcp_netfluid_wallet_rba` - Save bank account
- `mcp_netfluid_withdraw_to_bank` - Withdraw to bank
- `mcp_netfluid_withdraw_ott_providers_list` - List OTT providers
- `mcp_netfluid_withdraw_to_ott` - Withdraw to mobile (South Africa)

### Bridges
- `mcp_netfluid_bridge_list` - List available bridges
- `mcp_netfluid_bridge_on_ramp` - Create virtual account (SEPA/ACH)
- `mcp_netfluid_bridge_off_ramp_sepa` - Create SEPA off-ramp
- `mcp_netfluid_bridge_off_ramp_ach_wire` - Create ACH/Wire off-ramp
- `mcp_netfluid_bridge_blockchain` - Cross-chain transfer

### KYC & Verification
- `mcp_netfluid_wallet_kyc_check` - Check full KYC status
- `mcp_netfluid_wallet_kyc_check_lite` - Check lite KYC status
- `mcp_netfluid_wallet_kyc_session_create` - Create KYC session

### Utilities
- `mcp_netfluid_wallet_mnemonic` - Get recovery phrase
- `mcp_netfluid_wallet_fee` - Calculate transaction fee
- `mcp_netfluid_beneficiaries_list` - List saved beneficiaries

---

## Supported Blockchains

| Blockchain | Native Token | Explorer |
|------------|--------------|----------|
| Algorand | ALGO | https://allo.info/ |
| Hedera | HBAR | https://hashscan.io/mainnet/ |
| Ethereum | ETH | https://etherscan.io |
| Avalanche-C | AVAX | https://snowtrace.io/ |
| Solana | SOL | https://explorer.solana.com/ |
| Fluids Network | FLDS | https://fluids.tryethernal.com/ |

---

## Supported Currencies

### Fiat
- **ZAR** - South African Rand (South Africa)
- **USD** - US Dollar (Global)
- **EUR** - Euro (Europe)
- **BWP** - Botswana Pula (Botswana)

### Crypto
- **USDC** - USD Coin (Solana, Ethereum, Avalanche)
- **USDT** - USD Tether (Solana, Ethereum, Avalanche)
- **SOL** - Solana
- **ETH** - Ethereum
- **AVAX** - Avalanche
- **ALGO** - Algorand
- **HBAR** - Hedera
- **FLDS** - Netfluids

---

## Important Rules

⚠️ **ALWAYS** ask for confirmation before executing any transaction

⚠️ **NEVER** store wallet IDs, PINs, balances, or financial details in memory

⚠️ **NEVER** create and redeem a voucher from the same wallet (circular transaction)

⚠️ Always prompt for user's own platform ID when assigning wallet to a channel

---

## Common Workflows

### Send ZAR to Another Netfluid User
```
1. Get destination account address from user
2. Call /account/info to check balance
3. Ask user to confirm amount
4. Call /account/send with account_fk, destination, amount
5. Verify with /account/info (balance should decrease)
```

### Buy Crypto with Fiat
```
1. Check fiat balance: /account/info(account_fk)
2. Check crypto balance: /crypto/balance(account_fk)
3. Use /account/swap to swap fiat to crypto
```

### Withdraw to Bank
```
1. Check if RBA exists: /wallet/rba_list
2. If not, create: /wallet/rba
3. Withdraw: /withdraw/to_bank(account_fk, rba_fk, amount)
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

---

## Files

- `SKILL.md` - Full skill documentation
- `skill-package.json` - Skill manifest and metadata
- `skills.json` - MCP tools schema
- `README.md` - This file

---

## License

Copyright © 2026 Netfluid. All rights reserved.
