# 📜 scroll-prover 📜
[![Unit Test](https://github.com/scroll-tech/scroll-prover/actions/workflows/unit_test.yml/badge.svg)](https://github.com/scroll-tech/scroll-prover/actions/workflows/unit_test.yml)
![issues](https://img.shields.io/github/issues/scroll-tech/scroll-prover)

## Usage

### Latest Release

v0.10.3 is used in Scroll Mainnet currently. Branch `main` HEAD is used for development.

### Prerequisite

Install Solidity compiler `solc` of version `0.8.19` via [svm-rs](https://github.com/alloy-rs/svm-rs):
```shell
cargo install svm-rs
svm install 0.8.19
```

Download all setup params(SRS), degree `20`, `24` and `26` are used in [config](https://github.com/scroll-tech/scroll-prover/tree/main/integration/configs).
Could only download params of degree `26`, but it may affect performance (when downsizing params).
```shell
make download-setup -e degree=20
make download-setup -e degree=24
make download-setup -e degree=26
```
Or specify other degree and target directory to download.
```shell
# As default `degree=26` and `params_dir=./integration/params`.
make download-setup -e degree=DEGREE params_dir=PARAMS_DIR
```

These params are mirrored from [PSE's converted setup files](https://github.com/han0110/halo2-kzg-srs) which was originally created by `perpetual-powers-of-tau`.

### Testing

`make test-chunk-prove` and `make test-e2e-prove` are the main testing entries for multi-level circuit constraint system of scroll-prover. Developers could understand how the system works by reading the codes of these tests.

And there are other tests:
- `make test-inner-prove` could be used to test the first-level circuit.
- `make test-batch-prove` could be used to test the final two levels.

### Binaries

Could use the following command to run binaries locally.

Run zkevm prover to generate chunk proof (work directory is `./integration`)
```shell
# Params file should be located in `./integration/params`.
cargo run --release --bin trace_prover -- --params=params --trace=tests/extra_traces/batch_34700/chunk_1236462/block_4176564.json
```

### Verifier Contract

Both YUL and bytecode of verifier contract could be generated when running aggregation tests (`make test-e2e-prove`). After running aggregation tests, a new folder is created in `integration` folder of scroll-prover and named like `integration/outputs/e2e_tests_*`. It contains below files:

- Chunk protocol: `chunk_chunk_0.protocol`
- Chunk VK: `vk_chunk_0.vkey`
- Batch VK: `vk_batch_agg.vkey`
- Verifier YUL source code: `evm_verifier.yul`
- Verifier bytecode: `evm_verifier.bin`

The YUL source code is generated by params, VK and num instance of proof, could reference [gen_evm_verifier function](https://github.com/scroll-tech/snark-verifier/blob/develop/snark-verifier-sdk/src/evm_api.rs#L121) in snark-verifier.

The verifier bytecode is compiled from YUL source code, it invokes Solidity compiler (`0.8.19` as mentioned above) command line with specified parameters, could reference [compile_yul function](https://github.com/scroll-tech/snark-verifier/blob/develop/snark-verifier/src/loader/evm/util.rs#L107) in snark-verifier.

## License

Licensed under either of

- Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.
