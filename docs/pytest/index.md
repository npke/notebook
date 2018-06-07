
# Pytest

## [Introduction](0-introduction.md)
- [Introduction](0-introduction.md#introduction)
- [Features](0-introduction.md#features)

## [Quickstart](1-quickstart.md)
- [Installation](1-quickstart.md#installation)
- [First test](1-quickstart.md#first-test)
- [Exit codes](1-quickstart.md#exit-codes)
- [Running tests](1-quickstart.md#running-tests)

## [Assertions](2-assertions.md)
- [`assert` statement](2-assertions.md#`assert`-statement)
- [Assertions about expected exceptions](2-assertions.md#assertions-about-expected-exceptions)
- [Defining a assertion comparison](2-assertions.md#defining-a-assertion-comparison)

## [Fixtures](3-fixtures.md)
- [Introduction](3-fixtures.md#introduction)
- [Usage](3-fixtures.md#usage)
- [Scope](3-fixtures.md#scope)
- [Fixture finalization / executing teardown code](3-fixtures.md#fixture-finalization-/-executing-teardown-code)
- [`yield` vs `addfinalizer`](3-fixtures.md#`yield`-vs-`addfinalizer`)
- [Factories as fixtures](3-fixtures.md#factories-as-fixtures)
- [Parametrizing fixtures](3-fixtures.md#parametrizing-fixtures)
- [Overriding fixtures](3-fixtures.md#overriding-fixtures)

## [Marking](4-marking.md)
- [Skipping test functions](4-marking.md#skipping-test-functions)
- [Usage](4-marking.md#usage)
- [XFail: mark test functions as expected to fail](4-marking.md#xfail:-mark-test-functions-as-expected-to-fail)
- [Skip/xfail with parametrize](4-marking.md#skip/xfail-with-parametrize)
- [Parametrizing fixtures and test functions](4-marking.md#parametrizing-fixtures-and-test-functions)
- [`@pytest.mark.parametrize`: parametrizing test functions](4-marking.md#`@pytest.mark.parametrize`:-parametrizing-test-functions)
- [`pytest_generate_tests`](4-marking.md#`pytest_generate_tests`)
