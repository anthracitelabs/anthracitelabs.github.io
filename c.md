---
layout: default
---

# Notes on C

You can seperate your program into modules like mod1.c, mod2.c, mod3.c, main.c

Communication between modules
---------------------------------
Function inside module A can call the function inside another module B. But a prototype declaration must be made in module A to let the compiler know the signature of the function when compiling module A.
The point is even though more than one module can be specified to the compiler at the same time the compiler compiles each module independently.
That means no knowledge about structure definitions, function return types or function argument types is shared across module compilations. 

Functions in seperate files can communicate through external variables. 
External variable values can be accessed and changed by another module.
First define a global variable inside a module. Inside the module that wants to access the external variable the variable data type is preceded with the keyword extern.
If you want to define a global variable and make it local to a particular module use keyword static. 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
int moveVal = 0; // global variable definition

void foo()
{
	...
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
mod1.c

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
extern int moveVal; // external variable declaration for a global variable in mod1.c

void bar()
{
	moveVal = 5;
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
mod2.c

Memory
-------------------------------------
There are three forms of memory you can use in your program. Each type determines where and how it is stored.
	
	- static - persists throughout the entire life of the program
	- stack 
		- special region of memory that stores temporary variables (variables created inside a function)
		- LIFO (last in first out) linear data structure
		- variables allocated/freed automatically
		- divided into stack frames for each function call, when a function exits all its variables are popped off
	- heap  
		- hierarchical data structure
		- large pool of memory that can be used dynamically
		- memory is not automatically managed 
		- accessed through pointers (malloc/free)
		- no restrictions on the size of the heap - only limit is physical memory

storage classes
-------------------------------------
C storage classes 

	- auto - is used to declare variables of automatic storage duration
		- created when the block in which they are defined is entered
		- exists while the block is active
		- destroyed when the block is exited
		- local variables have automatic storage duration by default
		- also note that auto is completely different meaning in C++, must be used with caution for the sake of C/C++ compatibity
	- register
		- this storage class is used to define local variables that should be stored in a register instead of RAM
		- can be used for heavily used variables which need quick access
		- but it is the compiler's choice to put the variable in a register or not so depends on hardware and implementation restrictions
		- generally compilers themselves do optimizations and put the variables in registers
		- maximum size is equal to the register size
		- you cannot obtain the address of a register variable using pointers and cannot apply the & operator to it. It does not have a memory location
		- life-time is limited to the block it is defined in, same as auto
		- can only be used in local block, cannot be used globally
	- extern - tells us that a variable is defined elsewhere, not within the same block where it is used
		- The first way to use extern variable is declare a global variable inside a module - not preceded by the word extern, then use the variable in other modules by declaring it with keyword extern
		- second way is declaring the variable outside any function with the extern keyword and assigning an initial value to it
		- functions can be declared extern and this way function can be used inside a module without an include file
		- function declarations are extern by default the other alternative is static 
	- static
		- when applied to local variables it instructs the compiler to keep the variable in existence during the life-time of the program
		- when applied to global variables static modifier causes that variable's scope to be restricted to the file in which it is declared
		- when applied to functions, static function can only be called from within the same file as the function appears
		- static variables preserve their last value and no new memory is allocated as they are not re-declared
		- making local variables static allows them to maintain their values between function calls
		- Note that these two local static variables are different

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
int foo1()
{
	static int count = 100; // initial value at program execution start, then it can change during execution
}

int foo2()
{
	static int count2; 
	count2 = 100;	// variable will be initialized each time this function is called 
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
		
		- static variables should not be declared inside a structure because memory allocation for structure members should be contiguous. Whatever might be the case all structure members should reside in the same memory segment. The value of the structure element is fetched by counting the offset of the element from the beginning address of the structure.
		
		(#) Summary of storage classes
		
		Storage Class |       Declaration  	    |          Scope       	   | 	   Lifetime
					  |         Location	    |       (Visibility)	   |    	(Alive)
		--------------|-------------------------|--------------------------|--------------------------
		auto     	  | Inside a function/block |        Within the   	   | Until the function/block
					  |            				|      function/block  	   |	completes
		--------------|-------------------------|--------------------------|--------------------------
		register  	  | Inside a function/block |        Within the    	   | Until the function/block
					  |            				|      function/block  	   |	completes			
		--------------|-------------------------|--------------------------|--------------------------
		extern  	  | Outside all functions   | Entire file plus other   | Until the program
					  |            				| files where the variable |	terminates				
					  |            				| is declared as extern    |										  
		--------------|-------------------------|--------------------------|--------------------------
		static  	  | Inside a function/block |        Within the    	   | Until the program
		(local)		  |            				|      function/block  	   |	terminates			
		--------------|-------------------------|--------------------------|--------------------------
		static  	  | Outside all functions 	|   Entire file in which   | Until the program
		(global)	  |            				|      it is declared      |	terminates								  
		
typedef
--------------------------------
typedef can be used for casting.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
typedef int(*ptr_to_int_fun)(void);
char *p;
...= (ptr_to_int_fun) p;
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

variable length arrays
----------------------------------------
For C99 and C11 the following declaration is valid
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
int sum2d(int, int, int array[*][*]); // names can be ommitted
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

and the definition can be like the following
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
int sum2d(int rows, int cols, int array[rows][cols])
{
	//..
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

But C11 conforming compiler does not have to implement support for variable length arrays because it is an optional feature.
Check for support for variable length arrays
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
#ifdef __STDC_NO_VLA__
	printf("Variable length arrays are not supported!\n");
	exit(1);
#endif
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

flexible member arrays
-------------------------------
This is a feature introduced in C99 and there are some restrictions on the usage of this.
Flexible array has to be the last member of the struct and it also cannot be the only member of the struct
Each struct can contain one flexible array at most
The struct has to be allocated dynamically, cannot be statically initialized (size cannot be fixed at compile time)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
#include <stdio.h>
#include <malloc.h>

struct s {
	int arraysize;
	int array[];
};

int desiredSize = 10;
struct s *ptr;
ptr = malloc(sizeof(struct s) + desiredSize * sizeof(int));
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# unity builds
This is a method used in C and C++ software development to speed up the compilation of projects by combining multiple translation units into a single one, usually achieved by using include directives to bundle multiple source files into one larger file.

If two different translation units FileA.cpp:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
#include "Header.hpp"

// content of source file A ...
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
and FileB.cpp:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
#include "Header.hpp"

// content of source file B ...
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

in the same project both include the header Header.hpp, that header will be processed twice by the compiler chain, once for each build task. If the two translation units are merged into a single source file JumboFile.cpp:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
#include "FileA.cpp"
#include "FileB.cpp"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
then Header.hpp will be processed only once (thanks to include guards) when compiling JumboFile.cpp.



# mixing C with C++
To use C code in C++, you must wrap your C function declarations or header inclusions in an extern "C" block. This prevents the C++ compiler from mangling the function names, allowing the C++ linker to correctly find and connect to your compiled C code.

### Standard Approach: The extern "C" Wrapper

1. Inlining Specific Functions
   If you only need to call a few individual functions, declare them directly in your C++ file:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
// main.cpp
#include <iostream>

// Tell C++ to expect C-style linkage for these functions
extern "C" {
    void my_c_function();
    int add_numbers(int a, int b);
}

int main() {
    my_c_function(); 
    std::cout << add_numbers(5, 10) << std::endl;
    return 0;
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2. Including Entire C Headers
   If you are importing an existing C header file, wrap the #include statement:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
// main.cpp
#include <iostream>

extern "C" {
    #include "my_c_header.h"
}

int main() {
    run_c_logic();
    return 0;
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### The Cleanest Approach: Writing C/C++ Compatible Headers
If you own the C source code, the best practice is to make the header file smart. Use the __cplusplus macro to automatically apply extern "C" only when a C++ compiler reads the file. This prevents compilation errors if the header is also read by a regular C compiler.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
// my_c_header.h
#ifndef MY_C_HEADER_H
#define MY_C_HEADER_H

#ifdef __cplusplus
extern "C" {
#endif

// Define your C functions normally here
void my_c_function();
int add_numbers(int a, int b);

#ifdef __cplusplus
}
#endif

#endif

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Calling C Functions from C++
When you need to bring external C logic into your C++ codebase, implement the following steps:
1. Keep your implementation in a standard .c source file.
2. Structure your header file with the extern "C" block shown above.
3. Include that header normally using #include "my_c_header.h" inside your .cpp source files.

### Calling C++ Functions from C
Because the C compiler cannot parse C++ keywords, classes, or templates, you must expose a clean, flat C-compatible interface from your C++ code:
1. Write your core logic using standard C++ features inside a .cpp file.
2. Create a bridging header file using only valid C syntax wrapped inside extern "C" blocks.
3. Make sure any object-oriented components are passed to C as an opaque handle (e.g., void*).
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
/* bridge.cpp */
#include "my_cpp_class.hpp"
#include "bridge.h"

void* create_cpp_object() {
    return static_cast<void*>(new MyCppClass());
}

void call_cpp_method(void* obj_ptr) {
    static_cast<MyCppClass*>(obj_ptr)->executeMethod();
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You cannot compile C files directly into C++ files using a single compilation pass without telling the compiler how to handle them. You must compile them separately and link the resulting binaries:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ bash
gcc -c my_c_code.c -o my_c_code.o
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ bash
g++ main.cpp my_c_code.o -o my_program
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Critical Pitfalls to Avoid
* Type Casting: C++ has much stricter type checking than C. For instance, malloc returns a void* which C automatically converts. In C++, you must explicitly cast it:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
  int* arr = (int*)malloc(sizeof(int) * 10);
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Keywords: Ensure your C code does not use variable names that are reserved keywords in C++ (such as class, new, delete, template, or virtual).

* Struct Definitions: In C, you often need the typedef struct syntax to instantiate structs cleanly. In C++, the struct keyword automatically acts as a type definition. Keep the C-style syntax so it compiles seamlessly in both environments.



# Client-Server Application in C

client.c
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ C
/*
	if this is Windows programming:
	"sock" should be of type SOCKET instead of int.
	Use closesocket instead of closesocket
	#include <winsock2.h> instead of all those unix headers
*/

#include <sys/types.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <netdb.h>
#include <memory.h>
#include <ifaddrs.h>
#include <net/if.h>
#include <errno.h>
#include <stdio.h> 
#include <stdlib.h>
// #include <iostream>  // C++

#include <iomanip>
#include <fcntl.h>

#include <thread>

#define PORT	"13123"
#define	MAX_DATA_SIZE	1024
#define	SA struct sockaddr



void *get_in_addr(struct sockaddr *sa)
{
	if(sa->sa_family == AF_INET) {
		return &(((struct sockaddr_in*)sa)->sin_addr);
	}
	
	return &(((struct sockaddr_in6*)sa)->sin6_addr);
}

void memzero(void *b, int len)
{
	volatile int x;
	char *a;
	a = (char *)b;
	for(x = 0; x < len; x++)
	{
		*(a+x) = 0;
	}
}

#include <atomic>

int sockfd;
bool running = true;
std::atomic<bool> connected(false);

static unsigned char mem_data[RECEIVE_MEM_SIZE];
std::atomic<size_t> memptr(0);

bool iterate_mem_ptr(size_t n)
{
	memptr += n;
	
	if(memptr >= data_size)
	{
		// memptr exceeded data size, reset
		memptr = 0;
		return false;
	}
	
	return true;
}

void send()
{
	std::vector<uint8_t> message;
	for(size_t i = 0; i < send_data_size; i++)
	{
		message.push_back(mem_send_data[i]);
	}
	
	printf("sending %d bytes..\n", message.size());
	
	int result = send(sockfd, &message[0], message.size() * sizeof(uint8_t), 0);
}

void receive()
{
	while(running)
	{
		if(connected == true)
		{
			int n = read(sockfd, &mem_data[memptr], 4 * sizeof(uint8_t));
			
			size_t beginAddr = memptr;
			
			if(n > 0)
			{
				uint16_t numberOfBytes = 0x00;
				numberOfBytes = mem_data[memptr + 0] << 8;
				numberOfBytes |= mem_data[memptr + 1];
				
				uint8_t msgType = mem_data[memptr + 2];
				
				iterate_mem_ptr(n);
				
				size_t wordCount = numberOfBytes / 4;
				for(size_t i = 1; i < wordCount; i++)
				{
					size_t n = read(sockfd, &mem_data[memptr], 4 * sizeof(uint8_t));
					if(n > 0)
					{
						iterate_mem_ptr(n);
					}
				}
			}
		}
	}
}


int main()
{
	if(!connected)
	{
		struct addrinfo hints, *servinfo, *p;
		int rv;
		char s[INET6_ADDRSTRLEN];
		
		memzero(&hints, sizeof hints);
		hints.ai_family = AF_UNSPEC;
		hints.ai_socktype = SOCK_STREAM;
		
		if((rv = getaddrinfo(address, PORT, &hints, &servinfo)) != 0)
		{
			// address not found
		}
		
		// loop through all the results and connect to the first we can
		for(p = servinfo; p != NULL; p = p->ai_next) {
			if ((sockfd = socket(p->ai_family, p->ai_socktype,
					p->ai_protocol)) == -1) {
				perror("client: socket");
				continue;
			}

			if (connect(sockfd, p->ai_addr, p->ai_addrlen) == -1) {
				perror("client: connect");
				close(sockfd);
				continue;
			}

			break;
		}

		if (p == NULL) {
			fprintf(stderr, "client: failed to connect\n");
			return 2;
		}
		else
		{
			inet_ntop(p->ai_family, get_in_addr((struct sockaddr *)p->ai_addr), s, sizeof s);
			fcntl(sockfd, F_SETFL, 0_NONBLOCK);
			
			// client connected to %s
			connected = true;
			
			freeaddrinfo(servinfo);
		}
	}
	else
	{
		// already connected
	}
	
	std::thread t(receive);
	// session loop
	{
	
		// send whenever requested
		// send();
	}
	
	// when all done
	running = false;
	if(close(sockfd) == 0)
	{
		printf("socket closed\n");
		connected = false;
	}
	
	t.join();
	return 0;
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
