
# 🛡️ Solana Security & Audit Portfolio

Welcome to my professional smart contract security and auditing portfolio. Here, I showcase my methodology, tools, and detailed security findings on the Solana blockchain.

---

## 🔍 Focus Areas

### 1. Account Validation & Owner Checks
- Verifying program ownership and enforcing strict signer checks.
- Preventing account substitution and spoofing attacks in Anchor and native programs.

### 2. Math & Logic Vulnerabilities
- Preventing Integer Underflow/Overflow using safe math practices.
- Auditing reward calculations, fee distributions, and liquidity pool metrics.

### 3. Advanced Tooling & Manual Analysis
- Performing rigorous manual code reviews.
- Deep logic verification on-chain.

---

## 📂 Findings & Reports

### 🔴 High Severity
- **[H-01: Integer Underflow in Pool Distribution]** - *Description:* A critical flaw where subtraction before validation allowed attackers to drain rewards.
  - *Remediation:* Implemented `checked_sub` and strict mathematical bounds checks.
  
- **[H-02: Missing Signer Validation on Config Account]**
  - *Description:* Unauthorized users could modify system parameters due to a missing `.is_signer` check.
  - *Remediation:* Added Anchor's `Signer<'info>` constraint on the administrative account.

---

## 🛠️ Tech Stack & Skills
- **Languages:** Rust 🦀, Solidity
- **Frameworks:** Anchor, Solana CLI

---

*“Any knowledge you’ve ever picked up is a weapon now.”*
Find more of my work on [GitHub](https://github.com/karm77529-code) or connect with me on [X (Twitter)](https://x.com/karm77529).
