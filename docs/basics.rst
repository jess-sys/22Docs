.. EPITECH 2022 - Technical Documentation documentation master file, created by
   sphinx-quickstart on Tue Nov  7 09:05:01 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

========
C Basics
========

-----------------
Table of contents
-----------------

.. toctree::
   :maxdepth: 2

Just to be sure that you still know the basics.

Types
-----------

Integers
~~~~~~~~~~~~~

+---------------+----------------------------+---------------------------+-------+
|Type           |Minimum                     |Maximum                    | Bytes |
+===============+============================+===========================+=======+
| INT           |-2,147,483,648              |2,147,483,647              |4      |
+---------------+----------------------------+---------------------------+-------+
| UNSIGNED INT  |0                           |4,294,967,295              |4      |
+---------------+----------------------------+---------------------------+-------+
| LONG          |-9,223,372,036,854,775,808  |9,223,372,036,854,775,807  |8      |
+---------------+----------------------------+---------------------------+-------+
| UNSIGNED LONG |0                           |18,446,744,073,709,551,615 |8      |
+---------------+----------------------------+---------------------------+-------+
| SHORT         |-32,768                     |32,767                     |2      |
+---------------+----------------------------+---------------------------+-------+
| UNSIGNED SHORT|0                           |65,535                     |2      |
+---------------+----------------------------+---------------------------+-------+

Operators
=========

Arithmetic Operators
---------------------

.. topic:: Addition : +

   Addition is used to add operands

.. code-block:: c

   int a = 10;
   int b = 15;


Now a + b = 30

Bitwise Operators
------------------

Bitwise operator perfom bit-by-bit operation.

Binary
~~~~~~~

**Did you just say bit-by-bit operation ?**

A bit-by-bit operation is an operation that apply to each bit of a binary value.

**Binary ?**

Let's take a ``short`` as an example,
our ``short`` has a value of 42.

In computer's memory, it takes 2 bytes to stores our value.
It will look like that

.. code-block:: python

   0 0 0 0 0 0 0 0  0 0 0 1 0 1 0 1

Logic gate
~~~~~~~~~~~

It is important to understand what a logic gate is.

Logic are used in electronic, they have inputs and an output

Look at the first gate:

**AND**

.. code-block:: none

	    +-----+
       A ---|     |
	    |  &  |---- OUTPUT
       B ---|     |
	    +-----+


=====  =====  ======
   Inputs     Output
------------  ------
  A      B    A & B
=====  =====  ======
0      0      0
1      0      0
0      1      0
1      1      1
=====  =====  ======

The output is 1 when both of the inputs are 1

**OR**

.. code-block:: none

	    +-----+
       A ---|     |
	    |  ≥1 |---- OUTPUT
       B ---|     |
	    +-----+


=====  =====  ======
   Inputs     Output
------------  ------
  A      B    A & B
=====  =====  ======
0      0      0
1      0      1
0      1      1
1      1      1
=====  =====  ======

The output is 1 when at least one of the inputs is 1

**XOR**

.. code-block:: none

	    +-----+
       A ---|     |
	    |  =1 |---- OUTPUT
       B ---|     |
	    +-----+


=====  =====  ======
   Inputs     Output
------------  ------
  A      B    A & B
=====  =====  ======
0      0      0
1      0      1
0      1      1
1      1      0
=====  =====  ======

The output is 1 when `only` one of the inputs is 1

Operators
~~~~~~~~~~

Now we are going to look at bitwise operators in C

What operators do is applying logic gate to each one of bits in the variable

.. topic:: AND : &

	   AND operator apply AND gate to each bit

.. code-block:: c

   short a = 10; // 00000000 00001010
   short b = 20; // 00000000 00010100


To represent the operation:

.. code-block:: none

    00000000 00001010
  & 00000000 00010100

    00000000 00000000

a & b = 0


.. topic:: OR : |

	   OR operator apply OR gate to each bit

.. code-block:: c

   short a = 10; // 00000000 00001010
   short b = 15; // 00000000 00001111


To represent the operation:

.. code-block:: none

    00000000 00001010
  & 00000000 00001111

    00000000 00001111

a | b = 10

