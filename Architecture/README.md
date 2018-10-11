# Description

This file will contain open questions and architecture proposal


## Problems to solve

There are a lot of possible design decisions. While the [DIZK paper](https://eprint.iacr.org/2018/691.pdf) explains a lot about how to parallelize the zkSNARKs proving, substantial question remain on how to make a hybrid CPU/GPU solution

- [x] Original implementation uses a custom curve with large `2^n` factor in `q-1` where `q` is a group order of the pairing curve. Find what are the consequences for using arbitrary curve with pairing support (to be compatible with BN curve in Ethreum precompiles)
    - Yes, it's required for efficient polynomial evaluation and interpolation, so we stick to [EDIT] BN256 curve standard for Ethereum
    - While it still may be feasible on GPU to use naive Lagrange (`n^2` difficulty), it'll not be in the priority list
- [x] Check for a prefactor in BLS12-381 curve.
    - Yes, it does. `q = 0x73eda753299d7d483339d80809a1d80553bda402fffe5bfeffffffff00000001`, so `q-1` has a factor of `2^32`
- [x] Check for a prefactor in Ethereum BN256 curve.
    - Yes, it does. `q = 0x30644e72e131a029b85045b68181585d2833e84879b9709143e1f593f0000001`, so `q-1` has a factor of `2^28`
- [x] Chose whether to use OpenCL/Cuda or [Halide](http://halide-lang.org/)
    - Use CUDA for now due to architecture-specific choices for multiexponentiation
- [ ] Choose a library for long arithmetics (actually, finite field arithmetics) for GPU
    - [ ] There is a great new thesis about GPU arithmetics [here](https://scholarworks.umass.edu/cgi/viewcontent.cgi?article=2252&context=dissertations_2), check if some existing library can be reused. Unfortunately such papers focus on very long arithmetics and go into FFT
    - [x] This [one](https://github.com/NVlabs/xmp) from NVidia looks like a good candidate to start
- [x] Decide at what point computations will be "parallelized". Options:
    - [x] Finite field arithmerics
        - Lagrange interpolation and polynomial evaluation are well-suited
    - [x] Point multiplication and addition
    - [x] Fixed base multiexponentiation. This options is ok for CUDA architecture, but only needed for a setuper
    - [x] Variable base multiexponentiation
        -  This one most likely will be "multiply independently on GPU - add somewhere" due to high memory requirements and may be too complicated logic on Peppinger. Unfortunately this is required for a prover
- [x] Depending from the parallelicity point decide how work will be discributed due to large latency `CPU <-> GPU`
    - More or less done, few points above
- [x] Check that Lagrange interpolation described in DIZK works fine for GPU (most likely related to question about large factor of `q-1`)
    - Stick for DIZK approach with large factor in `q-1` 
- [ ] Implement finite field arithmetics on GPU
- [ ] Implement chosen argorithms for point multiplications on GPU
- [ ] Implement pairing calculation on GPU - optional, not needed for prover
- [ ] Implement CPU part for dataflow
    - [ ] Chose distributed computing framework
    - [ ] Check whether GPU programs can be easily called from Java (if we chose Apache Spark)


