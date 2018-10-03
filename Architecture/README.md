# Description

This file will contain open questions and architecture proposal


## Problems to solve

There are a lot of possible design decisions. While the [DIZK paper](https://eprint.iacr.org/2018/691.pdf) explains a lot about how to parallelize the zkSNARKs proving, substantial question remain on how to make a hybrid CPU/GPU solution

- [ ] Original implementation uses a custom curve with large `2^n` factor in `q-1` where `q` is a group order of the pairing curve. Find what are the consequences for using arbitrary curve with pairing support (to be compatible with BN curve in Ethreum precompiles)
- [x] Check for a prefactor in BLS12-381 curve.
    - Yes, it does. `q = 0x73eda753299d7d483339d80809a1d80553bda402fffe5bfeffffffff00000001`, so `q-1` has a factor of `2^32`
- [ ] Chose whether to use OpenCL/Cuda or [Halide](http://halide-lang.org/)
- [ ] Make a library for long arithmetics (actually, finite field arithmetics) for GPU
    - [ ] There is a great new thesis about GPU arithmetics [here](https://scholarworks.umass.edu/cgi/viewcontent.cgi?article=2252&context=dissertations_2), check if some existing library can be reused. Unfortunately such papers focus on very long arithmetics and go into FFT
    - [ ] Karatsuba or trivial
- [ ] Decide at what point computations will be "parallelized". Options:
    - [ ] Finite field arithmerics (unlikely)
    - [ ] Point multiplication and addition
    - [ ] Fixed base and variable base multiexponentiation (need to check for available memory)
- [ ] Depending from the parallelicity point decide how work will be discributed due to large latency `CPU <-> GPU`
- [ ] Check that Lagrange interpolation described in DIZK works fine for GPU (most likely related to question about large factor of `q-1`)
- [ ] Implement finite field arithmetics on GPU
- [ ] Implement chosen argorithms for point multiplications on GPU
- [ ] Implement pairing calculation on GPU
- [ ] Implement CPU part for dataflow
    - [ ] Chose distributed computing framework
    - [ ] Check whether GPU programs can be easily called from Java (if we chose Apache Spark)


