---
name: A first CMake project
dependsOn: [technology_and_tooling.cmake.01_introduction]
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

## Getting Started

The course repository contains a number of checkpoints, each of which is a
step through this course. Each checkpoint contains a small C++ project along with a `CMakeLists.txt` file.

Checkpoint 0 is a simple "hello, world" program written in C++. Let's use CMake to build it.

```bash
cd checkpoint_0
```

### `CMakeLists.txt`

The `CMakeLists.txt` file is where you write the definitions of your targets and
configuration, and is processed by CMake to generate the build files. It is
written in the CMake language, which is a bit like a scripting language.

Let's look at the sample `CMakeLists.txt` line by line.

### CMake has changed a lot

Often the first line of a `CMakeLists.txt` file describes the minimum version of
CMake required, this is because CMake has changed a lot over the years, and some
features are only available in newer versions.

```cmake
cmake_minimum_required(VERSION 3.13)
```

tells CMake which version we used, affecting the features available and the
interpretation of `CMakeLists.txt`

### Define a project

Then we define a project by giving it a name and specifying the languages used.

```cmake
project(IntroCMakeCourse LANGUAGES CXX)
```

We have a project called `IntroCMakeCourse`, in the C++ language.

### Configure the compiler

Often we want to target a particular version of the C++ standard, and we can do that like so:

```cmake
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
```

We're using the [C++17](https://en.cppreference.com/w/cpp/17) language dialect.
On its own, `CMAKE_CXX_STANDARD` is only a _request_: if the compiler doesn't
support it, CMake will quietly fall back to an older standard. Setting
`CMAKE_CXX_STANDARD_REQUIRED` to `ON` turns that into a hard error instead.
Setting `CMAKE_CXX_EXTENSIONS` to `OFF` asks for standard C++ (e.g. `-std=c++17`)
rather than compiler-specific extensions (e.g. `-std=gnu++17`), which keeps the
build portable across compilers.

### Tell it what to build

Now we can tell CMake what to build. We do this with the `add_executable` command:

```cmake
add_executable(main_executable main.cpp)
```

There is a program, called `main_executable`, which depends on the source code in `main.cpp`

### Using CMake

That is our `CMakeLists.txt`, now we can use CMake to configure our project.
This is the first step in the compilation, during which CMake uses our
`CMakeLists.txt` file to generate the build files. If we are using a Linux
distribution like Ubuntu, it is likely that we will be using Make, so in this
case CMake will generate a Makefile that can build our project.

It's typical to build "out of tree", by running CMake in a separate place. This keeps
generated files out of your source folder and allows you to have multiple
builds, for instance for different compilers or build configurations.

```bash
# from checkpoint0$
mkdir build
cd build
cmake ..
```
```output
[...]
-- Build files have been written to: <...>/checkpoint_0/build
```

### Build your project

CMake only generated the build script, it didn't actually compile anything.
To compile the project, we use the build system that CMake generated, in this case Make:

```bash
# from build$
make
```

```output
[...]
[100%] Built target main_executable
```

We can then run the executable directly:

```bash
./main_executable
```

```output
Checkpoint 0
Hello, World!
```

We can also use CMake to build the project. This does the same thing as the
`make` command above, but is more portable as it works with other build systems
too.

```bash
# from build$
cmake --build .
```

```output
[...]
[100%] Built target main_executable
```

::::challenge{id=cmake-workflow title="CMake workflow"} 
Verify that you can configure, compile and run the executable in Checkpoint 0.
::::
