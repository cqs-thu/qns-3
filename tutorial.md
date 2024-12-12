This repository is a module of [ns-3](https://www.nsnam.org), and uses [ExaTN](https://github.com/ORNL-QCI/exatn) as the basic engine. Therefore, before using this repository, you need to first build an executable ns-3 and ExaTN package. 

## Prerequisites

Our module requires no more than `ns-3` and `ExaTN`'s requirements. And the main features require the following:

1. C and C++ compiler (version: gcc 11 and g++ 11), Python (version 3.6 or above), CMake and a build-system. (However, the latest Python 3.11 may cause a problem (invalid use of incomplete type ‘PyFrameObject’) when building `ExaTN`. So you may choose a version lower than 3.11.)

2. Basic Linear Algebra Subprograms (BLAS) library. We recommend and use [OpenBLAS](https://github.com/xianyi/OpenBLAS).

3. Message Passing Interface (MPI) library. We recommend and use [OpenMPI](https://www.open-mpi.org/).

## Building ns-3

When building and running `ns-3`, we recommand you follow the [official tutorial of ns-3](https://www.nsnam.org/docs/release/3.39/tutorial/html/). Here is an installation example. 
(You can also use the git method to install `ns-3`, or use other versions of `ns-3` due to your interest.)

1. Download the [latest release](https://www.nsnam.org/releases/latest).

2. Unpack it in a working directory of your choice.

   ```bash
   $ tar xjf ns-allinone-3.39.tar.bz2
   ```

3. Change into the `ns-3` directory directly; e.g.

   ```bash
   $ cd ns-allinone-3.39/ns-3.39
   ```

4. Building and testing `ns-3`.

   ```bash
   $ ./ns3 configure --enable-examples --enable-tests
   ```

5. use `ns3` command to build `ns-3` and run the unit tests to check your build.

   ```bash
   $./ns3 build
   $ ./test.py
   ```


## Building ExaTN

When building and running `ExaTN`, we recommand you follow the [official tutorial of ExaTN](https://github.com/ORNL-QCI/exatn) to download the package and submodules, and use our method to install it.

1. Download `ExaTN` using git.

   ```bash
   $ git clone --recursive https://github.com/ornl-qci/exatn.git
   ```

2. Change into the `ExaTN` directory.

   ``` bash
   $ cd exatn
   ```

3. Download the submodules.

   ```bash
   $ git submodule init
   $ git submodule update --init --recursive
   ```

4. Make a new directory for building.

   ```bash
   $ mkdir build && cd build
   ```

5. Use the following method to install `ExaTN`.

   ```bash
   $ CC=gcc-11 CXX=g++-11 FC=gfortran-11 cmake .. -DCMAKE_BUILD_TYPE=Release -DMPI_LIB=OPENMPI -DMPI_ROOT_DIR=/usr/lib/x86_64-linux-gnu/openmpi/ -DBLAS_LIB=OPENBLAS -DBLAS_PATH=/usr/lib/x86_64-linux-gnu/openblas-pthread/
   $ make -j install
   ```

If your installation succeeds, then you will find a `\.exatn` directory in your user folder (`~/.exatn`). This directory contains all codes of `ExaTN` that we need to further use.

## Adding qns-3 to ns-3

Because our repository is a module of [ns-3](https://www.nsnam.org), the method to adding it to ns-3 completely follows [the standard of ns-3](https://www.nsnam.org/docs/manual/html/new-modules.html). Therefore, you can simply move the folder qns-3 to `/contrib`. Make sure that you're in your ns-3 folder, and run the following:

```bash
$ cd contrib
$ git clone https://github.com/qns-3/qns-3.git quantum
```

And edit the 13th line of quantum module's CMake file `/contrib/quantum/CMakeLists.txt`, making the module able to find the `ExaTN` codes:

```bash
set(ExaTN_DIR ~/.exatn) # change it to your exatn installation path
```

Then you can rebuild `ns-3` and run the examples. We take the `telep-app-example` as an example:

```bash
$ ./ns3 configure --enable-example
$ ./ns3 run telep-app-example
```

You can also run the examples with NS_LOG to see the information outputs by running the following:

```bash
$ NS_LOG="QuantumNetworkSimulator=info:QuantumPhyEntity=info|logic" ./ns3 run telep-app-example
```

## Adding new examples

You can write new codes in `/contrib/quantum/examples`. If you want to run a new example, please follow the tutorial of `ns-3` by editing to `/ns-3-dev/contrib/quantum/examples/CMakeLists.txt` with this form:

```cmake
build_lib_example(
    NAME YOUR_EXAMPLE
    SOURCE_FILES YOUR_EXAMPLE.cc
    LIBRARIES_TO_LINK ${libquantum}
)
```

Then you can rebuild `ns-3` and run the example:

```bash
$ ./ns3 configure --enable-example
$ ./ns3 run YOUR_EXAMPLE
```

Similarly, you can also run the examples with NS_LOG to see the information outputs by running the following:

```bash
$ NS_LOG="QuantumNetworkSimulator=info:QuantumPhyEntity=info|logic" ./ns3 run YOUR_EXAMPLE
```

## Possible Problems

When working with different versions of ns-3, the methods for adding modules and linking libraries may vary. Our `CMakeLists.txt` file and this tutorial have been tested and are confirmed to work well with versions 3.39 to 3.41.  However, for newer versions, ns-3 might have updates and different requirements.

If you encounter any issues, please verify your ns-3 version or consult the [official ns-3 tutorial](https://www.nsnam.org/documentation) for the proper procedures to add new modules.

