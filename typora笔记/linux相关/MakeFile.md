# 案例1

## 程序 准备

 main.cpp

```cpp
#include<iostream>
#include"functions.h"
using namespace std;

int main(){
        printhello();
        cout<<"this is main:"<<endl;
        cout<<"the factorial of 5 is :"<<factorial(5)<<endl;
        return 0;
}
                
```

printhello.cpp

```cpp
#include<iostream>
#include"functions.h"
using namespace std;

void printhello(){
        int i;
        cout<<"hello world" <<endl;
}
```

factorial.cpp

```cpp
#include"functions.h"

int factorial(int n){
        if(n==1)
                return 1;
        else return n*factorial(n-1);
}
```



## 没有`makefile`的编译

```bash
g++ *.cpp -o main
./main
```



## makefile多文件编译

`version1`

​	缺点：每次都要重新编译，如果 文件很多，编译时间太长

```makefile
hello: main.cpp printhello.cpp factorial.cpp # hello 是目标，：后面的是依赖
	g++ -o hello main.cpp printhello.cpp factorial.cpp # 该行要有开头 tap
```

```bash
make # 自动找Makefile，如果自定义了文件名要 make -f makefile_name    
```



`version2`

```makefile
CXX = g++
TARGET = hello
OBJ = main.o printhello.o factorial.o

$(TARGET):$(OBJ)
	$(CXX) -o $(TARGET) $(OBJ)
	
main.o: main.cpp
	$(CXX) -c main.cpp
	
printhello.o: printhello.o
	$(CXX) -c printhello.cpp
	
factorial.o: factorial.cpp
	$(CXX) -c factorial.cpp
```



​	`version3`

```makefile
CXX = g++
TARGET = hello
OBJ = main.o printhello.o factorial.o # version4会改进这里

CXXFLAGS = -c -Wall

$(TARGET): $(OBJ)
	$(CXX) -o $@ $^ # @表示目标文件TAEGET，^表示依赖项的所有文件
	
%.o: %.cpp
	$(CXX) $(CXXFLAGS)  $< -o $@ # <表示 依赖项的第一个
	
.PHONY: clean # 该行的作用是防止clean同名makefile文件使make clean无效
clean: 
	rm -f *.o $(TARGET)
```



`version4`

```makefile
CXX　= g++
TARGET = hello
SRC = $(wildcard *.cpp)
OBJ = $(patsubst %.cpp, %.o, $(SRC)) 

CXXFLAGS = -c -Wall

$(TARGET): $(OBJ)
	$(CXX) -o $@ $^
	
%.o: %.cpp
	$(CXX) $(CXXFLAGS) -o $@ $<
	
.PHONY:clean
clean: 
	rm -f %.o $(TARGET)
```











# CMake

## 语法相对简单，且跨平台

`Makefile` -> `CMakeLists.txt`

```CMake
# CMakeLists.txt内容如下 
cmake_minimum_required(VERSION 3.10)

project(hello)

add_executable(hello main.cpp factorial.cpp printhello.cpp)
```

`bash`操作

```bash
cmake . # 在当前目录查找执行CMakeLists.txt
make 	# cmake 帮助生成的Makefile

# cmake会生成一系列文件和目录，建议创建build目录在build中cmake
mkdir build
cd build
cmake ..
make

```

