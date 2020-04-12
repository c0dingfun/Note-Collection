String Interpolation in Unary Expression
====

> https://stackoverflow.com/questions/31844058/how-to-use-the-ternary-operator-inside-an-interpolated-string

unary expression in string interpolation (C#) 

According to the documentation:

The structure of an interpolated string is as follows:

`$ "{ <interpolation-expression> <optional-comma-field-width> <optional-colon-format> }"`

The problem is that the colon is used to denote formatting, like

`Console.WriteLine($"Time in hours is {hours:hh}")`

So, the tl;dr answer is: wrap the conditional in parenthesis.

`var result = $"descending? {(isDescending ? "yes" : "no")}";`

See the `()`?