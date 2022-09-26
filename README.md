# build_framework
Build Framework for C/C++ Applications

# Structure
 - **src**: Should contain all of your source code
   - These should **NOT** contain *main* functions
 - **include**: Should contain all of your header files
 - **tests**: Should contain all of the files that should be turned into executables (in theory, these are tests for your source code)
   - These **SHOULD** contain *main* functions

# Running
From the root directory, 
 - `make <test>` compiles a given test from the `tests` directory
 - `make` compiles all of the tests in the `tests` directory
 - The default language is set to C++, but can be switched to C if the command is run with `LANGUAGE=C`
   - C++ files should have `.cpp` extensions, and C files should have `.c` extensions - no other files will be interpreted

All outputs are generated in a `build` directory
