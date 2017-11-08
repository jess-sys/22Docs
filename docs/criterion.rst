.. Main criterion documentation file

Criterion
============

Criterion is a unit-testing tool that will allow you to test your code efficiently.

Introduction
------------

Setup
~~~~~

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

.. note::
	Please note that the following asserts only work for non-array comparison.
	If you want to compare arrays, please refer to :c:func:`cr_assert_str_eq` or :c:func:`cr_assert_arr_eq`.


.. c:function:: cr_assert_eq(Actual, Reference)
		cr_assert_neq(Actual, Reference)

Passes if and only if ``Actual`` is equal (or not equal, if you are using ``neq``) to ``Reference``.

.. c:function:: cr_assert_lt(Actual, Reference)
		cr_assert_leq(Actual, Reference)

Will pass if ``Actual`` is less than (or less than or equal if you used ``leq``) ``Reference``.

.. c:function:: cr_assert_gt(Actual, Reference)
		cr_assert_geq(Actual, Reference)

Will pass if ``Actual`` is greater than (or greater than or equal if you used ``geq``) ``Reference``.

String asserts
~~~~~~~~~~~~~~

.. note::
	Those functions won't allow you to compare the output of your progam with a given reference string. To do so you must use redirections. Check :c:func:`cr_assert_stdout_eq_str` for more info.


.. c:function:: cr_assert_str_eq(Actual, Reference)
		cr_assert_str_neq(Actual, Reference)


Just like :c:func:`cr_assert_eq`, but will check two strings, character by character.

.. c:function:: cr_assert_empty(Value)
		cr_assert_not_empty(Value)

Will pass if the string is empty (or is not empty is you used ``not_empty``).

.. note::
	There are also ``str_lt``, ``str_gt``, etc... macros that will check the lexicographical values of the two sting given, just like your ``my_strcmp`` would do (if you've done it well :D).

Array asserts
~~~~~~~~~~~~~

.. c:function:: cr_assert_arr_eq(Actual, Expected, Size)
		cr_assert_arr_neq(Actual, Expected, Size)

Compares each element of ``Actual`` with each of ``Expected``.

.. DANGER::
	While not documented in criterion's official documentation, ``Size`` is mandatory, otherwise the test will be marked as fail.

Redirections
~~~~~~~~~~~~

.. WARNING::
	To use the following assertions, you must include ``<criterion/redirect.h>`` along with ``<criterion/criterion.h>``.
	``redirect.h`` allows Criterion to get the content of stdout and stderr and run asserts on it.

.. c:function:: cr_assert_stdout_eq_str(Value)
		cr_assert_stdout_neq_str(Value)

Compares the content of ``stdout`` with ``Value``. This assertion behaves similarly to :c:func:`cr_assert_str_eq`.

.. c:function:: cr_assert_stderr_eq_str(Value)
		cr_assert_stderr_neq_str(Value)

Compares the content of ``stderr`` (a.k.a. "error output") with ``Value``.

Here is a sample usage of this assert.

.. code-block:: c

	int error(void)
	{
		write(2, "error", 5);
		exit(0);
	}

	Test(errors, exit_code)
	{
		error();
		cr_assert_stderr_eq("error", "");
	}


Test options
------------

Options reference
~~~~~~~~~~~~~~~~~

It is possible for you to provide additional parameters to a test. Here is a full list of thos parameters and what you can do with them.

.. c:member:: .init

This parameter takes a function pointer as an argument. Criterion will execute the function just before running the test.

Note that the function pointer should be of type ``void (*)(void)``.

Here is a sample usage of this parameter.

.. code-block:: c

	void my_func(void)
	{
		my_putstr("Here is the beginning of my test\n");
	}

	Test(suite_name, test_name, .init = my_func)
	{
		//tests
	}

.. c:member:: .fini

This parameter takes a function pointer to a function that will be executed after the tests is finished.

It takes the same pointer type as the ``.init`` parameter, and also has the same usage.

.. c:member:: .signal

.. WARNING::
	In order to use this parameter, you must include the header <signal.h>

If a test receives a signal, it will by default be marked as a failure. However, you can expect a test to pass if a special kind of signal is received.

.. code-block:: c

	#include <stddef.h>
	#include <signal.h>
	#include <criterion/criterion.h>

	Test(example, will_fail)
	{
		int *ptr = NULL;
		*ptr = 42;
	}

	Test(example, will_pass, .signal = SIGSEGV)
	{
		int *ptr = NULL;
		*ptr = 42;
	}

In the above example, the first test will fail while the second one will not.

You can find a full list of handled signals by checking signal.h's documentation here : http://pubs.opengroup.org/onlinepubs/009695399/basedefs/signal.h.html.

.. c:member:: .exit_code

By default, Criterion will mark a test as failed if it exits with another exit code than 0.

If you want to test your error handling, you can use the ``.exit_code`` parameter so the test will be marked as passed if the given exit code is found.

Here is a sample usage of this parameter :

.. code-block:: c

	#include <unistd.h>

	int error(void)
	{
		write(2, "error", 5);
		exit(0);
	}

	Test(errors, exit_code, .exit_code = 84)
	{
		error();
		cr_assert_stderr_eq("error", "");
	}

.. c:member:: .disabled

If ``true``, the test will be skipped.

.. c:member:: .description

This parameter must be used in order to give extra definition of the test's purpose, which can be quite helpful if your suite_name and test_name aren't as explicit as you would like.

.. c:member:: .timeout

This parameter takes a ``double``, representing a duration. If your test takes longer than this duration to run, the test will be marked as failed.

Suite configuration
~~~~~~~~~~~~~~~~~~~

If you want to set a test for all of a suite's members (for example, setting the exit code of all your error handling tests), you can, using the ``TestSuite`` macro.

.. code-block:: c

	#include <criterion/criterion.h>

	TestSuite(suite_name, [params...]);

	Test(suite_name, test_1)
	{
		//tests
	}

	Test(suite_name, test_2)
	{
		//other tests
	}

As you can see, you can set some params to all the tests with the same suite_name at once.


.. c:type:: int

This is an awesome type.
