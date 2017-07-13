# SKILL Tools


This is my personal set of tools written for working with SKILL in Cadence virtuoso.

The main feature of interest is the unit testing framework in the folder `qtest`.

## Namespaces used
### `qub`
Currently contains all code other than the testing framework: a catch-all namespace.

### `qtest`
Contains all code relating to unit testing.

## Unit Testing

The unit testing framework is modelled after [Python's `unittest` module](https://docs.python.org/3/library/unittest.html) and provides a basic set of assertions for testing your code.

### Example Test

```lisp
(qtest::runSuites
  (qtest::TestSuite ((f qub:::join_lists))
    (qtest::TestCase join_three_lists
      (qtest::assertEqual (list "X" 2 3 4 5 6)
                           (f (list (list 1 2) (list 3 4) (list 5 6))))))
  (qtest::TestSuite ((f qub::flatten_list))
    (qtest::TestCase normal_use
      (qtest::assertEqual (list 1 2 3 4 5 6 7)
                           (f (list 1 2 (list 3 (list 4 (list 5 6) 7))))))))
```

```lisp
(load "./code/src/std/test_lists.ils")
1 of 2 tests passed (1 failures) (0 skipped) (0 expected failures)
Test: join_three_lists
Result: Fail
 Message: No msg.
 Inputs: (list("X" 2 3 4 5 6) (f list(list(1 2) list(3 4) list(5 6))))
 Evaluated Inputs: (("X" 2 3 4 5 6) (1 2 3 4 5 6))
``` 

### Main Functions and Macros

## `runAllTests`

This takes a relative folder path and loads all modules prefixed with test_

## `TestCase`

This represents a single test. It can contain any code but it must return the result of an assertion function. The test name is contained in a list with the function object so tests don't pollute the top level namespace.

## `TestSuite`

A collection of test cases. The function just creates a list of tests. A suite should be used to test a single 

## `runSuites`

The test suites should be written in the body of this macro. When the module file is loaded in cadence the tests are initialised and the results are printed in the CIW.

## Importing the module

Simply run the SKILL `load` function on `init.ils` at the root of the repo and all other modules will be imported.

## Folders

### `geometry`

This is a side project under development and should be ignored.

### `qtest`

All code for the unit testing.

### `std`

My own "standard library" of useful functions.
