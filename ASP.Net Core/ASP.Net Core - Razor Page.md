---
title: ASP.Net Core - Razor Page 
category: Notes
uid: NOTE-COLLECTION/ASP.Net Core - Razor Page.md
---
# [ASP.Net Core - Razor Page](https://github.com/dotnet/AspNetCore.Docs.git "Microsoft Example Source")

What is Razor Pages?
----

- Introduced in ASP.Net Core 2.0

- Designed to make coding page scenarios much simpler than MVC pattern.

- Typical page scenarios are <form>, raw HTML, or using Razor @Html.BeginForm()

- And functionality of the page, from needed properties to methods, are accomplished by the View (.cshtml) and the PageModel (the base class of the ViewModel) (.cshtml.cs); there is no need to have a Controller, a Razor View...

- Use a code style that more like M-V-VM.

- There are many conventions used:

    + Abc (view) would matches the AbcModel : Page Model (the ViewModel)

- Razor Pages is built on top of ASP.Net Core MVC, not beside it, so routing, ModelState, DataBinding, etc.) works the same way.

- Note: ASP.Net Core Web Application is an umbrella for following project templates:

    1) (Web) API
    2) Web Application (Razor Pages)
    3) Wab Application (MVC)
    4) Empty 

    + For Blazor apparently use "Blazor App" template

Folder Structure: Razor 
----

- For a Razor project, it has a _**Pages**_ folder

Notable Razor Pages Summary
----

- Anti-forgery tokens are automatically included on every Razor Page.

- The PageModel is actually the ViewModel as in MVC

- Razor Pages have greater cohesiveness than normal MVC sites, because all the data related to a page is on a file that's physically related to that page (whereas in MVC you could have a Controller, a ViewModel, a View, and a Model of some kind that are all pieces of the same action).

- Razor Pages are a wonderful tool in our developer toolbox for building page-focused applications
