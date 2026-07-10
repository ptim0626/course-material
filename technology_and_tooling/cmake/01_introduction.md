---
name: Introduction
dependsOn: [technology_and_tooling.ide.cpp]
tags: [cpp]
attribution:
  - citation: >
      "Modern CMake" course developed by Fergus Cooper and the Oxford Research
      Software Engineering group
    url: https://www.rse.ox.ac.uk/
    image: ./cmake/img/2024_oxrse_square.svg
    license: CC-BY-4.0
---

## Course materials

Clone the material repository and change your current working directory to the project
root:

```bash
git clone https://github.com/OxfordRSE/IntroCMakeCourse
cd IntroCMakeCourse
```

## What is CMake and why should I use it?

To compile a simple C++ project, you might compile the source files on the command line by directly invoking the compiler:

```bash
g++ -std=c++20 main.cpp functionality.cpp -o main_executable -I/usr/local/include/eigen3 -lboost_program_options
```

This is fine for small projects, but it quickly becomes unwieldy as the number
of source files and dependencies increases. Moreover, it is not portable: the
above command will only work on systems with the Boost and Eigen libraries
installed in the specified locations. We might also have to change the compiler,
and would have to change the command line invocation.

Another option is to use a build tool like [Make](https://www.gnu.org/software/make/), which allows us to specify
the build process by defining rules. This can scale to larger projects, but
still is not completely portable as the rules are specific to Make, and we still
have to manually specify the compiler and libraries.

CMake is a build system *generator* that allows us to specify the build process for large project, in
a portable way, and to generate build files for a variety of build tools like
Unix Makefiles, Visual Studio projects, or the [Ninja build tool](https://ninja-build.org/). It has a lot
of features for finding dependencies and for making the build process more portable across
operating systems, compilers, and architectures. It has many features, but we
will only cover the basics here.

## Enter CMake

In [CMake](https://cmake.org/) you describe *targets* (what to build), *inputs* (the sources files),
and *configuration* (what libraries to use, what compiler settings, etc.).

CMake uses that with its own *rules* (how to turn sources into programs) to
generate makefiles, IDE projects, or other outputs. CMake doesn't build your
project itself.

CMake works on Linux, Windows, macOS and more.
