#Hello world program in CPP
1. Create a folder 'mycpp'
2. gedit hello.cpp
3. Type the following and save
```
#include <iostream>
using namespace std;
int main() {
    cout << "Hello, World!" << endl;
    return 0;
}
```
Executing C++ files
```
g++ hello.cpp
./a.out
```

Giving a name to executable

```
g++ hello.cpp -o hello
./hello
```
#Executing cpp using cmake

In the same folder 'mycpp' create a file called 'CMakeLists.txt' and add following

```
cmake_minimum_required(VERSION 3.0)

project(mycpp)

add_executable(hello main.cpp)
```
This will make an executable hello
Now follow the commands

```
mkdir build
cd build
cmake ..
make
```
after compilation, run
```
./hello
```

