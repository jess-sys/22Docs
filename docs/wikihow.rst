WikiHow
=======

Have you ever wondered how to properly malloc an integer variable ? Here you'll find the answers to all your questions !

.. warning::
	Please do not consider this page as a reliable source of knowledge. If you really believe anything here, you're even dumber than you look.

How to malloc an int
--------------------

First things first : "Why should I use malloc on an int variable ?"

	**To ensure you have enough space to hold every possible numeric value.**

Now that this is clear, let's speak a bit about malloc. Malloc uses a special variable type, called size_t to take its value. Usually this type of variable can be
obtained by using the `sizeof` function. But have you ever wondered what happens if you use sizeof on the size_t type ? Yeah, you've guessed it. Infinity.

Now that we know that, here's a sample code to help you create you integer that can hold infinity.

.. code-block:: c

	int integer = malloc(sizeof(size_t));

.. warning::

	Do not EVER try to malloc infinity onto a void variable. DO YOU REALLY WANT TO KNOW WHAT HAPPENS WHEN YOU PUT AN INFINITY INTO NOTHING ? I do not.
	So please be mercyful and spare our lives.
