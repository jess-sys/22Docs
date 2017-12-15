.. EPITECH 2022 - Technical Documentation documentation master file, created by
   sphinx-quickstart on Tue Nov  7 09:05:01 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

C Basics
========

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
-----------

Arithmetic Operators
~~~~~~~~~~~~~~~~~~~~~~~~

.. topic:: Addition : +

   Addition is used to add operands

.. c:code::
   int a = 10;
   int b = 15;


Now a + b = 30

Bitwise Operators
~~~~~~~~~~~~~~~~~~~~~~~~

Bitwise operator perfom bit-by-bit operation.

**Did you just say bit-by-bit operation ?**

A bit-by-bit operation is an operation that use binary values

**Binary ?**

Let's take a :c:type:`short` as an example

Our :c:type:`short` has a value of 42

In computer's memory, it takes 4 bytes to stores our value.
It will look like that

0 0 0 0 0 0 0 0 | 0 0 0 1 0 1 0 1
