# HEXL

## About

I tried out [HEXL][HEXL], a library for homomorphic encryption published by Intel. The idea is to install it on a laptop and run a simple example.

The HEXL library is written in C++.

## HEXL documentation

GitHub: https://github.com/intel/hexl

## Download and compile the library

### Ubuntu

Download:
```
git clone https://github.com/intel/hexl
cd hexl
```

Try to compile:

```
cmake -S . -B build
```
fails

```
cmake -S . -B build -DHEXL_DEBUG=ON
```
fails
```
cmake -S . -B build -DHEXL_EXPORT=ON # this is what I need for the examples
```
fails

Common error
```
Performing C SOURCE FILE Test CMAKE_HAVE_LIBC_PTHREAD failed with the following output:
Change Dir: /home/christiane/github/intel-hexl/hexl-test/build/CMakeFiles/CMakeTmp

Run Build Command(s):/usr/bin/make cmTC_52557/fast && /usr/bin/make -f CMakeFiles/cmTC_52557.dir/build.make CMakeFiles/cmTC_52557.dir/build
make[1]: Entering directory '/home/christiane/github/intel-hexl/hexl-test/build/CMakeFiles/CMakeTmp'
Building C object CMakeFiles/cmTC_52557.dir/src.c.o
/usr/bin/cc   -DCMAKE_HAVE_LIBC_PTHREAD -fPIE   -o CMakeFiles/cmTC_52557.dir/src.c.o   -c /home/christiane/github/intel-hexl/hexl-test/build/CMakeFiles/CMakeTmp/src.c
Linking C executable cmTC_52557
/usr/bin/cmake -E cmake_link_script CMakeFiles/cmTC_52557.dir/link.txt --verbose=1
/usr/bin/cc  -DCMAKE_HAVE_LIBC_PTHREAD    CMakeFiles/cmTC_52557.dir/src.c.o  -o cmTC_52557
/usr/bin/ld: CMakeFiles/cmTC_52557.dir/src.c.o: in function `main':
src.c:(.text+0x46): undefined reference to `pthread_create'
/usr/bin/ld: src.c:(.text+0x52): undefined reference to `pthread_detach'
/usr/bin/ld: src.c:(.text+0x63): undefined reference to `pthread_join'
collect2: error: ld returned 1 exit status
make[1]: *** [CMakeFiles/cmTC_52557.dir/build.make:87: cmTC_52557] Error 1
make[1]: Leaving directory '/home/christiane/github/intel-hexl/hexl-test/build/CMakeFiles/CMakeTmp'
make: *** [Makefile:121: cmTC_52557/fast] Error 2
```

That one goes to my [issue list][issues], I guess.


## Example

Research paper with example code snippets: 

* [[arXiv:2103.16400][arXivHEXL]] Fabian Boemer, Sejun Kim, Gelila Seifu, Fillipe D. M. de Souza, Vinodh Gopal.**Intel HEXL: Accelerating Homomorphic Encryption with Intel AVX512-IFMA52**.

--

Responsible for the content of the page is [Christiane Peters][cpp].



[cpp]: http://cbcrypto.org/
[HEXL]: https://github.com/intel/hexl
[arXivHEXL]: https://arxiv.org/abs/2103.16400
[issues]: https://github.com/christianepeters/homomorphic-encryption/issues
