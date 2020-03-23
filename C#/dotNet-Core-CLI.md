Useful .NET CLI (dotnet) commands
====

|Command | Description | Run from|
|--| --- | ---|
|dotnet restore |  Restores the NuGet packages for all the projects in the solution. |    Solution folder|
|dotnet build   |Builds all the projects in the solution. To build in Release mode, use the -c switch.    | Solution folder|
|dotnet run  | Runs the project in the current folder. Use during development to run your app.  |   Project folder|
|dotnet publish -c Release –o `<Folder>`   |Publishes the project to the provided folder. This copies all the required files to the provided output folder so that it can be deployed.   |  Project folder|
|dotnet test  | Builds the project and executes any unit tests found in the project. Requires the .NET Test SDK and a testing framework adapter (see chapter 20).  |   Project folder|
|dotnet add package `<Name>` |  Installs the NuGet package with the provided name into the current project. Optionally, specify a package version, for example -v 2.1.0.  |   Project folder|
|dotnet new --list   |Views all the installed templates for creating ASP.NET Core apps, libraries, test projects, solution files, and much more.   |  Anywhere|
|dotnet new --install `<PackageName>`  |Installs a new template from the provided NuGet package name. For example, dotnet new --install "NetEscapades.Templates::*".  |   Anywhere|
|dotnet new `<Template> –o <Folder> -n <NewName> `  |Creates a new item from the provided template, specifying the folder in which to place the item, and the name for the item. eg: _dotnet new webapp -n TestPage -o Pages_ |Anywhere, empty folder for new projects|
|dotnet ef --help |  Displays help for managing your database and migrations using EF Core. Requires the EF Core tools (see chapter 12).  |  Project folder|
|dotnet ef migrations add `<Name>` |   Adds a new EF Core migration to the project with the provided name. |    Project folder|
|dotnet ef database update  | Applies pending EF Core migrations to a database. Warning—this will modify your database!|