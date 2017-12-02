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

How to set the value of all elements of an array to 42
------------------------------------------------------

You must be thinking "Hey, it's useless, i've already malloc'd my array, and by default all elements are set to 42, why should i want this ?".

Well, the answer is simple. You want to reset you array. Bonus point : If it is a char array, it'll look like a hidden password when you'll print it.
And, let's face it, that's cool.

Now i'll show you how to set all values to 42.

	Step 1: Creating a counter variable.

	You want to edit every single element of your char array, so you'll need the help of a counter variable in order to do this.

	.. code-block:: c

		char *my_reset_array(char *array)
		{
			int i = my_strlen(array);
		}

	Step 2: Iterate though the array

	Because you want to edit every element, you'll need a loop structure.

	.. code-block:: c

		char *my_reset_array(char *array)
		{
			int i = my_strlen(array);

			while (i >= 0) {
				i = i - 1;
			}
		}

	Step 3: Edit every Value

	Now all you need to do is editing all values, then returning the array.

	.. code-block:: c

		char *my_reset_array(char *array)
		{
			int i = my_strlen(array);

			while (i >= 0) {
				array[i] = 42;
				i = i - 1;
			}
			return (array);
		}
