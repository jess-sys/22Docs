.. Documentation for the upcoming Criterion assert API

Criterion - Upcoming assert API
===============================

Introduction
------------

In this tutorial we will take a look at Criterion's upcoming assert API. If you're not familiar with Criterion,
you may start with the first part of our Criterion documentation.

Setup
~~~~~

Before using the new assert API, it is necessary to download and build the development version of Criterion, as
we are working with unreleased functionality.

Start by downloading the code with this command :

.. code-block:: bash

    $ git clone --recursive git@github.com/Snaipe/Criterion.git

.. NOTE::
    Take note of the `--recursive` option, it is necessary to compile the code.

Once the code is downloaded, let's build it.

.. code-block:: bash

    $ mkdir build
    $ cd build
    $ cmake ..
    $ cmake --build .
    $ sudo make install

Now, to use any of the features described in this page, you will need to include the following headers to your test files.

.. code-block:: c

    #include <criterion/criterion.h>
    #include <criterion/new/assert.h>

Assertions
----------

With the old Criterion, there was a multitude of assertion functions, covering a lot of cases.
This led to a few useless repetitions, such as `cr_assert_eq`, `cr_assert_str_eq`, `cr_assert_arr_eq`...
The new API is much more straightforward, there are only 5 assertion functions :

Reference
~~~~~~~~~

.. c:function:: cr_assert(``Criterion``)

Passes if ``Criterion`` is true, abort if not.

.. c:function:: cr_expect(``Criterion``)

Passes if ``Criterion`` is true, fails if not.

.. NOTE::
    The difference between `cr_assert` and `cr_expect` is the behavior when the test fails. An assert will stop the test right away
    while an expect will continue until the end of the test.

.. c:function:: cr_fail()

Marks the test as failed. Can be used when testing manually with conditions for example.

.. c:function:: cr_fatal()

Marks the test as failed then abort.

.. c:function:: cr_skip()

Marks the test as skipped then abort.

.. NOTE::
    All of those macros can take an optional printf-like string which will be printed if the test fails (see below for an example).

Example
~~~~~~~

.. code-block:: c

    Test(my_suite, my_test) {
        FILE *myfile = fopen("myfile.txt", "r");
        if (myfile == NULL) {
            cr_fatal("Test mytest failed, file could not be opened : %s\n", strerror(errno));
        } else {
            // Do something...
        }
    }

Criteria
--------

As you can see, there aren't any macros that compare values. This new API does not expect you to write the comparison functions yourself,
you may use a set of predefined Criteria (or _Criterions_) which are the recommended parameters of `cr_assert` and `cr_expect`.

Reference
~~~~~~~~~

Logical Criteria
~~~~~~~~~~~~~~~~

Logical criteria are simple helpers to test multiple criteria at once.

.. c:function:: not(``Criterion``)

Evaluates to `!Criterion`.

.. c:function:: all(`...`)

Takes a sequence of Criteria as parameters. Will be true if all Criteria given are true. (equivalent to an &&)

.. c:function:: any(`...`)

Takes a sequence of Criteria as parameters. Will be true if any Criteria given is true. (equivalent to an ||)

.. c:function:: none(`...`)

A combination of a `not` over each criteria and an `all`.

Tagged Criteria
~~~~~~~~~~~~~~~

Those are the real useful testing macros.

.. note::
    Do not worry with the following `Tag` parameters, the complete list of tags are described in the nex section.

General Purpose
_______________

.. c:function:: eq(``Tag``, ``Actual``, ``Expected``)

Tests if Actual is equal to Expected.

.. NOTE::
    This function may only use the operator `==` if the tag specifies numeric values. It is also able to the the equality between strings if given the tag `str`.

.. c:function:: ne(``Tag``, ``Actual``, ``Expected``)

Tests if Actual is not equal to Expected.

.. c:function:: lt(``Tag``, ``Actual``, ``Expected``)

Tests if Actual is less than Expected.

.. c:function:: le(``Tag``, ``Actual``, ``Expected``)

Tests if Actual is less than or equal to Expected.

.. c:function:: gt(``Tag``, ``Actual``, ``Expected``)

Tests if Actual is greater than Expected.

.. c:function:: ge(``Tag``, ``Actual``, ``Expected``)

Tests if Actual is greater than or equal to Expected.

Floating point
______________

.. warning::
    The following criteria only work with the following tags : `flt`, `dbl` and `ldbl`.

Epsilon
:::::::

.. warning::
    This method of comparison is more accurate when comparing two IEEE 754 floating point values that are near zero. When comparing against values that aren't near zero, please use ieee_ulp_eq instead.

    It is recommended to have Epsilon be equal to a small multiple of the type epsilon (FLT_EPSILON, DBL_EPSILON, LDBL_EPSILON) and the input parameters.


.. c:function:: epsilon_eq(``Tag``, ``Actual``, ``Expected``, ``Epsilon``)

Tests if Actual is almost equal to Expected, the difference being the Epsilon value.

.. c:function:: epsilon_ne(``Tag``, ``Actual``, ``Expected``, ``Epsilon``)

Tests if Actual is different to Expected, the difference being more than the Epsilon value.

ULP
:::

.. warning::
    This method of comparison is more accurate when comparing two IEEE 754 floating point values when Expected is non-zero. When comparing against zero, please use epsilon_ne instead.

    A good general-purpose value for Ulp is 4.

.. c:function:: epsilon_eq(``Tag``, ``Actual``, ``Expected``, ``Epsilon``)

Tests if Actual is almost equal to Expected, by being between Ulp units from each other.

.. c:function:: epsilon_ne(``Tag``, ``Actual``, ``Expected``, ``Epsilon``)

Tests if Actual is different to Expected, the difference being more than Ulp units from each other.

Example
~~~~~~~

All the following tests pass :

.. code-block:: c

    Test(strings, eq) {
        cr_assert(eq(str, "Hello", "Hello"));
    }
    Test(strings, ne) {
        cr_assert(ne(str, "Hello", "Hella"));
    }
    Test(integers, eq) {
        cr_assert(eq(int, 8, 8));
    }
    Test(logical, all) {
        cr_assert(all(eq(int, 8, 8), eq(str, "Hello", "Hello")));
    }

Tags
----

The tags are Criterion-defined macros that represent standard C types.

Predefined Tags
~~~~~~~~~~~~~~~

Here is the complete list of all predefined tags :

.. c:macro:: int8_t i8

.. c:macro:: int16_t i16

.. c:macro:: int32_t i32

.. c:macro:: int64_t i64

.. c:macro:: uint8_t u8

.. c:macro:: uint16_t u16

.. c:macro:: uint32_t u32

.. c:macro:: uint64_t u64

.. c:macro:: size_t sz

.. c:macro:: void * ptr

.. c:macro:: intptr_t iptr

.. c:macro:: uintptr_t uptr

.. c:macro:: char chr

.. c:macro:: int int

.. c:macro:: unsigned int uint

.. c:macro:: long long

.. c:macro:: unsigned long ulong

.. c:macro::long long llong

.. c:macro:: unsigned long long ullong

.. c:macro:: float flt

.. c:macro:: double dbl

.. c:macro:: long double ldbl

.. c:macro::complex float cx_flt

.. c:macro:: complex double cx_dbl

.. c:macro:: complex long double cx_ldbl

.. c:macro::struct cr_mem mem

See below for details about the implementation of this structure.

.. c:macro:: const char * str

.. c:macro:: const wchar_t * wcs

String of wide characters

.. c:macro:: const TCHAR * tcs

Windows character string.

Mem struct
~~~~~~~~~~

Here is the definition of the cr_mem structure :

.. c:type:: struct cr_mem

.. c:member:: const void * data

data is a pointer to the data to test

.. c:member:: size_t size

size is the size of data to test.

User-Defined Type
~~~~~~~~~~~~~~~~~

You can use the following macro to use you own types as Tags :

.. c:function:: type(``UserType``)

You may then use you type the same way as any other tag :

.. code-block:: c

    cr_assert(eq(type(my_type), var1, var2));

However there are some functions to implement in order to use this type in your code.

.. warning::

    Due to implementation restrictions, UserType must either be a structure, an union, an enum, or a typedef.

    For instance, these are fine:

    .. code-block::
        type(foo)
        type(struct foo)

    and these are not:

    .. code-block::
        type(foo *)
        type(int (&foo)(void))

    in these cases, use a typedef to alias those types to a single-word token.

In any case
___________

The type must be printable, and thus should implement a "to-string" operation. These functions are mandatory in any case.

C
:::

.. c:function:: char *cr_user_<type>_tostr(const <type> *val);


For example if you have a `character` type, you must implement the `char *cr_mem_character_tostr(const character *val);`

C++
:::

.. cpp:function:: std::ostream &operator<<(std::ostream &os, const <type> &val);

eq, ne, le, ge
______________

These functions are mandatory in order to use the operators `eq`, `ne`, `le` or `ge` with your type.
They should return 1 (or true) if the two parameters are equal, 0 (or false) otherwise.

C
:::

.. c:function:: int cr_user_<type>_eq(const <type> *lhs, const <type> *rhs);

C++
:::

.. cpp:function:: bool operator==(const <type> &lhs, const <type> &rhs);

lt, gt, le, ge
______________

These functions are mandatory in order to use the operators `lt`, `gt`, `le` or `ge` with your type.
They should return 1 (or true) if the first parameter is less than the second, 0 (or false) otherwise.

C
:::

.. c:function:: int cr_user_<type>_lt(const <type> *lhs, const <type> *rhs);

C++
:::

.. cpp:function:: bool operator<(const <type> &lhs, const <type> &rhs);

User-defined type example
-------------------------

.. code-block:: c

    #include <stdio.h>
    #include <criterion/criterion.h>
    #include <criterion/new/assert.h>


    typedef struct vector3d {
        int x;
        int y;
        int z;
    } vector3d;

    /*
        Defines the string representation of a vector3d
    */
    char *cr_user_vector3d_tostr(const vector3d *val)
    {
        char *str = malloc(sizeof(char) * (12 + 3 * 9));

        if (str == NULL) {
            return "";
        }
        sprintf(str, "X:%d; Y:%d: Z:%d\n", val->x, val->y, val->z);
        return str;
    }

    /*
        Defines an equality between two vector3d
    */
    int cr_user_vector3d_eq(const vector3d *lhs, const vector3d *rhs)
    {
        if (lhs->x == rhs->x && lhs->y == rhs->y && lhs->z == rhs->z) {
            return 1;
        }
        return 0;
    }

    /*
        Creates a new vector with predefined values
    */
    vector3d *create_vector(void)
    {
        vector3d *vect = malloc(sizeof(vector3d));

        vect->x = 8;
        vect->y = 7;
        vect->z = 2;
        return vect;
    }

    Test(usertype, test) {
        vector3d *test_vect = malloc(sizeof(vector3d));
        vector3d *vect = create_vector();

        test_vect->x = 8;
        test_vect->y = 7;
        test_vect->z = 2;

        cr_assert(eq(type(vector3d), *vect, *test_vect));
    }