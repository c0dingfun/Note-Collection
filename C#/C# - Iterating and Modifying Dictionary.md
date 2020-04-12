Careful with iteration and modifying dictionary in a foreach() loop!!!
====

Use ConcurrentDictionary

if we iterating a dictionary while modifying it -- adding new items (new key) or changing its value, it can ran into exception! Because while modifying the dictionary, the previous obtain iterator in foreach loop is no longer valid. If continue to use the iterator, exception will be thrown.

Unhandled exception: System.InvalidOperatoinExceptoin: Collection was modified; enumeration operation may not execute.

Solution
----

to avoid the exception, we can use ConcurrentDictionary, instead.
to avoid the exception, we can get the keys first and loop over it (if the
key list not changing).
to avoid the exception, we can lock up the dictionary while looping, but
is it less elegant than using above two methods.

Basically, when looping through KeyValuePairs, we have problem. when looping through keys, we don't have problem.
