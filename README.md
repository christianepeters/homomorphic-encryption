# HElib

## About

I tried out [HElib][HElib], a library for homomorphic encryption.

I simplified the instructions in the HElib [INSTALL.md][HElib-install].
I don't plan to run any huge computations. It's all about trying
out the library. I worked both on a Linux machine (Ubuntu) and on
a Mac.


## HElib documentation

* Documentation of the library: https://homenc.github.io/HElib/
* The math behind the HElib: [HElib-design.pdf][helib-math]
* [HElib namespaces][namespaces] to keep track of all the function, class, and variable names in various libraries (for folks like me who forgot that there were namespaces in C++ see [here][cppns])
* Github https://github.com/homenc/HElib
* Pre-compiled examples: https://github.com/homenc/HElib/tree/master/examples


## Install the HElib library

Steps for
* [MacOS](#macos)
* [Ubuntu](#ubuntu)


### MacOS

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
git clone git@github.com:homenc/HElib.git
cd HElib
mkdir build
cd build
cmake -DPACKAGE_BUILD=ON ..
make -j4
sudo make install
```
The `helib` library is now installed in `/usr/local/./helib_pack/`.

Jump to the [next chapter](#example-packed-arithmetic) to see how to work with an example.


### Ubuntu

Check pre-reqs on Ubuntu:
* cmake
* g++
* patchelf >= 0.9
* m4 >= 1.4.16
* GMP and NTL libraries

```
sudo apt-get update
sudo apt-get install g++ cmake patchelf m4 libgmp-dev libntl-dev
```

Download and compile the library:
```
git clone git@github.com:homenc/HElib.git
cd HElib
mkdir build
cd build
cmake -DPACKAGE_BUILD=ON ..
make
sudo make install
```
When I used multiple threads for the `make` step, the machine crashed. So I decided to use just `make` rather than `make -j4`.

The `helib` library is now installed in `/usr/local/./helib_pack/`.



## Example Packed Arithmetic

Let's try out the packed arithmetic example which is a nice illustration of SIMD (=Single instruction, multiple data).

```
cd HElib/examples/BGV_packed_arithmetic
cmake .
make
```
Now let's run the example code without modification.
```
$ ./BGV_packed_arithmetic

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
Building modulus chain...
m = 32109, p = 4999, phi(m) = 16560
  ord(p) = 690
  normBnd = 2.32723
  polyNormBnd = 58.2464
  factors = [3 7 11 139]
  generator 320 has order (== Z_m^*) of 6
  generator 3893 has order (== Z_m^*) of 2
  generator 14596 has order (== Z_m^*) of 2
  T = [ 1 14596 3893 21407 320 14915 25618 11023 6073 20668 9965 27479 16820 31415 10009 27523 20197 2683 24089 9494 9131 23726 2320 19834 ]

Security: 59.3374
Creating secret key...
Generating key-switching matrices...
Number of slots: 24
Initial Plaintext: [[0], [1], [2], [3], [4], [5], [6], [7], [8], [9], [10], [11], [12], [13], [14], [15], [16], [17], [18], [19], [20], [21], [22], [23]]
Operation: 2(a*a)/(a*a) - 2(a*a)/(a*a) = 0
Decrypted Result: [[0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0]]
Plaintext Result: [[0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0], [0]]
Operation: Enc{(0 + 1)*1} + (0 + 1)*1
Decrypted Result: [[2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2], [2]]
```

:-)


## Author
Responsible for the content of the page is [Christiane Peters][cpp].





[cpp]: http://cbcrypto.org/
[HElib]: https://github.com/homenc/HElib/
[HElib-install]: https://github.com/homenc/HElib/blob/master/INSTALL.md
[namespaces]: https://homenc.github.io/HElib/namespacehelib.html
[cppns]: https://www.tutorialspoint.com/cplusplus/cpp_namespaces.htm
[helib-math]: https://homenc.github.io/HElib/documentation/Design_Document/HElib-design.pdf
[helib-pa]: https://github.com/homenc/HElib/tree/master/examples/BGV_packed_arithmetic
[cmakeintro]: https://www.yoctopuce.com/EN/article/compiling-the-c-library-with-cmake
