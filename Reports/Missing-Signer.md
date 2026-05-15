# [H-02] Missing Signer Validation

**Severity:** High
**Vulnerability Type:** Access Control

## Description
The program allows funds to be withdrawn without verifying if the caller is the actual owner of the account.

## Vulnerable Code
```rust
pub struct Withdraw<'info> {
    pub vault: Account<'info, VaultState>,

   pub user: AccountInfo<'info>, // Should be Signer
pub struct Withdraw<'info> {
    pub vault: Account<'info, VaultState>,
    pub user: Signer<'info>, 
}
