Debug : Understanding Valgrind's messages
=========================================

Valgrind is a very useful debug tool, which happens to be already installed on EPITECH's dump.

Introduction
------------

What is Valgrind ?
~~~~~~~~~~~~~~~~~~

Valgrind is an "instrumentation framework for building dynamic analysis tools", according to Valgrind's official documentation.

Valgrind comes with a bunch of tools, but in this page we will only focus on one of those tools : Memcheck.

Memcheck is a memory error detector. As such, it will detect and show you every memory error your code produces.
It will also show you your program's memory leaks.

How to use it ?
~~~~~~~~~~~~~~~

To use Valgrind to debug your program, you can simply add `Valgrind` in front of your program's name and arguments. It should look like this

.. code-block:: console

	$ valgrind [valgrind\'s options] ./program [program\'s arguments]

Valgrind will now lauch your program and report any error it detects.

Valgrind's messages
-------------------

.. WARNING::
	Valgrind will give you more information about where your errors come from if your code has been compiled using GCC's ``-g`` flag.

Invalid read/write
~~~~~~~~~~~~~~~~~~

One of the most common errors you will encounter are invalid reads or writes.

Invalid write
/////////////

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

.. code-block:: console

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
	==18332== All heap blocks were free'd -- no leaks are possible
	==18332==
	==18332== For counts of detected and suppressed errors, rerun with: -v
	==18332== ERROR SUMMARY: 5 errors from 1 contexts (suppressed: 0 from 0)

So, what happened ? Well, Valgrind detected an invalid write error in our program. But what does it mean ?

"Invalid write" means that our program tries to write data in a memory zone where it shouldn't.

But Valgrind tells you way more than that. It first tells you the size of the written data, which is 1 bytes, and corresponds to the size of a character.
Then the line ``at 0x400553: main (test.c:7)`` tells you at which line your error occured. Line 7, which corresponds to ``str[i] = '\0'``.

At the line ``Address 0x521004a is 0 bytes after a block of size 10 alloc\'d``, it also tells you that the invalid adress is located right after a block of ten bytes allocated.
What this means is that a 10 bytes (so probably 10 characters) long memory zone was allocated, but we tried to write an eleventh byte.

Invalid read
////////////

This other code will produce a Invalid read error :

.. code-block:: c

	int main(void)
	{
	        int i;
	        int *ptr = NULL;

	        i = *ptr;
	        return (0);
	}

If we compile and run this code, Valgrind will produce this error :

.. code-block:: console

	==26212== Invalid read of size 4
	==26212==    at 0x400497: main (test.c:8)
	==26212==  Address 0x0 is not stack\'d, malloc\'d or (recently) free\'d

It means that we tried to read 4 bytes, starting at adress 0x0 (for those of you who don't know it yet, NULL is actually a pointer to adress 0x0, so we tried to read 4 bytes starting from NULL).

As before, Valgrind also tells us that the error occured at line 8 of our code, which corresponds to this instruction : ``i = *ptr``.

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

.. code-block:: console

	==28042== Conditional jump or move depends on uninitialised value(s)
	==28042==    at 0x4004E3: main (test.c:5)

This message may be a bit harder to understand.

Well, a jump is a computer instruction similar to a ``goto`` in C. There are several types of jumps. Some are unconditionnal, meaning the jump will always occur. Some other are conditionals,
which means that the jump will be taken if a previous test was successful, and will not otherwise.

In this case, our program had a conditional jump, because one of the values that were test was not initialized, it led to unexpected behaviour. It means that the outcome of the test may change.
For example it could work as intented on your computer, but could fail during the autograder's tests.

.. note::
	This type of error could happen if you do some tests involving a recently malloc'd block. (Note that malloc will *never* initialize your data).

Syscall param points to unadressable bytes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here is our program :

.. code-block:: c

	int main(void)
	{
		int fd = open("test", O_RDONLY);
		char *buff = malloc(sizeof(char) * 3);

		free(buff);
		read(fd, buff, 2);
	}

``read`` will try to read at the adress pointed to by ``buff``. But this adress has already been free'd, so Valgrind will show us this error :


.. code-block:: console

	==32002== Syscall param read(buf) points to unaddressable byte(s)
	==32002==    at 0x4F3B410: __read_nocancel (in /usr/lib64/libc-2.25.so)
	==32002==    by 0x400605: main (test.c:11)
	==32002==  Address 0x5210040 is 0 bytes inside a block of size 3 free\'d
	==32002==    at 0x4C2FD18: free (vg_replace_malloc.c:530)
	==32002==    by 0x4005EF: main (test.c:10)
	==32002==  Block was alloc\'d at
	==32002==    at 0x4C2EB6B: malloc (vg_replace_malloc.c:299)
	==32002==    by 0x4005DF: main (test.c:8)

Here there is a lot of information that will help you debug your code. First, we know that we gave an invalid pointer to a system call, ``read`` in our case.

Then Valgrind tells us that this pointer is "0 bytes inside a block of size 3 free'd". In fact, we allocated a 3 bytes block, then free'd it. "0 bytes inside" means that our pointer
points to the very first byte of this block.

Valgrind tells us where the error occured, where the block was free'd and also where is was malloc'd.

Invalid/mismatched frees
~~~~~~~~~~~~~~~~~~~~~~~~


Invalid free
////////////

Another error you may encounter is the "Invalid free" one. It means that we tried to free a pointer that cannot be free'd. Here is an example :

.. code-block:: c

	int main(void)
	{
		char *buff = malloc(sizeof(char) * 54);

		free(buff);
		free(buff);
		return (0);
	}

Yes, I agree, this error is obvious. But it does happen that the same pointer is twice free'd, or that some programmer tries to free something that wasn't allocated. There are plenty of reasons for
an invalid free to happen. Let's look at Valgrind's message :

.. code-block:: console

	==755== Invalid free() / delete / delete[] / realloc()
	==755==    at 0x4C2FD18: free (vg_replace_malloc.c:530)
	==755==    by 0x400554: main (test.c:10)
	==755==  Address 0x5210040 is 0 bytes inside a block of size 54 free\'d
	==755==    at 0x4C2FD18: free (vg_replace_malloc.c:530)
	==755==    by 0x400548: main (test.c:9)
	==755==  Block was alloc\'d at
	==755==    at 0x4C2EB6B: malloc (vg_replace_malloc.c:299)
	==755==    by 0x400538: main (test.c:7)

Valgrind tells use that there is a problem with a free, a delete, a delete[] or a realloc, but since delete is a C++ instruction, and we're not allowed to use realloc at EPITECH, you will probably
only use free.

As before, Valgrind tells us that the error occured because we tried to use free on an adress that belongs to an already free'd block.

Mismatched free
///////////////

Another error you can encounter is this one :

.. code-block:: console

	==3073== Mismatched free() / delete / delete []
	==3073==    at 0x4C2FD18: free (vg_replace_malloc.c:530)
	==3073==    by 0x400613: main (in /home/oursin/a.out)
	==3073==  Address 0xa09a5d0 is 0 bytes inside a block of size 368 alloc\'d
	==3073==    at 0x4C2F1CA: operator new(unsigned long) (vg_replace_malloc.c:334)
	==3073==    by 0x4E5AB0F: sfSprite_create (in /usr/local/lib/libc_graph_prog.so)
	==3073==    by 0x400603: main (in /home/oursin/a.out)

Here, I created a CSFML sprite using ``sfSprite_create``, then I tried to free this sprite, resulting in this error.

In fact,  ``sfSprite_create`` does allocate some memory, but it does not use our dear friend ``malloc``, but it's C++ brother,  ``new``.
And the problem is that something that has been allocated using ``new`` must be free'd using ``delete``, not ``free``. As ``delete`` does not exist in C, you should use CSFML's ``sfSprite_destroy`` function.

Fishy values
~~~~~~~~~~~~

The last type of error you may see is this one :

.. code-block:: console

	==29010== Argument 'size' of function malloc has a fishy (possibly negative) value: -1
	==29010==    at 0x4C2EB6B: malloc (vg_replace_malloc.c:299)
	==29010==    by 0x4004EA: main (in /home/oursin/a.out)

It simply means that you gave a impossible value to a system call. In this case I called ``malloc`` with argument ``-1``.
