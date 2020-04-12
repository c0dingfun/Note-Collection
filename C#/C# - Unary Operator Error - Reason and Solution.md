?: unary operator error -- reason and solution
====

> https://stackoverflow.com/questions/18260528/type-of-conditional-expression-cannot-be-determined-because-there-is-no-implicit
> https://stackoverflow.com/questions/858080/nullable-types-and-the-ternary-operator-why-is-10-null-forbidden?answertab=active#tab-top

The compiler first tries to evaluate the right-hand expression:

GetBoolValue() ? 10 : null

The 10 is an int literal (not int?) and null is, well, null. 

There's no implicit conversion between those two, hence the error message.

If you change the right-hand expression to one of the following then it compiles because there are an implicit conversion between int? and null (#1) between int and int? (#2, #3).

GetBoolValue() ? (int?)10 : null    // #1
GetBoolValue() ? 10 : (int?)null    // #2
GetBoolValue() ? 10 : default(int?) // #3
