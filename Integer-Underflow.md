
# [H-01] Integer Underflow in Balance Deduction

**Severity:** High
**Vulnerability Type:** Math Error

## Description
In Solana programs, standard subtraction `-` can lead to an integer underflow if the result is negative. Since Rust types like `u64` are unsigned, a result of `-1` becomes a massive positive number.

## Vulnerable Code
```rust
user.balance -= amount; // No check if amount > balance


user.balance = user.balance.checked_sub(amount).ok_or(error::InsufficientFunds)?;
