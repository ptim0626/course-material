---
name: Subdirectories and the CMake language
dependsOn: [technology_and_tooling.cmake.03_configuring_the_build]
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

## Checkpoint 1

Now look at the next checkpoint in the `checkpoint_1` subdirectory, which looks like this:

```bash
CMakeLists.txt/
src/
    CMakeLists.txt
    functionality.cpp
    functionality.hpp
    main.cpp
```

Our project has grown! In addition to the code in `main.cpp`, some new
functionality was added to new source files `functionality.cpp` and
`functionality.hpp`.

This code is now contained in a specific directory `src/`, inside the project
directory.

### Compartmentalising build logic

In the top-level `CMakeLists.txt`, we use the `add_subdirectory` cmake function
to add the `src` directory to the project:

```cmake
add_subdirectory(src)
```

This causes CMake to processes the `CMakeLists.txt` file in the directory `src`.


```bash
# src/CMakeLists.txt
set(src_source_files file1 file2 file3)
add_executable(executable ${src_source_files})
```

Variables defined in the upper scope are available in the lower scope, but not
the other way around.

Using subdirectories enables clear structure and modularity, and keeps the
top-level `CMakeLists.txt` clean and tidy.

### Programming CMake

Lets have a look at some of the features of the CMake language.

Like many languages you can create variables, which you can do by simply
assigning a value via the `set` function:

```cmake
set( my_var "Hello, World!" )
```

This creates a variable called `my_var` and assigns it the string "Hello, World!".

Variables can hold lists:

```cmake
set( src_files main.cpp functionality.cpp functionality.hpp )
```

Variables can be dereferenced using `${}`:

```cmake
set(another_list ${src_files})
```

Here the value of `another_list` is set to the value of `src_files`.

You can even store a variable name or part of a variable name in another variable. For example:

```cmake
set(var files) # var = "files"
set(yet_another_list ${src_${var}})
```

::::challenge{id=add-function title="Adding new files to the project"} 

Look through the files in Checkpoint 1.

Add a new pair of hpp/cpp files that defines a new function.

- Call it from the main executable
- Add the files to `src/CMakeLists.txt`
- Configure, compile and run: check that your new function has been executed

:::solution

`src/CMakeLists.txt` should look like this:

```cmake
set(
        src_source_files
        functionality.cpp
        new_function.cpp
        main.cpp
)

set(
        src_header_files
        functionality.hpp
        new_function.hpp
)

add_executable(main_executable ${src_source_files} ${src_header_files})
```

`src/new_function.cpp` should look like this:

```cpp
#include "new_function.hpp"
#include <iostream>

void new_function()
{
    std::cout << "New function called" << std::endl;
}
```

`src/new_function.hpp` should look like this:

```cpp
#ifndef NEW_FUNCTION_HPP
#define NEW_FUNCTION_HPP

void new_function();

#endif
```

`src/main.cpp` should look like this:

```cpp
#include <iostream>
#include "new_function.hpp"
#include "functionality.hpp"

int main() {
    std::cout << "Checkpoint 2" << std::endl;
    std::cout << "Hypergeometric probability: " << hypergeometricPmf(10'000, 4'270, 300, 128) << std::endl;
    new_function();
    return 0;
}
```
:::

::::
