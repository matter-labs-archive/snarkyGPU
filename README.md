# Description

This repository will contain a source code for the GPU based zkSNARK prover.

## Brief history

Recent release of [DIZK](https://github.com/scipr-lab/dizk) has put a new record in how fast zkSNARK proving can be done and how large circuits are available. Their original estimation is roughly `10 microseconds` per gate in case of utilization of 256 CPUs in a large cluster. With prover running time being linear in number of contraints in `R1CS` it makes larger circuits available to prove in a reasonable time, but still, running a proving procedure on a circuit of 60 million constraints would require `~30 minutes`. With proposals to scale the Ethereum with transaction aggregation via the provable state transition (Merkle tree update) the need for much faster prover is obvious. While there is always an option to increase the number of CPUs and linearly reduce the proving time, there is an alternative approach to utilize a hybrid infrastructure to use CPUs for data workflow and GPUs for computations of point multiplications in groups on elliptic curves, pairing calculations and polynomial interpolations.

Before the implementation starts the final architecture will be assembled [here](https://github.com/matterinc/snarkyGPU/tree/master/Architecture)

## Authors:

- Alex Vlasov, [alex.m.vlasov@gmail.com](mailto:alex.m.vlasov@gmail.com)
- Konstantin Panarin, [kopanarin@yandex.ru](mailto:kopanarin@yandex.ru)