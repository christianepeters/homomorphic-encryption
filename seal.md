# SEAL

## About

I tried out [SEAL][SEAL], a library for homomorphic encryption published by Microsoft.

The SEAL library is written in C++.

## SEAL documentation

* GitHub: https://github.com/Microsoft/SEAL


## Download and compile the library

I'm following the instructions for MacOS and LInux in the SEAL [README.md][SEALRM].

### MacOS and Ubuntu

Prerequisites are `cmake` and a new compiler, a modern version of GNU G++ (>= 6.0) or Clang++ (>= 5.0) is needed. In macOS the Xcode toolchain (>= 9.3) will work. Microsoft SEAL compiled with Clang++ has much better runtime performance than that compiled with GNU G++.


* Install helper tools on MacOS:
  * Xcode Command Line Tools
  ```
  xcode-select --install
  ```
  * cmake >= 3.17.3  
  ```
  brew install cmake
  ```
  * Apple clang
  ```
  $ clang -v
  Apple clang version 12.0.0 (clang-1200.0.32.2)
  Target: x86_64-apple-darwin19.6.0
  Thread model: posix
  InstalledDir: /Library/Developer/CommandLineTools/usr/bin
  ```

* Install helper tools on Ubuntu:
  ```
  sudo apt-get update
  sudo apt-get install g++ cmake
  ```

* Clone repo
  ```
  git clone git@github.com:microsoft/SEAL.git
  ```

* Compile the library including examples (on either MacOS or Ubuntu):
    ```
    cd SEAL/
    cmake . -DSEAL_BUILD_EXAMPLES=ON
    ```
* Compile and install the library system wide (requires root access).
  ```
  make -j
  sudo make install
  ```

The `sealexamples` executable is located in `native/bin/`.

## Example

* Write your first program with SEAL following [these instructions][SEALvideo].
* Create a test repo and a file called `sealdemo.cpp`.
  ```
  mkdir SEALdemo
  vi sealdemo.cpp
  ```
  with the following content:
  ```
  #include "seal/seal.h"
  #include <iostream>

  using namespace std;
  using namespace seal;

  int main()
  {
          EncryptionParameters parms(scheme_type::BFV);
          return 0;

  }
  ```
* Create a cmake file `CMakeLists.txt`.
  ```
  cmake_minimum_required(VERSION 3.12)
  cmake_minimum_required(VERSION 3.10)
  project(SEALDemo VERSION 1.0)
  add_executable(sealdemo sealdemo.cpp)

  find_package(SEAL)
  target_link_libraries(sealdemo SEAL::seal)
  ```
* Compile and run.
  ```
  cmake .
  make
  ./sealdemo
  ```
  Obviously there's no output. But it's a first minimal program using the SEAL library.

## Author
Responsible for the content of the page is [Christiane Peters][cpp].





[cpp]: http://cbcrypto.org/
[SEAL]: https://www.microsoft.com/en-us/research/project/microsoft-seal/
[SEALLinuxMacOS]: https://www.microsoft.com/en-us/research/video/installing-microsoft-seal-on-linux-macos/
[SEALRM]: https://github.com/microsoft/SEAL/blob/master/README.md
[SEALvideo]: https://www.youtube.com/watch?v=7vJJMU2gMn4&feature=youtu.be&ab_channel=MicrosoftResearch
