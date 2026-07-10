---
name: Modern CMake
id: cmake
dependsOn: [technology_and_tooling.ide.cpp]
files: [
    01_introduction.md,
    02_a_first_cmake_project.md,
    03_configuring_the_build.md,
    04_subdirectories_and_the_cmake_language.md,
    05_targets_and_libraries.md,
    06_finding_dependencies.md,
    07_more_cmake_features.md,
]
learningOutcomes:
  - Explain what CMake is and why it is used to build C++ projects.
  - Write a CMakeLists.txt to configure, build and run a C++ executable.
  - Control the build using different generators and build configurations.
  - Structure a larger project across subdirectories using the CMake language.
  - Define executable and library targets and manage their dependencies with target visibility keywords.
  - Locate and link external dependencies using find_package.
  - Use CMake language features such as includes, conditionals, loops, functions and options.
summary: |
  CMake is a widely used, cross-platform build system generator for C++ projects.
  This course introduces the basics of CMake: writing CMakeLists.txt files to
  configure and build executables and libraries, structuring larger projects across
  subdirectories, finding and linking external dependencies, and using the CMake
  language to make builds portable and reproducible.
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

CMake is a build system generator: it lets you describe how a C++ project should
be built in a portable way, and then generates the build files for a variety of
tools such as Unix Makefiles, Ninja, or Visual Studio projects. This makes it
possible for your code to be configured, compiled and run consistently across
different operating systems, compilers and machines.

This course introduces the basics of CMake. Starting from a simple "hello, world"
executable, you will learn how to write `CMakeLists.txt` files, configure and
build a project, structure larger projects across subdirectories, define library
and executable targets, find and link external dependencies, and use features of
the CMake language to make your builds portable and reproducible.
