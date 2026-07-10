---
name: Finding dependencies
dependsOn: [technology_and_tooling.cmake.05_targets_and_libraries]
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

## Finding dependencies

Libraries can be installed in various locations on your system. CMake makes it
easy to link against libraries **without having to know where they are
installed**:

```cmake
find_package(library_name CONFIG REQUIRED)
```

The above defines a new target (usually named `library_name`) that can now be linked
against other targets using `target_link_libraries`.

### "config" mode for `find_package`

```cmake
find_package(library_name CONFIG REQUIRED)
```

In "config mode", `find_package` will search for a
`<PackageName>Config.cmake` file.

This file specifies all the information CMake needs (particularly where
the library is installed).

This is usually given by the library vendor.

::::challenge{id=adding-eigen-dep title="Adding the Eigen dependency"} 

Look at Checkpoint 3. A new file `src/functionality_eigen.cpp` depends on the
[Eigen](https://libeigen.gitlab.io/) library for linear
algebra.

Task: Using `find_package`, modify the `CMakeLists.txt` in directory `src/` to
link target `cmake_course_lib` against Eigen.

_Hint: Useful instructions can be found at [Using Eigen in CMake Projects](https://libeigen.gitlab.io/eigen/docs-5.0/TopicCMakeGuide.html)._

Note that keyword `NO_MODULE` is equivalent to `CONFIG`.

:::solution

Your `src/CMakeLists.txt` should look like this:

```cmake
find_package(Eigen3 CONFIG REQUIRED)
message(STATUS "Found Eigen3 ${Eigen3_VERSION}")

set(
        cmake_course_source_files
        functionality.cpp
        functionality_eigen.cpp
)

set(
        cmake_course_header_files
        functionality.hpp
        functionality_eigen.hpp
)

add_library(cmake_course_lib STATIC ${cmake_course_source_files} ${cmake_course_header_files})
target_include_directories(cmake_course_lib INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(cmake_course_lib PRIVATE Eigen3::Eigen)
```

:::
::::

### "module" mode for `find_package`

Libraries don't always come with a CMake config file
`<PackageName>Config.cmake`.

CMake can also find the library based on a file `Find<PackageName>.cmake`.
This behaviour corresponds to using `find_package` with the keyword `MODULE`:

```cmake
find_package(library_name MODULE REQUIRED)
```

`Find<PackageName>.cmake` files are cmake language scripts that try to find the
library on the system, they are often specific to a particular library and look
in specific locations that the library might be found in different operating
systems (e.g. `/usr/lib`, `/usr/local/lib`).

Such *module files* are provided by CMake itself for common libraries,
they can also be written for a particular use case if required.

### Package components

Often libraries are split into different components. E.g. Boost: filesystem,
thread, date-time, program-options, numpy...

Most programs only rely on a subset of components, and so it is useful to be
able to find and link against only the components that are needed using `find_package`.

```cmake
set(boost_components filesystem chrono)
find_package(Boost CONFIG REQUIRED COMPONENTS ${boost_components})
```

The CMake target for a component is `<PackageName>::<ComponentName>`
(*e.g.* `Boost::filesystem`).

:::callout
Boost used to be the classic example of a library found in _module_ mode, but
modern Boost (1.70 and later) ships its own `BoostConfig.cmake`, so we use
_config_ mode (`CONFIG`) here. In fact, CMake's bundled `FindBoost` module was
deprecated in CMake 3.30 (policy `CMP0167`), so `CONFIG` is now the recommended
way to find Boost. _Module_ mode is still important for the many libraries that
only ship a `Find<PackageName>.cmake` module.
:::


::::challenge{id=adding-boost-dep title="Adding the Boost dependency"} 

Look at Checkpoint 4. The executable `exe/main.cpp` depends on the [Boost Program Options](https://www.boost.org/doc/libs/1_74_0/doc/html/program_options.html)
library for handling command line arguments.

Task: Using `find_package` in `CONFIG` mode, modify the `CMakeLists.txt` in directory `exe/` to
find and link target `main_executable` against `Boost::program_options`.

:::solution
Your `exe/CMakeLists.txt` should look like this:

```cmake
find_package(Boost CONFIG REQUIRED COMPONENTS program_options)
message(STATUS "Found Boost ${Boost_VERSION}")

add_executable(main_executable main.cpp)
target_link_libraries(main_executable PRIVATE cmake_course_lib)
target_link_libraries(main_executable PRIVATE project_warnings)
target_link_libraries(main_executable PRIVATE Boost::program_options)
```
:::
::::
