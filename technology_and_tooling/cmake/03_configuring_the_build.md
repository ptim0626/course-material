---
name: Configuring the build
dependsOn: [technology_and_tooling.cmake.02_a_first_cmake_project]
tags: [cpp]
attribution:
  - citation: >
      "Introduction to CMake" course developed by Fergus Cooper and the Oxford Research
      Software Engineering group
    url: https://github.com/OxfordRSE/IntroCMakeCourse
    image: https://www.rse.ox.ac.uk/sites/default/files/rse/site-logo/banner_ox_rse_desktop.svg
    license: CC-BY-4.0
  - citation: This course material was developed as part of UNIVERSE-HPC, which is funded through the SPF ExCALIBUR programme under grant number EP/W035731/1
    url: https://www.universe-hpc.ac.uk
    image: https://www.universe-hpc.ac.uk/assets/images/universe-hpc.png
    license: CC-BY-4.0
---

## CMake configuration

### Choosing a generator

CMake can create more than Makefiles. It can generate IDE projects, or build descriptions for the fast Ninja tool.

```bash
build$ cmake -G Ninja ..
[...]

build$ ninja
[2/2] Linking CXX executable main_executable
```

You can build uniformly, regardless of the generator:

```bash
build$ cmake -G Ninja ..
build$ cmake --build . --target main_executable
```

This can be particularly useful in automated scripts that may be run on
different systems using different generators.

### Setting configuration

You (and users) can override choices made by CMake using the `-D` argument.

```bash
build$ cmake -DCMAKE_CXX_COMPILER=/usr/local/bin/g++-10 ..
-- Configuring done
You have changed variables that require your cache to be deleted.
Configure will be re-run and you may have to reset some variables.
The following variables have changed:
CMAKE_CXX_COMPILER= /usr/local/bin/g++-10

-- The CXX compiler identification is GNU 10.2.0
[...]
```

You can switch between Debug, Release, RelWithDebInfo and MinSizeRel, by default:

```bash
build$ cmake -DCMAKE_BUILD_TYPE=Release ..
[...]
```

The default flags with `g++` are:

```cmake
CMAKE_CXX_FLAGS_DEBUG            -g
CMAKE_CXX_FLAGS_MINSIZEREL       -Os -DNDEBUG
CMAKE_CXX_FLAGS_RELEASE          -O3 -DNDEBUG
CMAKE_CXX_FLAGS_RELWITHDEBINFO   -O2 -g -DNDEBUG
```

::::challenge{id=cmake-config title="CMake configuration"} 

Try using the Ninja generator, compiling in Release mode, and using another compiler if you have one installed.

Remember that you might have to clean your build directory when, e.g., changing generator.

::::
