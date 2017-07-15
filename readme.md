# SKILL Tools

Tools to ease working with SKILL and SKILL++ in Cadence Virtuoso. The main feature of interest is the unit testing framework in the folder `qtest`.

## Project Structure

| Folder | Namespace | Purpose
|---|---|---|
|`geometry` | `qub` | A side project currently under development and should be ignored.
|`qtest` | `qtest` | All code for the unit testing.
| `std` | `qub` | A "standard library" of useful functions.

## Importing the project

Run the SKILL `load` function on `init.ils` at the root of the repo and all other modules will be imported.

## Unit Testing

The unit testing framework is modelled after [Python's `unittest` module](https://docs.python.org/3/library/unittest.html) and provides a basic set of assertions for testing your code.

### Example Test

The test is run by calling `load` on the file containing the test.

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

## Main Functions and Macros

### `qtest::runAllTests`

| Parameter(s)  |  Side Effect(s) |
|-----------------|----------------|
| Directory (`string`)|Calls `load` on all files prefixed with `test_` in the directory |

Takes a folder path and loads all modules prefixed with test_.

### `qtest::TestCase`

| Parameters(s)| Keyword Parameters  | Output(s)
|---|---|---|
| Test Name (unquoted symbol) |`skip` (bool)| List of the test name symbol and the function object
|body|  `expect_fail` (bool)

Represents a single test. Can contain any code but must return the result of an assertion function. The test name is contained in a list with the function object so tests don't pollute the top level namespace.

#### Skipping a test

To mark a test to be skipped, set the `skip` keyword argument to true.

#### Marking a test that you expect to fail

Set the `expect_fail` keyword argument to true. If the test passes it will count as a pass but if it fail it will not be recorded as a fail.

### `qtest::TestSuite`

| Parameter(s)  | Output(s) |
|---|---|
| Test Cases | List containing lists returned by `qtest::TestCase` |

A collection of test cases. A suite should be used to test a single function or method.

### `qtest::runSuites`

| Parameter(s)  | Side Effect(s) |
|-----------------|----------------|
| Any number of Test Suites | Prints the results of the tests |

Test suites should be written in the body of this macro. When the module file is loaded in Virtuoso, the tests are initialised and the results are printed in the CIW.

## Assertions

Each assertion takes a keyword argument `msg` which is printed if the test fails.

### `qtest::assertEqual`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| a | msg (`string`) | `qtest::Result` |
| b 



Checks if two objects are equal. Uses the `qub::equal` method to allow you to implement equality for your own objects as well as numbers, strings etc.

### `qtest::assertNotEqual`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| a | msg (`string`) | `qtest::Result` |
| b 


Checks if two objects are not equal. Uses the `qub::notEqual` method to allow you to implement equality for your own objects as well as numbers, strings etc.

### `qtest::assertTrue`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| a | msg (`string`) | `qtest::Result` |

Checks that the argument is true.

### `qtest::assertNil`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| a | msg (`string`) | `qtest::Result` |

Checks that the argument is `nil`.

### `qtest::assertEq`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| a | msg (`string`) | `qtest::Result` |
| b 


Both arguments are the same object.

### `qtest::assertNotEq`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| a | msg (`string`) | `qtest::Result` |
| b 

The arguments are different objects.

### `qtest::assertMember`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| value | msg (`string`) | `qtest::Result` |
| list 

The object is a member of the list.

### `qtest::assertNotMember`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| value | msg (`string`) | `qtest::Result` |
| list



The object is not a member of the list.

### `qtest::assertIsInstance`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| Object | msg (`string`) | `qtest::Result` |
| Quoted Class


The object is an instance of the class.

### `qtest::assertNotIsInstance`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| Object | msg (`string`) | `qtest::Result` |
| Quoted Class


The object is not an instance of the class.

### `qtest::assertRaises`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| Function Call | msg (`string`) | `qtest::Result` |

Checks that the expression raises an error using `error`. 
It may not catch other errors within the expression.

### `qtest::assertAlmostEqual`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| a | msg (`string`) | `qtest::Result` |
| b 


Checks that two floats are almost equal. Uses `qub::almostEqual` which is a port of Python's [`math.isclose`](https://docs.python.org/3/library/math.html#math.isclose) and uses the same optional arguments.

### `qtest::assertNotAlmostEqual`

| Parameter(s)  | Keyword Parameters | Output(s) |
|-----------------|-----------|---|
| a | msg (`string`) | `qtest::Result` |
| b 

Checks that two floats are not almost equal.

## Standard Library Functions and Macros

### `qub::checkType`

| Parameters | Side Effects |
| --- | --- |
| Object | Throws an error if the object is of the expected type.
| Quoted Class Name

For implementing run-time type checking in functions.

### `qub::equal`

| Parameters | Result |
| --- | --- |
| a | Bool
| b

A generic function for implementing equality for your own types.

### `qub::nequal`

| Parameters | Result |
| --- | --- |
| a | Bool
| b

A generic function for implementing inequality for your own types.

### `qub::allEqual`

| Parameters | Result |
| --- | --- |
| `@rest values` | bool

Checks if all values are equal. It uses `qub::equal` for checking equality.

### `qub::listFileRec`

| Parameter | Result |
|---|---|
| Path (string) | List of files in the directory |

Recursively searches the path for all files. It ignores all dotted directories.

### `qub::foldl`

| Paramater | Result |
|---|---|
| fn(accumulator value) | The "folded" value
| Initial Value
| List of Values

The [HaskellWiki](https://wiki.haskell.org/Fold) can probably explain this better than I could.

### `qub::foldr`

The same as `qub::foldl` but it calls `reverse` on the list before it processes it.

### `qub::sum`

Sums a list of numbers

### `qub:::lcmp`

An implementation of python's list comprehensions.
The predicate is optional.

```lisp
>>>(qub:::lcmp (times x 2) for x in (list 1 2 3 4) if (evenp x))
(4 8)
```

### `qub::joinLists`

| Paramater | Result |
| --- | --- |
| List of lists | A single list of the values within the input lists |

### `qub::range`

| Keyword parameters | Result |
| --- | --- |
| start | List of integers 
| stop
| step

A port of python's `range` function.

```lisp
>>>(qub::range ?start 0 ?stop 20 ?step 3)
(0 3 6 9 12
    15 18
)
```

### `qub::almostEqual`

| Parameters | Keyword Parameters | Result |
|---|---|---|
| a |`rel_tol`| Bool
| b | `abs_tol`

Checks that two values are almost equal. This is a generic function and can be applied to your own types. The method specialised on numbers is a port of Python's [`math.isclose`](https://docs.python.org/3/library/math.html#math.isclose) and uses the same optional arguments.

`rel_tol` is the relative tolerance. 0.05 would be a 5% relative tolerance.
`abs_tol` is the absolute tolerance. 



### `qub::startsWith`

| Parameters | Result |
| ---|---|
| string | Bool
| prefix (string)

Checks if a string begins with a particular prefix
