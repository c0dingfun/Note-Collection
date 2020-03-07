# [REST API Rules - URI Design](https://dzone.com/articles/7-rules-for-rest-api-uri-design-1)

Seven simple rules will help you write readable, conflict-free URIs that communicate all necessary resource information. 

## URIs

REST APIs use Uniform Resource Identifiers (URIs) to address resources.

[Clear URI]: http://api.example.com/louvre/leonardo-da-vinci/mona-lisa

[Hard to understand]: http://api.example.com/68dd0-a9d3-11e0-9f1c-0800200c9a66

URI = scheme "://" authority "/" path [ "?" query ] [ "#" fragment ]

## Rule #1

A trailing forward slash (/) should not be included in URIs.

## Rule #2

Forward slash separator (/) must be used to indicate a **hierarchical relationship**.

The forward slash (/) character is used in the path portion of the URI to indicate a hierarchical relationship between resources. For example: http://api.canvas.com/shapes/polygons/quadrilaterals/squares

## Rule 3

Hyphens (-) should be used to improve the readability of URIs.

For example: http://api.example.com/blogs/guy-levin/posts/this-is-my-first-post

## Rule 4

Underscores (_) should not be used in URIs.

## Rule 5

Lowercase letters should be preferred in URI paths.

For example: 
1) http://api.example.com/my-folder/my-doc
2) HTTP://API.EXAMPLE.COM/my-folder/my-doc 

3) http://api.example.com/My-Folder/my-doc
This URI is not the same as URIs 1 and 2, which may cause unnecessary confusion.

## Rule 6

File extensions should not be included in URIs.

## Rule 7

Should the endpoint name be singular or plural?

The keep-it-simple rule applies here. Although your inner grammatician will tell you it's wrong to describe a single instance of a resource using a plural, the pragmatic answer is to keep the URI format consistent and always use a plural.