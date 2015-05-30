# Examining Data

用 print打印变量

	print expr
	print /f expr
	
如果不写expr，gdb就打印上次print的变量

`x`命令可以用指定的格式查看内存。

`ptype exp` 查看变量类型信息。

另一种查看变量内容的方法是使用explore命令。It o↵ers an interactive way to start at the highest level
of the data type of an expression. and explore all the way down to leaf scalar values/fields embedded in the higher level data types. 	

	explore arg
		#arg is either an expression (in the source language), 
		or a type visible in the current context of the program being debugged.










	set multiple-symbols mode
	show multiple-symbols
		#When mode is set to ask, the debugger always uses the menu when 
		an ambiguity is detected.

## Program Variables
	
If you wish, you can specify a static variable in a particular function or file by using the colon-colon (::) notation:		




	$1 = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	(gdb) p *(cs.arr)@9
	$2 = {0, 1, 2, 3, 4, 5, 6, 7, 8}
	(gdb) p cs.arr
	$3 = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
	(gdb) p /x cs.arr
	$4 = {0x0, 0x1, 0x2, 0x3, 0x4, 0x5, 0x6, 0x7, 0x8, 0x9}		



	x addr
	x
	
		n, the repeat count
		f, the display format ((‘x’, ‘d’, ‘u’, ‘o’, ‘t’, ‘a’, ‘c’, ‘f’, ‘s’))
		u, the unit size
			The unit size is any of

 查看3调机器指令
 
	0x10000104a <main()+1178>:	jmpq   0x10000104f <main()+1183>
	0x10000104f <main()+1183>:	mov    -0x214(%rbp),%eax
	0x100001055 <main()+1189>:	add    $0x1,%eax		



		#Compare the data of a loadable section section-name in the executable 
		file of the program being debugged with the same section 
		in the target machine’s memory, and report any mismatches.
		With no arguments, compares all loadable sections. With an argument of -r, 
		compares all loadable read-only sections.

		Note: for remote targets, this command can be accelerated if the target 
		sup- ports computing the CRC checksum of a block of memory 


## Automatic Display				
	
	display expr
	display/fmt expr
	display/fmt addr
	undisplay dnums...
	delete display dnums...
	disable display dnums...
	enable display dnums...
	display				
	info display

## Print Settings

	set print address	
	set print address on
	set print address off
	show print address

## Pretty Printing

gdb provides a mechanism to allow pretty-printing of values using Python code

### Pretty-Printer Introduction

gdb打印一个变量的时候，首先看看有没有pretty-printer，如果有的花就用pretty-printer，否则用普通的print.

`info pretty-printer`可以打印当前已经有的pretty-printers，如果一个pretty-printers可以打印多个类型，那么它的子printer可以打印单独的类型。每个子pretty-printers都有它自己的名字。

格式是：

	printer-name ;subprinter-name.
	
Pretty-printers are installed by registering them with gdb。Typically they are auto- matically loaded and registered when the corresponding debug information is loaded, thus making them available without having to do anything special.

向gdb注册了以后，Pretty-printers就可以自动加载.

There are three places where a pretty-printer can be registered.(没有说那三个地方)


* 第一种是全局的，对所有的inferior都可用
* 第二种是针对某个程序的
* 第三种是objfile，需要加载和卸载。


### Pretty-Printer Example

Here is how a C++ std::string looks without a pretty-printer:

	(gdb) print s
	$1 = {


	$2 = "abcd"

### Pretty-Printer Commands

	info pretty-printer [object-regexp [name-regexp]]
	    #Print the list of installed pretty-printers. 
	    This includes disabled pretty-printers, which are marked as such.

	disable pretty-printer [object-regexp [name-regexp]]
	enable pretty-printer [object-regexp [name-regexp]]

例子：假如我们有三个pretty-printers:一个叫做library1.so 的so里面有foo这个类型的pretty-printers。library2.so里面有bar1，bar2两种类型的pretty-printers

	(gdb) info pretty-printer
		  bar1










would save in $foo the value contained in the object pointed to by object_ptr.




	set $i = 0



		(gdb) print $_exitcode
		$1 = void
		(gdb) print $_isvoid ($_exitcode)
		$2 = 1
		(gdb) run

		Returns one if the length bytes at the addresses given by buf1 and buf2 are equal. 		Otherwise it returns zero.

	$_regex(str, regex)
		#Returns one if the string str matches the regular expression regex. 
		Otherwise it returns zero.			

	$_streq(str1, str2)
	$_strlen(str)
	$_caller_is(name[, number_of_frames])
		#Returns one if the calling function’s name is equal to name. 
		Otherwise it returns zero.
	
	一个例子：	
      (gdb) backtrace
      #0  bottom_func ()
          at testsuite/gdb.python/py-caller-is.c:21
      #1  0x00000000004005a0 in middle_func ()
          at testsuite/gdb.python/py-caller-is.c:27
      #2  0x00000000004005ab in top_func ()
          at testsuite/gdb.python/py-caller-is.c:33
      #3  0x00000000004005b6 in main ()
          at testsuite/gdb.python/py-caller-is.c:39
      (gdb) print $_caller_is ("middle_func")
      $1 = 1
      (gdb) print $_caller_is ("top_func", 2)
      $1 = 1				
				


	help function
	
			        
## Registers
	
如果想看寄存器里的内容，就用`$+寄存器名字`，例如：

	(gdb) print /x $rax
	$2 = 0x100000bb0							

用`info register`可以查看当前系统有哪些寄存器。
	
	info registers
		Print the names and values of all registers except floating-point and 
		vector registers (in the selected stack frame).
		
	info all-registers
		Print the names and values of all registers, 
		including floating-point and vector registers (in the selected stack frame).
				
(什么是vector register?)

	info registers regname ...

打印下一条指令：
	
	x/i $pc	








		#Display the auxiliary vector of the inferior, 
		which can be either a live process or a core dump file.

On some targets, gdb can access operating system-specific information and show it to you.
The types of information available will di↵er depending on the type of operating system running on the target.
	
	info os infotype
		processes   #Display the list of processes on the target.
		procgroups  #Display the list of process groups on the target.
		threads     #Display the list of threads running on the target.
		files       #Display the list of open file descriptors on the target.
		sockets     #Display the list of Internet-domain sockets on the target
		shm         #Display the list of all System V shared-memory regions on the tar- get.
		semaphores  #Display the list of all System V semaphore sets on the target.
		msg         #Display the list of all System V message queues on the target.
		modules     #Display the list of all loaded kernel modules on the target.

	info os									

By default the description of memory regions is fetched from the target
but the user can override the fetched regions.

Defined memory regions can be individually enabled and disabled. When a memory region is disabled, gdb uses the default attributes when accessing memory in that region. Similarly, if no memory regions have been defined, gdb uses the default attributes when accessing all memory.

		#Define a memory region bounded by lower and 
		upper with attributes attributes . . . , 
		and add it to the list of regions monitored by gdb.

	mem auto
		#Discard any user changes to the memory regions and use target-supplied regions, 
		if available, or no regions if the target does not support.

	delete mem nums...
	disable mem nums...
	enable mem nums...
	info mem

### Attributes
#### Memory Access Mode
	
	ro #Memory is read only.
	wo #Memory is write only.
	rw #Memory is read/write. This is the default.
	
#### Memory Access Size

	8 Use 8 bit memory accesses.
	16 Use 16 bit memory accesses.
	32 Use 32 bit memory accesses.
	64 Use 64 bit memory accesses.

#### Data Cache
The data cache attributes set whether gdb will cache target memory.While this generally improves performance by reducing debug protocol overhead, it can lead to incorrect results because gdb does not know about volatile variables or memory mapped device registers.

	cache        #Enable gdb to cache target memory.
	nocache      #Disable gdb from caching target memory. This is the default.

### Memory Access Checking
gdb can be instructed to refuse accesses to memory that is not explicitly described. This can be useful if accessing such regions has undesired e↵ects for a specific target, or to provide better error checking. The following commands control this behaviour.

	set mem inaccessible-by-default [on|off]
	show mem inaccessible-by-default

## Copy Between Memory and a File

可以用`dump`, `append`和`restore`在被调试进程的内存和文件中拷贝数据。dump和append向文件中写入数据，restor从文件中读取数据写到进程的内存中。Files may be in binary, Motorola S-record, Intel hex, or Tektronix Hex format; however, gdb can only append to binary files.

	dump [format] memory filename start_addr end_addr
	dump [format] value filename expr
		
		The format parameter may be any one of:
		binary  #Raw binary form.
		ihex     #Intel hex format.
		srec    #Motorola S-record format.
		tekhex  #Tektronix Hex format.

	append [binary] memory filename start_addr end_addr
	append [binary] value filename expr
	restore filename [binary] bias start end
	
## How to Produce a Core File from Your Program
A core file or core dump is a file that records the memory image of a running process and its process status (register values etc.).


偶尔需要保存一个当前运行的进程内存快照，gdb提供了以下命令：

	generate-core-file [file]
	gcore [file]
		Produce a core dump of the inferior process.
		
## Character Sets

## Caching Data of Targets

gdb caches data exchanged between the debugger and a target. Each cache is associated with the address space of the inferior.Such caching generally improves performance in remote debugging, because it reduces the overhead of the remote protocol by bundling memory reads and writes into large chunks.Unfor- tunately, simply caching everything would lead to incorrect results, since gdb does not necessarily know anything about volatile values, memory-mapped I/O addresses, etc. Fur- thermore, in non-stop mode (see Section 5.5.2 [Non-Stop Mode], page 76) memory can be changed while a gdb command is executing. Therefore, by default, gdb only caches data known to be on the stack3 or in the code segment. Other regions of memory can be explicitly marked as cacheable; see Section 10.17 [Memory Region Attributes], page 141.

	set remotecache on 
	set remotecache off
	show remotecache
	set stack-cache on 
	set stack-cache off
	show stack-cache
	set code-cache on 
	set code-cache off
	show code-cache
	info dcache [line]
	set dcache size size
	set dcache line-size line-size
	show dcache size
	show dcache line-size
	

## Search Memory		
Memory can be searched for a particular sequence of bytes with the find command.

	find [/sn] start_addr, +len, val1 [, val2, ...]
	find [/sn] start_addr, end_addr, val1 [, val2, ...]
	
	  (gdb) find &hello[0], +sizeof(hello), "hello"
      0x804956d <hello.1620+6>
      1 pattern found
      (gdb) find &hello[0], +sizeof(hello), ’h’, ’e’, ’l’, ’l’, ’o’
      0x8049567 <hello.1620>
      0x804956d <hello.1620+6>
      2 patterns found
      (gdb) find /b1 &hello[0], +sizeof(hello), ’h’, 0x65, ’l’
      0x8049567 <hello.1620>
      1 pattern found
      (gdb) find &mixed, +sizeof(mixed), (char) ’c’, (short) 0x1234, (int) 0x87654321
      0x8049560 <mixed.1625>
      1 pattern found
      (gdb) print $numfound
      $1 = 1
      (gdb) print $_
      $2 = (void *) 0x8049560


















































			
				
	
						








			
	














