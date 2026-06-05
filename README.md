
use anchor_lang::prelude::*;

declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");

#[program]
pub mod drift_exploit_study {
    use super::*;

    /// ❌ VULNERABLE APPROACH
    /// This function verifies the signature/authority of the nonce correctly, 
    /// but permits instant state updates without checking the execution sequence 
    /// or enforcing a governance timelock window.
    pub fn execute_admin_action_vulnerable(ctx: Context<AdminAction>) -> Result<()> {
        let nonce_account = &ctx.accounts.nonce_account;
        
        // 1. Validate authority signature
        if nonce_account.authority != ctx.accounts.signer.key() {
            return Err(ErrorCode::InvalidAuthority.into());
        }
        
        // 2. Vulnerability: Immediate architectural change allowed in batch execution
        ctx.accounts.protocol_config.admin = ctx.accounts.new_admin.key();
        
        Ok(())
    }

    ///  SECURED APPROACH (Mitigation)
    /// Enforces strict execution time windows and sequence rules to prevent 
    /// pre-signed nonce batching attacks.
    pub fn execute_admin_action_secure(ctx: Context<AdminAction>) -> Result<()> {
        let nonce_account = &ctx.accounts.nonce_account;
        let clock = Clock::get()?;

        // 1. Validate authority signature
        if nonce_account.authority != ctx.accounts.signer.key() {
            return Err(ErrorCode::InvalidAuthority.into());
        }

        // 2. Mitigation: Enforce strict execution window / governance timelock
        if clock.unix_timestamp < ctx.accounts.protocol_config.next_execution_window {
            return Err(ErrorCode::TimelockActive.into());
        }

        // 3. Securely update state
        ctx.accounts.protocol_config.admin = ctx.accounts.new_admin.key();
        
        Ok(())
    }
}

#[derive(Accounts)]
pub struct AdminAction<'info> {
    #[account(mut)]
    pub protocol_config: Account<'info, ProtocolConfig>,
    pub nonce_account: Account<'info, NonceAccountMock>,
    pub signer: Signer<'info>,
    /// CHECK: Target new admin address
    pub new_admin: UncheckedAccount<'info>,
}

#[account]
pub struct ProtocolConfig {
    pub admin: Pubkey,
    pub next_execution_window: i64,
}

#[account]
pub struct NonceAccountMock {
    pub authority: Pubkey,
    pub nonce_value: u64,
}

#[error_code]
pub enum ErrorCode {
    #[msg("The provided authority does not match the nonce account.")]
    InvalidAuthority,
    #[msg("Governance timelock is still active. Instruction execution window denied.")]
    TimelockActive,
}
