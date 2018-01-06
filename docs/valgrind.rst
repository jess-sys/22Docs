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

	valgrind [valgrind\'s options] ./program [program\'s arguments]

Valgrind will now lauch your program and report any error it detects.

Valgrind's messages
-------------------

.. WARNING::
	Valgrind will give you more information about where your errors come from if your code has been compiled using GCC's `-g` flag.
