# SKILL Tools

Tools to ease working with SKILL and SKILL++ in Cadence Virtuoso.

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

## `qtest::runAllTests`

Takes a relative folder path and loads all modules prefixed with test_.

## `qtest::TestCase`

Represents a single test. Can contain any code but must return the result of an assertion function. The test name is contained in a list with the function object so tests don't pollute the top level namespace.

### Skipping a test

To mark a test to be skipped, set the `skip` keyword argument to true.

### Marking a test that you expect to fail

Set the `expect_fail` keyword argument to true. If the test passes it will count as a pass but if it fail it will not be recorded as a fail.

## `qtest::TestSuite`

A collection of test cases. A suite should be used to test a single function or method.

## `qtest::runSuites`

Test suites should be written in the body of this macro. When the module file is loaded in Virtuoso, the tests are initialised and the results are printed in the CIW.

# Assertions

## `qtest::assertEqual`

Checks if two objects are equal. Uses the `qub::equal` method to allow you to implement equality for your own objects as well as numbers, strings etc.

## `qtest::assertNotEqual`

Checks if two objects are not equal. Uses the `qub::notEqual` method to allow you to implement equality for your own objects as well as numbers, strings etc.

## `qtest::assertTrue`

## `qtest::assertNil`

## `qtest::assertTrue`

## `qtest::assertEq`

## `qtest::assertNotEq`

## `qtest::assertMember`

## `qtest::assertNotMember`

## `qtest::assertIsInstance`

## `qtest::assertNotIsInstance`

## `qtest::assertRaises`

## `qtest::assertAlmostEqual`

## `qtest::assertNotAlmostEqual`


## Importing the module

Run the SKILL `load` function on `init.ils` at the root of the repo and all other modules will be imported.

## Folders

### `geometry`

A side project currently under development and should be ignored.

### `qtest`

All code for the unit testing.

### `std`

A "standard library" of useful functions.
