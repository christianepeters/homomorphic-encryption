# Homomorphic Encryption

## About

Homomorphic Encryption allows to perform calculations on encrypted data without decrypting it first.

## Homomorphic Encryption

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

## HE libraries

* HElib https://github.com/homenc/HElib
* PALISADE https://palisade-crypto.org/
* SEAL https://www.microsoft.com/en-us/research/project/microsoft-seal/
* TFHE (TFHE: Fast Fully Homomorphic Encryption over the Torus) https://tfhe.github.io/tfhe/

## Companies in the HE space
* IBM
* Microsoft https://www.microsoft.com/en-us/research/project/microsoft-seal/
* Zama https://zama.ai/company/

## Community 
* Monthly meetings on FHE: https://www.meetup.com/fhe-org/
* HE Standardization https://homomorphicencryption.org/

## First steps to make FHE practical
In 2020, IBM Research conducted a project with a Brazilian Bank to implement a pilot using Homomorphic Encryption (HE) to a machine learning (ML) pipeline. Specifically they used HE in the ML to two of the important ML tasks, namely the variable selection phase of the model generation task and the prediction task.

* IBM Research, Hursley, UK and Banco Bradesco SA, Osasco, SP, Brasil ([link][DR])
* Masters et al. " Towards a Homomorphic Machine Learning Big Data Pipeline for the Financial Services Sector" ([eprint][EP191113])



[Wiki]: https://en.wikipedia.org/wiki/Homomorphic_encryption
[Bra]: https://www.ibm.com/blogs/research/2020/01/top-brazilian-bank-pilots-privacy-encryption-quantum-computers-cant-break/
[EP191113]: https://eprint.iacr.org/2019/1113.pdf
[DR]: https://www.darkreading.com/threat-intelligence/major-brazilian-bank-tests-homomorphic-encryption-on-financial-data/d/d-id/1336779
[EP201533]: https://eprint.iacr.org/2020/1533
[CCKS-mitigations]: https://github.com/homenc/HElib/blob/master/CKKS-security.md
