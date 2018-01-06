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
Then the line ``at 0x400553: main (test.c:7)`` tells you at which line you error occured. Line 7, which corresponds to ``str[i] = '\0'``.

At the line ``Address 0x521004a is 0 bytes after a block of size 10 alloc\'d``, it also tells you that the invalid adress is located right after a block of ten bytes allocated.
What is means is that a 10 bytes (so probably 10 characters) long memory zone was allocated, but we tried to write an eleventh character.

This other code will produce a Invalid read error :

.. code-block:: c

	int main(void)
	{
	        int i;
	        int *ptr = NULL;

	        i = *ptr;
	        return (0);
	}

If we compile and run this code, valgrind will produce this error :

.. code-block:: bash

	==26212== Invalid read of size 4
	==26212==    at 0x400497: main (test.c:8)
	==26212==  Address 0x0 is not stack\'d, malloc\'d or (recently) free\'d

It means that we tried to read 4 bytes, starting at adress 0x0 (for those of you who don't know it yet, NULL is actually a pointer to adress 0x0, so we tried to read 4 butes starting from NULL).

As before, valgrind also tells us that the error occured at line 8 of our code, which corresponds to this instruction : ``i = *ptr``.

Conditional jumps
~~~~~~~~~~~~~~~~~

Let's create a new C program :

.. code-block:: c

	int main(void)
	{
		int i;

		if (i == 0) {
			my_printf("Hello\n");
		}
		return (0);
	}

Valgrind will produce this error :

.. code-block:: bash

	==28042== Conditional jump or move depends on uninitialised value(s)
	==28042==    at 0x4004E3: main (test.c:5)

This message may be a bit harder to understand.

Well, a jump is a computer instruction similar to a ``goto`` in C. There are several types of jumps. Some are unconditionnal, meaning the jump will always occur. Some other are conditionals,
which means that the jump will be taken if a previous test was successful, and will not occur otherwise.

In this case, my program had a conditional jump, but one of the values that were tested was not initialized, which will lead to unexpected behaviour. It means that the outcome of the test may change.
For example it could work as intented on your computer, but could fail during the autograder's tests
