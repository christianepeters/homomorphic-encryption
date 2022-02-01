# HElib

## About

I tried out [HElib][HElib], a library for homomorphic encryption, written in C++.

I simplified the instructions in the HElib [INSTALL.md][HElib-install]. I don't plan to run any huge computations. It's all about trying out the library and computing a first example.


Last version I tried: **HElib v2.2.1** (1-Feb-2022)


## HElib documentation

* HElib on Github https://github.com/homenc/HElib
* Documentation of the library: https://homenc.github.io/HElib/
* The math behind the HElib: [HElib-design.pdf][helib-math]
* [HElib namespaces][namespaces] to keep track of all the function, class, and variable names in various libraries
* Pre-compiled examples: https://github.com/homenc/HElib/tree/master/examples


## Install the HElib library

Steps for

* [MacOS](#macos)
* [Ubuntu](#ubuntu)

I'm following the instructions in the HElib [INSTALL.md][HElib-install].

### MacOS

Latest version I tried on Mac was v2.0.0; see [Ubuntu](#ubuntu) for v2.2.1.

Check pre-reqs on MacOS:
* Apple clang >= 11.0.0
```
$ clang -v
Apple clang version 12.0.0 (clang-1200.0.32.2)
Target: x86_64-apple-darwin19.6.0
Thread model: posix
InstalledDir: /Library/Developer/CommandLineTools/usr/bin
```
* Xcode Command Line Tools
```
xcode-select --install
```
* cmake >= 3.17.3  
```
brew install cmake
```
CMake generates Makefiles which enable you to compile the application with gcc ([link to cmake intro][cmakeintro])

Download and compile the library:
```
git clone https://github.com/homenc/HElib.git
cd HElib
mkdir build
cd build
cmake -DPACKAGE_BUILD=ON ..
make -j4          # seems to be good enough on a small Macbook
sudo make install
```
The `helib` library is now installed in `/usr/local/./helib_pack/`.

Jump to the [next chapter](#examples) to see how to work with an example.


### Ubuntu

Check pre-reqs on Ubuntu:

* cmake
* g++ (see warning below)
* patchelf >= 0.9
* m4 >= 1.4.16
* GMP and NTL libraries
* pthreads (new!)

WARNING: I had compilation issues on my new machine which came with a Tiger Lake Intel processor. So I had to go to g++ version 10. See [Issue 1][Issue1] for the steps I took to convince my machine to work with g++ version 10 instead of 9 which seems to be the default as of now in Ubuntu 20.04.

```
sudo apt-get update
sudo apt-get install g++ cmake patchelf m4 libgmp-dev libntl-dev libpthread-stubs0-dev
```

Download and compile the library:
```
git clone https://github.com/homenc/HElib.git
cd HElib
mkdir build
cd build
cmake -DPACKAGE_BUILD=ON ..
make -j16
sudo make install
```
The `helib` library is now installed in `/usr/local/./helib_pack/`.



## Examples

### Dummy example

First check that the library is called correctly.

I create a little demo file
```
mkdir helibdemo
cd helibdemo
vi helibdemo.cpp
```
with the following content:
```
#include <helib/helib.h>
int main() {
  return 0;
}
```

In addition I create a cmake file `CMakeLists.txt`.
```
### simplified copy of the CMakeLists.txt file of the HElib examples
cmake_minimum_required(VERSION 3.10.2 FATAL_ERROR)
## Use -std=c++17 as default.
set(CMAKE_CXX_STANDARD 17)
## Disable C++ extensions
set(CMAKE_CXX_EXTENSIONS OFF)
## Require full C++ standard
set(CMAKE_CXX_STANDARD_REQUIRED ON)
project(HELibDemo VERSION 1.0)
add_executable(helibdemo helibdemo.cpp)
find_package(helib ${HELIB_VERSION} REQUIRED)
target_link_libraries(helibdemo helib)
```

Compile and run.
```
  cmake .
  make
  ./helibdemo
```

Obviously there's no output. But it's a first minimal program using HElib.

Good to go and have a look at the examples that were shipped with the library.

### Compile the provided Examples

Go to the `examples` folder and compile all examples.

```
cd ~/HElib/examples/
cmake .
make
```
Now you'll have compiled examples illustrating BGV and CKKS:
* [BGV_binary_arithmetic](#binary-arithmetic)
* [BGV_packed_arithmetic](#example-packed-arithmetic)
* [BGV_country_db_lookup](#privacy-preserving-search-example)
* 01_ckks_basics
* 02_ckks_depth
* 03_ckks_data_movement
* 04_ckks_matmul
* 05_ckks_multlowlvl
* 06_ckks_complex
* 07_ckks_serialization
* 08_ckks_deserialization


### Binary Arithmetic

This example computes `a*b+c` (one multiplication and one addition) and `a+b+c` (two additions) for encrypted values of `a`, `b`, and `c`. Note that the security level is total bogus. This example is purely for illustration purposes.

```
$ time ./bin/BGV_binary_arithmetic

*********************************************************
*            Basic Binary Arithmetic Example            *
*            ===============================            *
*                                                       *
* This is a sample program for education purposes only. *
* It attempts to demonstrate the use of the API for the *
* binary arithmetic operations that can be performed.   *
*                                                       *
*********************************************************
Initialising context object...
m = 4095, p = 2, phi(m) = 1728
  ord(p) = 12
  normBnd = 2.25463
  polyNormBnd = 22.5545
  factors = [3 5 7 13]
  generator 2341 has order (== Z_m^*) of 6
  generator 3277 has order (== Z_m^*) of 4
  generator 911 has order (== Z_m^*) of 6
r = 1
nslots = 144
hwt = 120
ctxtPrimes = [6,7,8,9,10,11,12,13,14]
specialPrimes = [15,16,17,18,19]
number of bits = 798

security level = 24.2499

Security: 24.2499
Creating secret key...
Number of slots: 144
Pre-encryption data:
a = 16234
b = 39681
c = 37096
a*b+c = 644218450
a+b+c = 93011
popcnt(a) = 10

real	1m2.352s
user	1m2.145s
sys	0m0.204s
```


### Example Packed Arithmetic

The packed arithmetic example is a nice illustration of SIMD (=Single instruction, multiple data).

```
$ time ./bin/BGV_packed_arithmetic

*********************************************************
*         Basic Mathematical Operations Example         *
*         =====================================         *
*                                                       *
* This is a sample program for education purposes only. *
* It attempts to show the various basic mathematical    *
* operations that can be performed on both ciphertexts  *
* and plaintexts.                                       *
*                                                       *
*********************************************************
Initialising context object...
m = 32109, p = 4999, phi(m) = 16560
  ord(p) = 690
  normBnd = 2.32723
  polyNormBnd = 58.2464
  factors = [3 7 11 139]
  generator 320 has order (== Z_m^*) of 6
  generator 3893 has order (== Z_m^*) of 2
  generator 14596 has order (== Z_m^*) of 2
  T = [ 1 14596 3893 21407 320 14915 25618 11023 6073 20668 9965 27479 16820 31415 10009 27523 20197 2683 24089 9494 9131 23726 2320 19834 ]
r = 1
nslots = 24
hwt = 0
ctxtPrimes = [6,7,8,9,10,11,12,13,14]
specialPrimes = [15,16,17,18,19]
number of bits = 773

security level = 62.4783

Security: 62.4783
Creating secret key...
Generating key-switching matrices...
Number of slots: 24
Initial Plaintext: {"HElibVersion":"2.1.0","content":{"scheme":"BGV","slots":[[0],[1],[2],[3],[4],[5],[6],[7],[8],[9],[10],[11],[12],[13],[14],[15],[16],[17],[18],[19],[20],[21],[22],[23]]},"serializationVersion":"0.0.1","type":"Ptxt"}
Operation: 2(a*a)/(a*a) - 2(a*a)/(a*a) = 0
Decrypted Result: {"HElibVersion":"2.1.0","content":{"scheme":"BGV","slots":[[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0]]},"serializationVersion":"0.0.1","type":"Ptxt"}
Plaintext Result: {"HElibVersion":"2.1.0","content":{"scheme":"BGV","slots":[[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0],[0]]},"serializationVersion":"0.0.1","type":"Ptxt"}
Operation: Enc{(0 + 1)*1} + (0 + 1)*1
Decrypted Result: {"HElibVersion":"2.1.0","content":{"scheme":"BGV","slots":[[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2],[2]]},"serializationVersion":"0.0.1","type":"Ptxt"}

real	0m15.698s
user	0m15.265s
sys	0m0.432s
```

### Privacy Preserving Search Example

The next examples demonstrates an encrypted search. Note that we need to point the executable to the `countries_dataset.csv` in the `BGV_country_db_lookup` directory.


```
$ time ./bin/BGV_country_db_lookup db_filename=BGV_country_db_lookup/countries_dataset.csv

*********************************************************
*           Privacy Preserving Search Example           *
*           =================================           *
*                                                       *
* This is a sample program for education purposes only. *
* It implements a very simple homomorphic encryption    *
* based db search algorithm for demonstration purposes. *
*                                                       *
*********************************************************

---Initialising HE Environment ... 
Initializing the Context ... 
Creating Secret Key ...
Creating Public Key ...

***Security Level: 0 *** Negligible for this example ***

Number of slots: 48

---Initializing the encrypted key,value pair database (47 entries)...
Converting strings to numeric representation into Ptxt objects ...
Encrypting the database...

Initialization Completed - Ready for Queries
--------------------------------------------

Please enter the name of an European Country: Austria
Looking for the Capital of Austria
This may take few minutes ... 

Query result: Vienna
  timer_TotalQuery: 12.3011 / 1 = 12.3011   [/home/christiane/github/helib-dev/HElib/myexamples/BGV_country_db_lookup/BGV_country_db_lookup.cpp:259]

real	0m20.595s
user	0m12.358s
sys	0m0.008s
```


---
Author: [Christiane Peters][cpp].


[cpp]: 		http://cbcrypto.org/

[HElib]: https://github.com/homenc/HElib/
[HElib-install]: https://github.com/homenc/HElib/blob/master/INSTALL.md
[namespaces]: https://homenc.github.io/HElib/namespacehelib.html
[helib-math]: https://homenc.github.io/HElib/documentation/Design_Document/HElib-design.pdf
[cmakeintro]: https://www.yoctopuce.com/EN/article/compiling-the-c-library-with-cmake
[Issue1]: https://github.com/christianepeters/homomorphic-encryption/issues/1
