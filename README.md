Wrapper for MOSEK 10.1.26
=======================

<https://www.mosek.com>


This is a fork from [here](https://github.com/RobotLocomotion/mosek) which is basically for the older version of mosek.
I haven't tested it for all operating systems and modifications might be needed for your project. 

Requires a free academic license or paid commercial license, as appropriate.
Install the license file to `$HOME/mosek/mosek.lic`.

Versions other than 10.1.26 (and non-Linux) are unsupported by this wrapper.

```
$ mkdir build && cd build
$ cmake ..
$ make install
```

## Adding as external project to your CMakeLists.txt
```
include(ExternalProject)
ExternalProject_Add(mosek
        SOURCE_DIR "${CMAKE_CURRENT_BINARY_DIR}/mosek-src"
        GIT_REPOSITORY https://github.com/carlosiglezb/mosek-pod.git
        GIT_TAG 2d63b36c3d0c55a90d91f9a550ea0c14323c1bd9
        CMAKE_ARGS
        -DBUILD_TESTING=OFF
        -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
        "-DCMAKE_C_COMPILER_LAUNCHER=${CMAKE_C_COMPILER_LAUNCHER}"
        "-DCMAKE_C_COMPILER=${CMAKE_C_COMPILER}"
        "-DCMAKE_C_FLAGS=${CMAKE_C_FLAGS}"
        "-DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}"
        BINARY_DIR "${CMAKE_CURRENT_BINARY_DIR}/mosek-build")
```

