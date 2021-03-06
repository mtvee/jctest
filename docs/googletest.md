---
nav_order: 4
---

# jc_test vs googletest

## Design

This testing framework software was written to speed up the compilation
times of a particular software.

It may very well be the case that there are nuances to the supported test classes and functions
that are intentionally/unintentionally left out or simply work slightly differently.

While `jc_test` is designed to be a replacement for gtest,
the base classes are named differently, so you need some setup to make it work.

The major goal with this software was to see if it was possible to decrease the compilation
times when compiling tests. See [the benchmarks](./benchmarks.md) for the results.

A secondary goal was to keep the code reasonably small, in order to make it into a single header only library.
As such, it wouldn't be necessary to cross compile the testing framework for many different target platforms.


## Base classes

The classes correspond like so:

    class ::testing::Test  -> class jc_test_base_class

    template<typename T>                    template<typename T>
    public ::testing::TestWithParam<T> ->   class jc_test_params_class

    ::testing::ValuesIn -> jc_test_values_in

    ::testing::Types<T1>                -> jc_test_type1<T1>
    ::testing::Types<T1, T2>            -> jc_test_type2<T1, T2>
    ::testing::Types<T1, T2, T3>        -> jc_test_type3<T1, T2, T3>
    ::testing::Types<T1, T2, T3, T4>    -> jc_test_type4<T1, T2, T3, T4>

## ASSERT_DEATH

`jc_test` is using a signal handler to catch errors such as SIGABRT.
The application is not forked, and the output from assert() is output in the log.

It also does not check if it's supported, hence the rename from `ASSERT_DEATH_IF_SUPPORTED`

## INSTANTIATE_TYPED_TEST_CASE

Instead of supporting the TYPED_TEST_CASE, with heavy templates, there's instead `INSTANTIATE_TYPED_TEST_CASE`. It works the same way but simply takes one type at a time.

The name INSTANTIATE_TYPED_TEST_CASE is also more in line with the name and design of INSTANTIATE_TEST_CASE.
