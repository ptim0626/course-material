---
name: Targets and libraries
dependsOn: [technology_and_tooling.cmake.04_subdirectories_and_the_cmake_language]
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

## Targets

Targets are the basic building blocks of CMake. In checkpoint 1, we have a
single target, the executable `main_executable`. You could also have a
library target, or a test target, or a custom target that runs a script.

### Target properties

CMake allows for a very fine-grained control of target builds, through
_properties_.

For example, the property `INCLUDE_DIRECTORIES` specifies the list of
directories to be specified with the compiler switch `-I` (or `/I`).

Properties can be set manually like variables, but in general CMake provides
commands for it:

```cmake
target_include_directories(main_executable
                            PUBLIC
                            ${CMAKE_CURRENT_SOURCE_DIR}
)
```

_Properties are different from variables!_

### Creating a library target

Creating a library target is similar to an executable target, but uses the
`add_library` command:

```cmake
add_library(my_lib STATIC ${source_files})
```

Use `SHARED` instead of `STATIC` to build a shared library: or,
if omitted, CMake will pick a default based on the value of the variable
`BUILD_SHARED_LIBS`.

### Linking libraries (`PRIVATE`)

Library dependencies can be declared using the `target_link_libraries()` command:

```cmake
target_link_libraries(another_target PRIVATE my_lib)
```

The `PRIVATE` keyword states that `another_target` uses `my_lib` only in its internal
implementation. Programs using `another_target` don't need to know about `my_lib`.

### Linking libraries (`PUBLIC`)

Picture another dependency scenario:

-   `another_target` uses `my_lib` in its internal implementation.
-   **and** `another_target` defines some function that take parameters of a type defined
    in `my_lib`.

Programs using `another_target` also must link against `my_lib`:

```cmake
target_link_libraries(another_target PUBLIC my_lib)
```

### Link libraries (`INTERFACE`)

Picture another dependency scenario:

- `another_target` only uses `my_lib` in its interface.
- **but not** in its internal implementation.

```cmake
target_link_libraries(another_target INTERFACE my_lib)
```

### Behaviour of target properties across dependencies

Target properties are paired with another property
`INTERFACE_<PROPERTY>`. For instance

```cmake
INTERFACE_INCLUDE_DIRECTORIES
```

These properties are inherited by depending targets (such as
executables and other libraries).

Example:

```cmake
target_include_directories(my_lib INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
```

-   `PRIVATE`: sets `INCLUDE_DIRECTORIES`.
-   `INTERFACE`: sets `INTERFACE_INCLUDE_DIRECTORIES`.
-   `PUBLIC`: sets both.


::::challenge{id=make-a-library title="Make a library"} 

Let's separate the functionality from the executable itself:

```text
CMakeLists.txt
src/
    <library>
exe/
    <executable>
```

Tasks:

1. Modify `src/CMakeLists.txt` so that a static library is created out
   of `functionality.cpp` and `functionality.hpp`.
2. Move `main.cpp` into a new directory `exe`, and add a `CMakeLists.txt` defining a new
   target that links against the library.
3. Modify the top-level `CMakeLists.txt` so that it processes both directories.

:::solution

Have a look at Checkpoint 2 for a solution.

:::

::::

### Print information with `message()`

You often need to print information during the configuration step. This can be
done with the `message()` command:

```cmake
set(name "Jane Doe")
message(STATUS "Hello ${name}")
```

```text
-- The C compiler identification is GNU 8.3.0
...
-- Hello Jane Doe
-- Configuring done
-- Generating done
```

### Options for `message()`

```cmake
message(STATUS "A simple message")
```

`STATUS` can be replaced by *e.g.* `WARNING`, `SEND_ERROR`, `FATAL_ERROR`
depending on the situation.

```cmake
message(SEND_ERROR "An error occurred but configure step continues")
```

```text
CMake Error at CMakeLists.txt:2 (message):
    An error occurred but configure step continues

-- Configuring incomplete, errors occurred!
```
