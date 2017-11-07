.. EPITECH 2022 - Technical Documentation documentation master file, created by
   sphinx-quickstart on Tue Nov  7 09:05:01 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Criterion
============

.. toctree::
   :maxdepth: 2

Criterion is a unit-testing tool that will allow you to test your code efficiently.

Introduction
------------

Setup
~~~~~~~

If you haven’t installed Criterion yet, please download install_criterion.sh from
the intranet, then run it. Criterion will be installed.

What is criterion ?
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

Basic asserts
~~~~~~~~~~~~~

.. note::
	Additional parameters
	In addition to the condition tested by the following tests, it is possible to give a string as parameter which will be printed to stderr if the test fails.
	This optionnal string can take printf-like arguments.
	For example, you can do something like:

	.. code-block:: c

		Test(suite_name, test, name) {
			int i = 0;
			int j = 2;
			cr_assert("i * 2 == j", "The result was %d. Expected %d", i * 2, j);
		}

.. c:macro:: cr_assert(``condition``)

Passes if ``condition`` is ``true``.

.. c:macro:: cr_assert_not(``condition``)

Passes if ``condition`` is ``false``.

.. c:macro:: cr_assert_null(``condition``)
.. c:macro:: cr_assert_not_null(``condition``)

Passes if ``condition`` is, or is not ``NULL``.

Common asserts
~~~~~~~~~~~~~~

.. c:macro:: cr_assert_eq(Actual, Reference)

Passes if and only if ``Actual`` is equal to ``Reference``.

.. c:macro:: cr_assert_neq(Actual, Reference)

Passes if and only if ``Actual`` is not equal to ``Reference``.

String asserts
~~~~~~~~~~~~~~

.. c:macro:: cr_assert_str_eq(Actual, Reference)

Just like :macro:`cr_assert_eq`, but will check two strings, character by character.

.. c:macro:: cr_assert_str_neq
