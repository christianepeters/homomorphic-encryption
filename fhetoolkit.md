# IBM FHE toolkits on Linux and Mac

## About

I tried out the [Fully Homomorphic Encryption (FHE) toolkit][FHELinux] on MacOS and Ubuntu.

* [Installation of the toolkit](#installation-of-the-toolkit)
* [Working with the toolkit](#working-with-the-toolkit)

Last version I tried: **2.1.0.3 Public Release**
Installation date: **30-January-2022** => build fails and preconfig image examples don't work

## Installation of the toolkit

Make sure you have [Docker](https://www.docker.com/) installed. On MacOS you can use [Docker for Desktop][Docker]. For instructions to install Docker on Ubuntu 20.04 see [here][mydocker].

I'm using the command line to download and install.
```
git clone https://github.com/ibm/fhe-toolkit-linux
```

The quick and dirty way is to start with the [preconfigured Docker images](#build-toolkit-from-preconfigured-image). Those are not up always up to date. 

Better use the latest helib version inside the toolkit and build the Docker image from scratch. 

Official instructions: 
* [Advanced Getting Started][AGS]
* [Getting Started][GS]
 

### Build toolkit

Build image on Ubuntu:
```
./BuildDockerImage.sh ubuntu
```
**
FAILED (tested on 30-Jan-2022) => GO TO THE NEXT SECTION AND SEE IF THE PRE-CONFIGURED IMAGES WORK.**

Check that you have the toolkit image is present:
```
$ docker images
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
```
Typically, on Ubuntu you will experience issues with missing writing permissions. The following workaround will fix this:

#### Permission Issue Workaround

As noted in [Issue \#28][Issue28], there is a permissions issue which does not allow to write from the toolkit to the  `FHE-Toolkit-Workspace` directory on Linux (note on Mac I haven't had that problem).

First change permissions of the working folder. 
```
chmod 777 FHE-Toolkit-Workspace
```

Then make this fix persistent and edit the `PersistData.sh` file, at the end of the file, line 260 or beyond, add a change permission command to the directory such as this...
```
chmod -R 777 $PERSISTENT_FHE_WORKSPACE_PATH
```
Execute the script to persist a local toolkit:
```
./PersistData.sh local/fhe-toolkit-ubuntu
```
To persist a fetched toolkit build:
```
./PersistData.sh ibmcom/fhe-toolkit-ubuntu
```

Now it should be possible to retain writing permissions inside the toolkit. Start the toolkit.


#### Start the locally built toolkit

Start the toolkit from the IBM preconfigured image.
```
./RunToolkit.sh -l ubuntu
```

You should see the message
```
FHE Development is open for business: https://127.0.0.1:8443/
```

In order to stop the container, you can run.
```
./StopToolkit.sh
```

Next: go to the [examples](#working-with-the-toolkit).


### Build toolkit from preconfigured image

Get the image from the github repo:
```
./FetchDockerImage.sh ubuntu
```
Check that you have the toolkit image is present:
```
$ docker images
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
ibmcom/fhe-toolkit-ubuntu-amd64   latest    fc8b02460f0a   7 months ago    2.64GB
ibmcom/fhe-toolkit-ubuntu         latest    fc8b02460f0a   7 months ago    2.64GB
```

If you are on Ubuntu you may experience issues with missing writing permissions. See here for a [workaround](#permission-issue-workaround).

Start the toolkit from the IBM preconfigured image.
```
./RunToolkit.sh -p ubuntu
```

You should see the message
```
FHE Development is open for business: https://127.0.0.1:8443/
```

In order to stop the container, you can run.
```
./StopToolkit.sh
```

Next: go to the [examples](#working-with-the-toolkit).

Update (30-Jan-2022): build works, examples fail.


## Working with the toolkit

Open the browser link https://127.0.0.1:8443/ and accept the certificate warning.

To make sure that I have write permissions, I switch to the terminal, type `bash` into the terminal window on the bottom. Type `touch a` which will make a file called `a` appear in my local directory. If you encounter writing permission issues, stop the toolkit using `./StopToolkit.sh`, and [execute the steps here](#permission-issue-workaround) before relaunching the container.


During the *Welcome* the IDE asks for which compiler to choose. This choice can be adapted when clicking on the compiler on the bottom blue bar. I chose GCC 9.3.0.


### Encrypted Search example

Open the `BGV_world_country_db_lookup` example in the terminal (right click on the folder and 'Open in Integrated Terminal').

```
fhe@69e45d6791c3:/opt/IBM/FHE-Workspace/myexamples/BGV_world_country_db_lookup$ ls
BGV_world_country_db_lookup.cpp  CMakeLists.txt  README.md  countries_dataset.csv  runtest.sh
```
Compile and run the example:
```
cmake .
make
```

Run the example and check the time:
```
$ time ./BGV_world_country_db_lookup 

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
HElib BGV
m=128 r=1 L=1000 c=2
SecurityLevel=0
Slots=32

***Security Level: 0 *** Negligible for this example ***

Number of slots: 32

---Initializing the encrypted key,value pair database (226 entries)...
Converting strings to numeric representation into Ptxt objects ...

Initialization Completed - Ready for Queries
--------------------------------------------

Please enter the name of a Country: Germany
Looking for the Capital of Germany
This may take a few minutes ... 

Query result:  Berlin
  timer_TotalQuery: 3.05808 / 1 = 3.05808   [/opt/IBM/FHE-Workspace/myexamples/BGV_world_country_db_lookup/BGV_world_country_db_lookup.cpp:229]

real    0m5.629s
user    0m3.129s
```


## Useful commands

* Toolkit terminal: type `bash` to get a bash-like terminal.

* Clean up folders with compiled files:
```
rm -rf CMakeCache.txt CMakeFiles cmake_install.cmake Makefile helib.log rm bin/*
```

---
Author: [Christiane Peters][cpp].


[cpp]: 		http://cbcrypto.org/
[FHELinux]: https://github.com/ibm/fhe-toolkit-linux
[AGS]: 		https://github.com/IBM/fhe-toolkit-linux/blob/master/GettingStarted.Advanced.md
[GS]: https://github.com/IBM/fhe-toolkit-linux/blob/master/GettingStarted.md
[Issue28]: 	https://github.com/IBM/fhe-toolkit-linux/issues/28#issuecomment-690673882
[Docker]:	https://www.docker.com/products/docker-desktop
[mydocker]: https://github.com/christianepeters/howto/blob/master/docker.md
