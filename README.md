# build_framework
Build Framework for C/C++ Applications

# Dependencies
 - gcc/g++
 - CMake Version 3.12 or greater

# Structure
 - **src**: Should contain all of your source code
   - These should **NOT** contain *main* functions
 - **include**: Should contain all of your header files
 - **tests**: Should contain all of the files that should be turned into executables (in theory, these are tests for your source code)
   - These **SHOULD** contain *main* functions

# Running with Makefile
From the root directory, 
 - `make <test>` compiles a given test from the `tests` directory
 - `make` compiles all of the tests in the `tests` directory
 - The default language is set to C++, but can be switched to C if the command is run with `LANGUAGE=C`
   - C++ files should have `.cpp` extensions, and C files should have `.c` extensions - no other files will be interpreted
 - All outputs are generated in a `build` directory

# Running with CMake (only C++ - haven't gotten dynamic language support yet)
First, edit `CMakeLists.txt` to reference your desired source and test files
 - Specify their names under `SRC_FILES` and `TEST_FILES`, like so:
```
set(SRC_FILES
  source1.cc
  source2.cc
)

set(TEST_FILES
  test1.cc
  test2.cc
)
```

Then, to setup the CMake build directory, from the root, run
```
rm -rf build
mkdir build
cd build
cmake ..
```
From here, you have a variety of options to run tests:
 - `make <test>` will compile the given test from the `tests` directory
 - `make all_tests` will compile all of your tests and run them automatically, outputting the results to the terminal
