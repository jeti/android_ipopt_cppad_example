This is a simple example app demonstrating how to use CppAD for differentiation within IPOPT on Android. This project is 99.9% based on the repo

- https://github.com/jeti/android_ipopt_example

with some minor modifications. More details on setting up IPOPT on Android can be found there. 

The specific changes we had to make were:

1. We downloaded the CppAD headers and put those into our `include` folder. Specifically, in the barebones IPOPT example, we had a directory structure that looked like this 

   ```
   project/
   ---app/
   ------build.gradle
   ------CMakeLists.txt
   ---------libs/
   ------------arm64-v8a/      
   ------------include/
   ---------------coin/
   ---------src/
   ------------main/
   ------------cpp/   
   ```
   After copying in the CppAD headers, that now looks like 

   ```
   project/
   ---app/
   ------build.gradle
   ------CMakeLists.txt
   ---------libs/
   ------------arm64-v8a/      
   ------------include/
   ---------------coin/
   ---------------cppad/
   ---------src/
   ------------main/
   ------------cpp/   
   ```

2. To reflect the additional headers, we modified our `CMakeLists.txt` file to reflect the new headers. Specifically, we change 

   ```
   include_directories(${CMAKE_SOURCE_DIR}/libs/include/coin
                       ${CMAKE_SOURCE_DIR}/libs/include/coin/ThirdParty)
   ```

   to 

   ```
   include_directories(${CMAKE_SOURCE_DIR}/libs/include
                       ${CMAKE_SOURCE_DIR}/libs/include/coin
                       ${CMAKE_SOURCE_DIR}/libs/include/coin/ThirdParty)
   ```

3. Finally, we just need to replace the contents of `project/app/src/cpp` with our new test file `cppad_example.cpp`, and update the `CMakeLists.txt` from

   ```
   add_library( # Sets the name of the library.
                native-lib

                # Sets the library as a shared library.
                SHARED

                # Provides a relative path to your source file(s).
                src/main/cpp/cpp_example.cpp
                src/main/cpp/MyNLP.cpp)
   ```

   to 

   ```
   add_library( # Sets the name of the library.
                native-lib

                # Sets the library as a shared library.
                SHARED

                # Provides a relative path to your source file(s).
                src/main/cpp/cppad_example.cpp)
   ```

