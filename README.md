# BIP-39 Mnemonic Generator

This crate provides a function to generate a BIP-39 mnemonic, which is a sequence of words used for securely storing cryptocurrency keys or other sensitive information. The mnemonic is derived from entropy and can be used for purposes like key generation or recovery.

## Features

- Generates BIP-39 mnemonics of valid lengths (12, 15, 18, 21, or 24 words).
- Ensures the mnemonic follows the BIP-39 standard, including checksum calculation.
- Can be integrated for wallets for Bitcoin, Ethereum or Solana.

## Installation

To use this crate, include it in your `Cargo.toml`:

```toml
[dependencies]
bip39-mnemonic = "0.1"
```

or use the command :

```bash
cargo add bip39_mnemonic_generator
```

Ensure you add the appropriate version of the crate based on your requirements.

## How to Use

1. **Add the crate to your project**:
   In your Rust project, include the crate in `Cargo.toml` as mentioned above.

2. **Use the mnemonic generator**:
   The crate exposes a function `generate_mnemonic` that takes the number of words you want in your mnemonic. The function returns a `Result` with either a space-separated string of words or an error message if the input length is invalid.

### Example Usage

```rust
use bip39_mnemonic::generate_mnemonic;

fn main() {
    let mnemonic_result = generate_mnemonic(12); // 12 words mnemonic

    match mnemonic_result {
        Ok(mnemonic) => println!("Generated Mnemonic: {}", mnemonic),
        Err(err) => eprintln!("Error: {}", err),
    }
}
```

### Explanation

- **`generate_mnemonic(length: usize) -> Result<String, String>`**:

  - **Parameters**:
    - `length`: The desired number of words in the mnemonic. This must be a multiple of 3 (12, 15, 18, 21, or 24).
  - **Returns**: A `Result`:
    - `Ok(String)` containing the mnemonic, if successful.
    - `Err(String)` containing an error message if the length is invalid.

- **Mnemonic Generation Process**:
  - Entropy is generated randomly based on the requested length, where the number of bytes is calculated based on the word count.
  - A checksum is derived by hashing the entropy using SHA-256.
  - The entropy and checksum are combined into a bit string.
  - The bit string is split into chunks of 11 bits, which are then mapped to the corresponding words from a predefined word list.

### Valid Lengths

The `length` parameter must be one of the following values:

- 12 words (128 bits of entropy)
- 15 words (160 bits of entropy)
- 18 words (192 bits of entropy)
- 21 words (224 bits of entropy)
- 24 words (256 bits of entropy)

## Error Handling

- **Invalid length**: The `length` parameter must be a multiple of 3 and between 12 and 24 (inclusive).
- **Other potential errors**: In case of any underlying failure (such as entropy generation or checksum calculation), the function returns a descriptive error message.

## Dependencies

This crate depends on the following libraries:

- `rand`: For generating cryptographically secure random numbers.
- `sha2`: For performing SHA-256 hashing to calculate the checksum.

You can include these dependencies automatically by using the crate, but make sure you have them in your `Cargo.toml` if you're using them directly.
