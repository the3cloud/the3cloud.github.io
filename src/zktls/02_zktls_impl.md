# zkTLS Implementation

The3Cloud zkTLS is implemented based on the [zero-knowledge virtual machine (zkVM)](https://dev.risczero.com/api/zkvm/zkvm-specification) using either [Risc0](https://risczero.com/) or [sp1](https://github.com/succinctlabs/sp1) as backend.


The zkTLS is inspired by the project of [rustls-rustcrypto](https://github.com/RustCrypto/rustls-rustcrypto) and [rustls](https://github.com/rustls/rustls). We extend the `rustls` with a replayable stack for TLS connections, which records the entire TLS handshake process and populates the public inputs with the recorded data. Then, running a **full stack** simulation in the zkVM to generate the zk proofs. We implements the simulation circuit in both of Risc0 and sp1 backends. We choose to produce the proth16 proofs so that its small enough to be verified by verifier contracts on blockchains.

## Running The3Cloud zkTLS

Detailed instructions can be found in the [README.md](https://github.com/the3cloud/zktls/blob/main/README.md) file of the zkTLS project.

We provide a quick sheetcheat here for running the benchamark of zkTLS on machine with GPU.

***Prerequisite***

Please make sure the following softwares are installed on your machine:

- [Rust](https://www.rust-lang.org/tools/install)
- [Cargo](https://doc.rust-lang.org/cargo/getting-started/installation.html)
- [Nvidia GPU Driver](https://developer.nvidia.com/cuda-downloads)
- [Nvidia Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html), optional needed with sp1 backend.
- [Nvidia CLI](https://developer.nvidia.com/cli)


```sh
git clone https://github.com/the3cloud/zktls.git
cd zktls
cargo build

# benchmark with spc
RUST_LOG=info cargo run --release --bin t3zktls-benchmark \
	--features sp1-backend-cuda

RUST_LOG=info cargo run --release --bin t3zktls-benchmark \
	--features risc0-backend-cuda

# benchmark with cpu with avx512f supports
RUST_LOG=info RUSTFLAGS="-C target-cpu=native -C target-feature=+avx512f" \
	cargo run --release --bin t3zktls-benchmark
```

## Hardware Requirements

Both of the Risc0 and sp1 backends are tested can can run on CPU and GPU. We recommend to use GPU for better performance and production environment. Any one who is interested in running it on their own hardware, please refer to the following benchmark results.

***RISC0 Backend***

<!-- > Generate the groth 16 proof. -->

| Input Length | Output Length | Cycle (user / total)    | CPU Model          | Core Number | Max Memory | GPU Model   | Max GPU Memory | Proof Time     |
| ------------ | ------------- | ----------------------- | ------------------ | ----------- | ---------- | ----------- | -------------- | -------------- |
| 56B          | 426B          | 37,758,703 / 41,943,040 | 8369B <sup>1</sup> | 8           | 4460MB     | Nvidia A10  | 9246MiB        | 282.295366971s |
| 56B          | 426B          | 37,758,703 / 41,943,040 | 8163 <sup>2</sup>  | 8           | 4908MB     | Nvidia T4   | 8605MiB        | 426.617830981s |
| 56B          | 426B          | 37,758,703 / 41,943,040 | 8255C <sup>3</sup> | 8           | 4491MB     | Nvidia V100 | 9983MiB        | 324.583163623s |


***SP1 Backend***

> Generate the groth 16 proof.

| Input Length | Output Length | Cycle      | CPU Model          | Core Number | Max Memory | GPU Model   | Max GPU Memory | Proof Time     |
| ------------ | ------------- | ---------- | ------------------ | ----------- | ---------- | ----------- | -------------- | -------------- |
| 56B          | 426B          | 36,018,041 | 8369B <sup>1</sup> | 8           | 19.2GB     | Nvidia A10  | 14682MiB       | 174.762700752s |
| 56B          | 426B          | 36,018,041 | 8163 <sup>2</sup>  | 8           | 14.1GB     | Nvidia T4   | 13725MiB       | 464.79359922s  |
| 56B          | 426B          | 36,018,041 | 8255C <sup>3</sup> | 12          | 19.6GB     | Nvidia V100 | 14747MiB       | 182.572047771s |

<!-- ***Comments*** -->

> 1: Intel(R) Xeon(R) Platinum 8369B CPU @ 2.90GHz
>
> 2: Intel(R) Xeon(R) Platinum 8163 CPU @ 2.50GHz
>
> 3: Intel(R) Xeon(R) Platinum 8255C CPU @ 2.50GHz