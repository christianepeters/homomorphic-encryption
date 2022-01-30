# Homomorphic Encryption

**tl;dr** Homomorphic Encryption allows to perform calculations on encrypted data without decrypting it first.

## About

### Intros for non-cryptographers

* [Introduction to FHE by Pascal Paillier](https://fhe.org/talks/introduction-to-fhe-by-pascal-paillier)
* Marc Joye’s blog for Zama FHE: [Homomorphic Encryption 101](https://medium.com/zama-ai/homomorphic-encryption-101-c1524fb76013)
* FHE Community of Interest: [FHE.org](https://fhe.org/)
  * Monthly meetings on FHE: https://www.meetup.com/fhe-org/
  * Chat space https://discord.fhe.org (wealth of information and discussions on how to use the libraries etc)
* HE Standardization https://homomorphicencryption.org/


### Example HE cryptosystems

* RSA cryptosystem (unbounded number of modular multiplications)
* ElGamal cryptosystem (unbounded number of modular multiplications)
* Goldwasser–Micali cryptosystem (unbounded number of exclusive or operations)
* Benaloh cryptosystem (unbounded number of modular additions)
* Paillier cryptosystem (unbounded number of modular additions)

See [Wikipedia][Wiki]

### Breakthrough Gentry's FHE
Ph.D. thesis by Craig Gentry
https://crypto.stanford.edu/craig/craig-thesis.pdf

### FHE more practical

* Brakerski-Gentry-Vaikuntanathan (BGV, 2011) scheme, building on techniques of Brakerski-Vaikuntanathan
```
=> good for masking and integer arithmetic
```
* The NTRU-based scheme by Lopez-Alt, Tromer, and Vaikuntanathan (LTV, 2012)
* Brakerski/Fan-Vercauteren (BFV, 2012) scheme, building on Brakerski's scale-invariant cryptosystem
* NTRU-based scheme by Bos, Lauter, Loftus, and Naehrig (BLLN, 2013), building on LTV and Brakerski's scale-invariant cryptosystem
* Cheon-Kim-Kim-Song (CKKS, 2016) scheme.
  * Note that there were some issues with CCKS reported recently by Li and Micciancio ([eprint report][EP201533]). In response, the HElib team published a [list of mitigations][CCKS-mitigations].
```
=> floating point arithmetic => machine learning
```

## Libraries

### HElib

* Website: https://homenc.github.io/HElib
* Repo: https://github.com/HomEnc/HElib
* Docs: https://homenc.github.io/HElib/
* Bug report: https://github.com/homenc/HElib/issues
* Math behind HElib: [HElib-design.pdf](https://homenc.github.io/HElib/documentation/Design_Document/HElib-design.pdf)
* FHE-toolkit https://github.com/ibm/fhe-toolkit-linux (containerized IDE with precompiled libraries and examples)


### SEAL

* Website: https://www.microsoft.com/en-us/research/project/microsoft-seal
* Repo: https://github.com/microsoft/SEAL
* Docs: https://github.com/microsoft/SEAL#introduction
* Bug report: https://github.com/microsoft/SEAL/issues

### Concrete

* Website: https://zama.ai/concrete
* Repo: https://github.com/zama-ai/concrete
* Docs: https://docs.zama.ai/concrete/lib
* Bug report: https://github.com/zama-ai/concrete/issues
* Programmable Bootstrapping whitepaper
* Python package: GitHub: https://github.com/zama-ai/concrete-numpy

### PALISADE

* Website: https://palisade-crypto.org/
* Repo: https://gitlab.com/palisade/palisade-release
* Docs: https://palisade-crypto.org/documentation
* Bug report: https://gitlab.com/palisade/palisade-release/-/issues

### Intel Homomorphic Encryption Acceleration Library (HEXL)

* [Intel HEXL](https://intel.github.io/hexl/v1.2.3/doxygen/html/index.html)
* Repo: https://github.com/intel/hexl


### Google C++ compiler

* Repo: https://github.com/google/fully-homomorphic-encryption
* Examples: https://github.com/google/fully-homomorphic-encryption/tree/main/transpiler/examples
* Blog Post: [Our latest updates on Fully Homomorphic Encryption](https://developers.googleblog.com/2021/06/our-latest-updates-on-fully-homomorphic-encryption.html)


## First steps to make FHE practical
In 2020, IBM Research conducted a project with a Brazilian Bank to implement a pilot using Homomorphic Encryption (HE) to a machine learning (ML) pipeline. Specifically they used HE in the ML for both the variable selection of the model generation and the actual prediction computation.

* IBM Research, Hursley, UK and Banco Bradesco SA, Osasco, SP, Brasil ([link][DR])
* Masters et al. " Towards a Homomorphic Machine Learning Big Data Pipeline for the Financial Services Sector" ([eprint][EP191113])



[Wiki]: https://en.wikipedia.org/wiki/Homomorphic_encryption
[Bra]: https://www.ibm.com/blogs/research/2020/01/top-brazilian-bank-pilots-privacy-encryption-quantum-computers-cant-break/
[EP191113]: https://eprint.iacr.org/2019/1113.pdf
[DR]: https://www.darkreading.com/threat-intelligence/major-brazilian-bank-tests-homomorphic-encryption-on-financial-data/d/d-id/1336779
[EP201533]: https://eprint.iacr.org/2020/1533
[CCKS-mitigations]: https://github.com/homenc/HElib/blob/master/CKKS-security.md
