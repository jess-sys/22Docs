.. EPITECH 2022 - Technical Documentation documentation master file, created by
   sphinx-quickstart on Tue Nov  7 09:05:01 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Criterion
============

Criterion is a unit-testing tool that will allow you to test your code efficiently.

Introduction
------------

Setup
~~~~~~~

If you haven’t installed Criterion yet, please download install_Criterion.sh from
the intranet, then run it. Criterion will be installed.

What is Criterion ?
~~~~~~~~~~~~~~~~~~~

.. WARNING::
   Do not forget to follow the Coding Style !

Criterion lets you create a set of tests that will be executed on your code.
Creating a test follows this pattern :

.. code-block:: c

      Test(suite_name, test_name, ...)
      {
      		//tests
      }

The ``suite_name`` must be the name of a set of tests, which will help you know exactly what part of your code fails.
The ``test_name`` is here to help you know which precise test failed, and thus will be usefull to help you debug your code.

.. DANGER::
	If you want your tests to work, you must compile your code with Criterion's library using the ``-lcriterion`` flag. You should also consider running your code with the ``--verbose`` flag if you want a full summary of the test's results.

Asserts
-------

Asserts are Criterion's way of defining tests to run. You will have to define several assets in order to test every bit of your code.
Let's see an example using Criterion's most basic assert, ``cr_assert``. This asserts takes a condition as a parameter, and the test passes if the condition is true :

.. code-block:: c

      Test(basics, first_test) {
      		cr_assert(1 + 1 == 2);
      }

Here, as one plus one will always be equal to two, the test will always pass.
Let's see the full list of Criterion's asserts.

.. WARNING::
	All asserts are located in the header ``<criterion/criterion.h>``. Don't forget to include this header in order to make your tests functionnal.

.. note::
	Most asserts here have the ``assert`` keyword in their name. You should be aware that using those macros will stop the whole test right away if they fail. If you want to run some code after the test, even if it fail, you should really consider changing ``assert`` with ``expect``. For example, in the following code the printf function will not be executed :

	.. code-block:: c

		Test(suite_name, test_name) {
			cr_assert(0);
			printf("I wanted to run this code :/");
		}

	But in this example it will still run :

	.. code-block:: c

		Test(suite_name, test_name) {
			cr_expect(0);
			printf("I will live :D");
		}

Basic asserts
~~~~~~~~~~~~~

.. note::
	Additional parameters
	In addition to the condition tested by the following tests, it is possible to give a string as parameter which will be printed to stderr if the test fails.
	This optionnal string can take printf-like arguments.
	For example, you can do something like:

	.. code-block:: c

		Test(suite_name, test_name) {
			int i = 0;
			int j = 2;
			cr_assert("i * 2 == j", "The result was %d. Expected %d", i * 2, j);
		}

.. c:function:: cr_assert(``condition``)

Passes if ``condition`` is ``true``.

.. c:function:: cr_assert_not(``condition``)

Passes if ``condition`` is ``false``.

.. c:function:: cr_assert_null(``condition``)
 		cr_assert_not_null(``condition``)

Passes if ``condition`` is ``NULL``, or is not ``NULL``.

Common asserts
~~~~~~~~~~~~~~

.. c:function:: cr_assert_eq(Actual, Reference)
		cr_assert_neq(Actual, Reference)

Passes if and only if ``Actual`` is equal (or not equal, if you are using ``neq``) to ``Reference``.

.. c:function:: cr_assert_lt(Actual, Reference)
		cr_assert_leq(Actual, Reference)

Will pass if ``Actual`` is lesser than (or lesser than or equal if you used ``leq``) ``Reference``.

.. c:function:: cr_assert_gt(Actual, Reference)
		cr_assert_geq(Actual, Reference)

Will pass if ``Actual`` is greater than (or greater than or equal if you used ``geq``) ``Reference``.

String asserts
~~~~~~~~~~~~~~

.. c:function:: cr_assert_str_eq(Actual, Reference)
		cr_assert_str_neq(Actual, Reference)


Just like :c:func:`cr_assert_eq`, but will check two strings, character by character.

.. c:function:: cr_assert_empty(Value)
		cr_assert_not_empty(Value)

Will pass if the string is empty (or is not empty is you used ``not_empty``).

.. note::
	There are also ``str_lt``, ``str_gt``, etc... macros that will check the lexicographical values of the two sting given, just like your ``my_strcmp`` would do (if you've done it well :D).
