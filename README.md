# SP1 Project Template

This is a template for creating an end-to-end [SP1](https://github.com/succinctlabs/sp1) project
that can generate a proof of any RISC-V program.

## gitSubmodules の追加方法

```bash
git submodule update --init --recursive
```

## Requirements

- [Rust](https://rustup.rs/)
- [SP1](https://docs.succinct.xyz/getting-started/install.html)

## Running the Project

There are four main ways to run this project: build a program, execute a program, generate a core proof, and
generate an EVM-compatible proof.

### Build the Program

To build the program, run the following command:

```sh
cd program
cargo prove build
```

### Execute the Program

To run the program without generating a proof:

```sh
cd script
cargo run --release -- --execute
```

This will execute the program and display the output.

```bash
warning: fibonacci-script@0.1.0: rustc +succinct --version: "rustc 1.85.0-dev\n"
warning: fibonacci-script@0.1.0: fibonacci-program built at 2025-03-20 00:12:21
   Compiling git2 v0.19.0
   Compiling sp1-sdk v4.0.0
    Finished `release` profile [optimized] target(s) in 1m 48s
     Running `/Users/harukikondo/git/sp1-project-template/target/release/fibonacci --execute`
thread 'main' panicked at /Users/harukikondo/.cargo/registry/src/index.crates.io-6f17d22bba15001f/sp1-sdk-4.0.0/src/env/mod.rs:67:18:
Invalid SP1_PROVER value. Expected one of: mock, cpu, cuda, or network. Got: 'local'.
Please set the SP1_PROVER environment variable to one of the supported values.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

### Generate a Core Proof

To generate a core proof for your program:

```sh
cd script
cargo run --release -- --prove
```

```bash
warning: `/Users/harukikondo/.cargo/config` is deprecated in favor of `config.toml`
note: if you need to support cargo 1.38 or earlier, you can symlink `config` to `config.toml`
warning: `/Users/harukikondo/.cargo/config` is deprecated in favor of `config.toml`
note: if you need to support cargo 1.38 or earlier, you can symlink `config` to `config.toml`
warning: fibonacci-script@0.1.0: rustc +succinct --version: "rustc 1.85.0-dev\n"
warning: fibonacci-script@0.1.0: fibonacci-program built at 2025-03-20 00:12:21
    Finished `release` profile [optimized] target(s) in 0.36s
     Running `/Users/harukikondo/git/sp1-project-template/target/release/fibonacci --prove`
thread 'main' panicked at /Users/harukikondo/.cargo/registry/src/index.crates.io-6f17d22bba15001f/sp1-sdk-4.0.0/src/env/mod.rs:67:18:
Invalid SP1_PROVER value. Expected one of: mock, cpu, cuda, or network. Got: 'local'.
Please set the SP1_PROVER environment variable to one of the supported values.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

### Generate an EVM-Compatible Proof

> [!WARNING]
> You will need at least 128GB RAM to generate a Groth16 or PLONK proof.

To generate a proof that is small enough to be verified on-chain and verifiable by the EVM:

```sh
cd script
cargo run --release --bin evm -- --system groth16
```

this will generate a Groth16 proof. If you want to generate a PLONK proof, run the following command:

```sh
cargo run --release --bin evm -- --system plonk
```

These commands will also generate fixtures that can be used to test the verification of SP1 zkVM proofs
inside Solidity.

### Retrieve the Verification Key

To retrieve your `programVKey` for your on-chain contract, run the following command in `script`:

```sh
cargo run --release --bin vkey
```

## Using the Prover Network

We highly recommend using the Succinct prover network for any non-trivial programs or benchmarking purposes. For more information, see the [setup guide](https://docs.succinct.xyz/docs/generating-proofs/prover-network).

To get started, copy the example environment file:

```sh
cp .env.example .env
```

Then, set the `SP1_PROVER` environment variable to `network` and set the `NETWORK_PRIVATE_KEY`
environment variable to your whitelisted private key.

For example, to generate an EVM-compatible proof using the prover network, run the following
command:

```sh
SP1_PROVER=network NETWORK_PRIVATE_KEY=... cargo run --release --bin evm
```
