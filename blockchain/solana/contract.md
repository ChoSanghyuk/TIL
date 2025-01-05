# Solana Contract



## 환경 설정



### Anchor.toml

```toml
[toolchain]

[features]
resolution = true
skip-lint = false

[programs.localnet]
token = "2pmCsVGBDXVEcHs3v2nTLVGVL2FyvrTdRXq3VkE38euD"

[registry]
url = "https://api.apr.dev"

[provider]
cluster = "Localnet"
wallet = "~/.config/solana/id.json"

[scripts]
test = "cargo test"

```

- `cluster` : 배포 환경
  - devnet : `cluster = "Devnet"`



### 로컬 네트워크 환경 기동

```bash
solana config set -ul    # For localhost
solana-test-validator
```



## 기본 컨트랙트



### anchor 프로젝트 생성

```bash
anchor init my-project
```

- If you prefer Rust for testing, initialize your project with the `--test-template rust` flag.



### sample program 구조

```rust
use anchor_lang::prelude::*;
 
declare_id!("11111111111111111111111111111111");
 
#[program]
mod hello_anchor {
    use super::*;
    pub fn initialize(ctx: Context<Initialize>, data: u64) -> Result<()> {
        ctx.accounts.new_account.data = data;
        msg!("Changed data to: {}!", data);
        Ok(())
    }
}
 
#[derive(Accounts)]
pub struct Initialize<'info> {
    #[account(init, payer = signer, space = 8 + 8)]
    pub new_account: Account<'info, NewAccount>,
    #[account(mut)]
    pub signer: Signer<'info>,
    pub system_program: Program<'info, System>,
}
 
#[account]
pub struct NewAccount {
    data: u64,
}
```

- `declare_id!`

  - program ID, a unique identifier for your program

  - By default, it is the public key of the keypair generated in `/target/deploy/my_project-keypair.json`.
  - `anchor keys sync` : To update the value of the program ID in the `declare_id` macro with keypair file

- `#[program]`

  - Specifies the module containing the program’s instruction logic
  - Each public function within this module corresponds to an instruction that can be invoked

- Instruction Context

  - Instruction handlers are functions that define the logic executed when an instruction is invoked.

  - The first parameter of each handler is a `Context<T>` type

    - `T` : a struct implementing the `Accounts` trait & specifies the accounts the instruction requires

    - The `Context` type provides the instruction with access to the following non-argument inputs

      ```rust
      pub struct Context<'a, 'b, 'c, 'info, T> {
          /// Currently executing program id.
          pub program_id: &'a Pubkey,
          /// Deserialized accounts.
          pub accounts: &'b mut T,
          /// Remaining accounts given but not deserialized or validated.
          /// Be very careful when using this directly.
          pub remaining_accounts: &'c [AccountInfo<'info>],
          /// Bump seeds found during constraint validation. This is provided as a
          /// convenience so that handlers don't have to recalculate bump seeds or
          /// pass them in as arguments.
          pub bumps: BTreeMap<String, u8>,
      }
      ```

    - The `Context` fields can be accessed in an instruction using dot notation:

      - `ctx.accounts`: The accounts required for the instruction

      - `ctx.program_id`: The program's public key (address)

      - `ctx.remaining_accounts`: Additional accounts not specified in the `Accounts` struct.

      - `ctx.bumps`: Bump seeds for any [Program Derived Address (PDA)](https://solana.com/docs/core/pda) accounts specified in the `Accounts` struct

  - Additional parameters are arguments instructions use

- `#[derive(Accounts)]`

  - Applied to structs to indicate a list of accounts required by an instruction
  - simplifies account validation and serialization and deserialization of account data.
  - Struct field
    - Each field in the struct represents an account required by an instruction. 
    - The naming of each field is arbitrary, but recommended to use a descriptive name that indicates the purpose

- `#[account]`

  - Applied to structs to create custom account types for the program

  - The key functionalities of the `#[account]` macro include:
    - Assign Program Owner
      - the program owner of the account is automatically set to the program specified in `declare_id`.
    - Set Discriminator
      - A unique 8 byte discriminator, specific to the account type
        - added as the first 8 bytes of account data during its initialization. 
      - helps in differentiating account types and is used for account validation
    - Data Serialization and Deserialization
      - Account data is automatically serialized and deserialized as the account type.
  - **When creating an account in an Anchor program, 8 bytes must be allocated for the discriminator.**
    - `#[account(init, payer = signer, space = 8 + 8)]`
      - 8+8인 이유 : `8 (discriminator) + 8 (u64) = 16 bytes`.



### 테스트

```bash
anchor test 	
```

- localnet 기준 동작 순서
  1. Start a local Solana validator
  2. Build and deploy your program to the local cluster
  3. Run the tests in the `tests` folder
  4. Stop the local Solana validator



### 배포

```bash
anchor build
anchor deploy
```





:bulb: **close program**

To reclaim the SOL allocated to a program account, you can close your Solana program.

```bash
solana program close <PROGRAM_ID>
```

**once a program is closed, the program ID cannot be reused to deploy a new program**





Program account에 대한 권한은 Program이 가짐

단, 배포 시 초기 authority 설정으로 배포하는 개인이 program을 통해서 program account를 통제할 수 있도록 함

program이 closed 되면 program account의 데이터는 유지되지만, 더 이상 동작되지 않는 고아 상태가 됨.

program이 closed 되지 않는 지 어떻게 보장할 지 조사 필요







## SPL



The SPL (Solana Program Library) programs, like the token program (`spl-token`), are precompiled programs provided by the Solana ecosystem. However, **they do not automatically exist on a local network** (such as when using `solana-test-validator`) unless explicitly included.

------

### **Steps to Use SPL Programs on a Local Network**

1. **Check for Pre-Deployed SPL Programs** By default, the `solana-test-validator` will pre-deploy some programs, including the SPL Token Program (`spl-token`), for convenience. You can verify this by running:

   ```bash
   solana address -k ~/.config/solana/id.json
   ```

   Or inspect the `--help` output of the `solana-test-validator`:

   ```bash
   solana-test-validator --help
   ```

2. **Include SPL Programs in the Local Validator** If the desired SPL program isn’t included by default, you can manually deploy it to your local network.

------

### **Deploy SPL Programs Manually**

If the SPL program is not available on your local network, you need to deploy it yourself.

#### **Steps to Deploy SPL Programs Locally**

1. **Download the SPL Program** Clone the Solana Program Library repository:

   ```bash
   git clone https://github.com/solana-labs/solana-program-library.git
   cd solana-program-library
   ```

2. **Build the Program** Navigate to the desired SPL program (e.g., the token program):

   ```bash
   cd token/program
   ```

   Build the program using Solana's build tools:

   ```bash
   cargo build-bpf
   ```

   This generates the program's shared object file (`.so`), which is the deployable program binary.

3. **Deploy the Program** Use the Solana CLI to deploy the program to your local network:

   ```bash
   solana program deploy path/to/your_program.so
   ```

   Replace `path/to/your_program.so` with the path to the `.so` file generated in the previous step.

4. **Get the Program ID** After deployment, the CLI will output a **Program ID**. This ID is used to interact with the deployed program.

------

### **Verify SPL Programs on Local Validator**

You can confirm the presence of SPL programs by checking the deployed programs list:

```bash
solana program show --program-id <PROGRAM_ID>
```

------

### **Default Pre-Deployed Programs**

Some SPL programs may already be pre-deployed when using the `solana-test-validator`. For example:

- SPL Token Program (`Tokenkeg...`)
- Associated Token Program (`AToken...`)

Use the `--bpf-program` flag to include specific programs when starting the local validator:

```bash
solana-test-validator --bpf-program <PROGRAM_ID> <PROGRAM_BINARY>
```

------

Let me know if you want specific help with deploying or testing an SPL program!





---



### anchor token

Here's an example of how you can create an Anchor-based Solana program that mimics the functionality of an SPL Token program or an Ethereum ERC-20 token. This program allows you to:

1. Initialize a new token mint.
2. Mint tokens to an account.
3. Transfer tokens between accounts.
4. Check balances.

------

### **Anchor Program Code:**

```rust
use anchor_lang::prelude::*;

declare_id!("YourProgramIDHere");

#[program]
pub mod my_token {
    use super::*;

    /// Initializes a new mint with a given supply and decimals.
    pub fn initialize_mint(ctx: Context<InitializeMint>, decimals: u8, total_supply: u64) -> Result<()> {
        let mint = &mut ctx.accounts.mint;
        mint.decimals = decimals;
        mint.total_supply = total_supply;
        mint.owner = ctx.accounts.authority.key();
        Ok(())
    }

    /// Mints tokens to a given account.
    pub fn mint_tokens(ctx: Context<MintTokens>, amount: u64) -> Result<()> {
        let mint = &mut ctx.accounts.mint;
        let to_account = &mut ctx.accounts.to_account;

        require_keys_eq!(ctx.accounts.authority.key(), mint.owner, CustomError::Unauthorized);
        require!(mint.total_supply >= amount, CustomError::InsufficientSupply);

        to_account.balance += amount;
        mint.total_supply -= amount;
        Ok(())
    }

    /// Transfers tokens between accounts.
    pub fn transfer(ctx: Context<Transfer>, amount: u64) -> Result<()> {
        let from_account = &mut ctx.accounts.from_account;
        let to_account = &mut ctx.accounts.to_account;

        require!(from_account.balance >= amount, CustomError::InsufficientFunds);

        from_account.balance -= amount;
        to_account.balance += amount;
        Ok(())
    }
}

#[derive(Accounts)]
pub struct InitializeMint<'info> {
    #[account(init, payer = authority, space = 8 + 32 + 1 + 8)]
    pub mint: Account<'info, Mint>,
    #[account(mut)]
    pub authority: Signer<'info>,
    pub system_program: Program<'info, System>,
}

#[derive(Accounts)]
pub struct MintTokens<'info> {
    #[account(mut, has_one = owner)]
    pub mint: Account<'info, Mint>,
    #[account(mut)]
    pub to_account: Account<'info, TokenAccount>,
    pub authority: Signer<'info>,
}

#[derive(Accounts)]
pub struct Transfer<'info> {
    #[account(mut)]
    pub from_account: Account<'info, TokenAccount>,
    #[account(mut)]
    pub to_account: Account<'info, TokenAccount>,
}

#[account]
pub struct Mint {
    pub owner: Pubkey,
    pub total_supply: u64,
    pub decimals: u8,
}

#[account]
pub struct TokenAccount {
    pub balance: u64,
}

#[error_code]
pub enum CustomError {
    #[msg("Insufficient token supply")]
    InsufficientSupply,
    #[msg("Insufficient balance in account")]
    InsufficientFunds,
    #[msg("Unauthorized operation")]
    Unauthorized,
}
```

------

### **Explanation of the Code**

1. **Mint Initialization (`initialize_mint`):**
   - Sets up a new mint with a specific total supply and number of decimals.
   - The authority (creator) becomes the owner of the mint.
2. **Minting Tokens (`mint_tokens`):**
   - Allows the mint owner to create tokens and assign them to a token account.
   - Decreases the available supply in the mint.
3. **Transfer Tokens (`transfer`):**
   - Moves tokens from one token account to another.
   - Validates that the sender has sufficient tokens.
4. **Accounts:**
   - **Mint:** Represents the token mint, including total supply, decimals, and owner.
   - **TokenAccount:** Represents a user’s token balance.
5. **Error Handling:**
   - Custom errors are defined for unauthorized access, insufficient supply, and insufficient funds.

------

### **Anchor CLI Commands for Deployment**

1. **Initialize Anchor Project:**

   ```bash
   anchor init my_token
   cd my_token
   ```

2. **Replace the Code:** Replace the content of `programs/my_token/src/lib.rs` with the code above.

3. **Build and Deploy:**

   ```bash
   anchor build
   anchor deploy
   ```

4. **Testing:** Write test cases in `tests/my_token.ts` to:

   - Initialize the mint.
   - Mint tokens to an account.
   - Transfer tokens between accounts.
   - Check balances.

------

### **Client Side Integration**

Use Anchor’s TypeScript SDK to interact with your deployed program. Here's an example of initializing the mint:

```typescript
import * as anchor from "@project-serum/anchor";

const provider = anchor.AnchorProvider.local();
anchor.setProvider(provider);

const program = anchor.workspace.MyToken;

// Example: Initialize Mint
const mint = anchor.web3.Keypair.generate();
await program.rpc.initializeMint(2, new anchor.BN(1_000_000), {
  accounts: {
    mint: mint.publicKey,
    authority: provider.wallet.publicKey,
    systemProgram: anchor.web3.SystemProgram.programId,
  },
  signers: [mint],
});
console.log("Mint initialized:", mint.publicKey.toBase58());
```

------

This program demonstrates the core concepts of creating a simple token system using Anchor. You can extend it to include advanced features like burning tokens, freezing accounts, or setting authorities. Let me know if you'd like to dive deeper!



