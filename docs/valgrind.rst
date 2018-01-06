Debug : Understanding Valgrind's messages
=========================================

Valgrind is a very useful debug tool, which happens to be already installed on EPITECH's dump.

Introduction
------------

What is Valgrind ?
~~~~~~~~~~~~~~~~~~

Valgrind is an "instrumentation framework for building dynamic analysis tools", according to Valgrind's official documentation.

Valgrind comes with a bunch of tools, but in this page we will only concentrate on one of those tools : Memcheck.

Memcheck is a memory error detector. As such, it will detect and show you every memory error your code produces.
It will also show you your program's memory leaks.

How to use it ?
~~~~~~~~~~~~~~~

To use valgrind to debug your program, you can simply add `valgrind` in front of your program's name and arguments. It should look like this

.. code-block:: bash

	$ valgrind [valgrind\'s options] ./program [program\'s arguments]

Valgrind will now lauch your program and report any error it detects.

Valgrind's messages
-------------------

.. WARNING::
	Valgrind will give you more information about where your errors come from if your code has been compiled using GCC's ``-g`` flag.

Invalid read/write
~~~~~~~~~~~~~~~~~~

One of the most common errors you will encounter are invalid reads or writes.

First, let's write a simple C program.

.. code-block:: c

	int main(void)
	{
		char *str = malloc(sizeof(char) * 10);
		int i = 0;

		while (i < 15) {
			str[i] = '\0';
			i = i + 1;
		}
		free(str);
		return (0);
	}

Yes, this code is absolutely useless, but still, let's compile it then run it with valgrind.

.. code-block:: bash

	$ gcc main.c -g
	$ valgrind ./a.out
	==18332== Memcheck, a memory error detector
	==18332== Copyright (C) 2002-2017, and GNU GPL\'d, by Julian Seward et al.
	==18332== Using Valgrind-3.13.0 and LibVEX; rerun with -h for copyright info
	==18332== Command: ./a.out
	==18332==
	==18332== Invalid write of size 1
	==18332==    at 0x400553: main (test.c:7)
	==18332==  Address 0x521004a is 0 bytes after a block of size 10 alloc\'d
	==18332==    at 0x4C2EB6B: malloc (vg_replace_malloc.c:299)
	==18332==    by 0x400538: main (test.c:3)
	==18332==
	==18332==
	==18332== HEAP SUMMARY:
	==18332==     in use at exit: 0 bytes in 0 blocks
	==18332==   total heap usage: 1 allocs, 1 frees, 10 bytes allocated
	==18332==
	==18332== All heap blocks were freed -- no leaks are possible
	==18332==
	==18332== For counts of detected and suppressed errors, rerun with: -v
	==18332== ERROR SUMMARY: 5 errors from 1 contexts (suppressed: 0 from 0)

So, what happened ? Well, valgrind detected an invalid write error in our program. But what does it mean ?

"Invalid write" means that our program tries to write data in a memory zone which it doesn't own.

But valgrind tells you way more than that. It first tells you the size of the written data, which is 1 bytes, and corresponds to the size of a character.
Then tells you where you made a mistake. Line 7, which corresponds to ``str[i] = '\0'``. It also tells you that the invalid adress is located right after a block of ten bytes allocated.
What is means is that a 10 bytes (so probably 10 characters) long memory zone was allocated, but we tried to write an eleventh character.
