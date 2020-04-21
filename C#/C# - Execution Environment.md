C# - .Net Environment
====

Compile Time and Runtime (CLR)
----

- Compile C# source to IL -- Manged Code

- .Net Assembly contains Assembly Manifest and IL (Managed Code)---.dll (library) or .exe (application executable)

- Assembly manifest can be modified using attributes

- CLR (Common Language Runtime) loads the Assemblies (ILs) and use JIT Compilation ILs to Platform specific native code (Machine Code)

- CLR provides Garbage collection.

ILDASM (IL Disassembler) ILASM (IL Assembler)
----

- Assembly manifest can be modified using attributes

- Use ILDASM (Disassembler) to peek at the assembly manifest and IL, and can export manifest and IL to a text file.

- Use ILASM (Assembler) to reconstruct an assembly from a text file that contain assembly manifest and IL.

Strong Naming an Assembly
----

- .Net Assemblies can be classified into 2 types

  - Weak Named Assemblies
  - Strong Named Assemblies

- An Assembly Name consists of 4 Parts

1. Simple Text Name
2. Version Number
3. Culture Information (without it the assembly is language neutral)
4. Public Key Token

- We use AssemblyVersion attribute (in Project->Properties->AssemblyInfo.cs) to specify the Assembly version. 

- The default is 1.0.0.0, which consists of 4 parts

1. Major Version
2. Minor Version
3. Build Number
4. Revision Number

```csharp
using System.Reflection;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

// General Information about an assembly is controlled through the following
// set of attributes. Change these attribute values to modify the information
// associated with an assembly.
[assembly: AssemblyTitle("ShortestPath")]
[assembly: AssemblyDescription("This is Test Description")]
[assembly: AssemblyConfiguration("No Configuration")]
[assembly: AssemblyCompany("Kenny Co.")]
[assembly: AssemblyProduct("ShortestPath")]
[assembly: AssemblyCopyright("Copyright © 2020 April 17")]
[assembly: AssemblyTrademark("OmniB")]
[assembly: AssemblyCulture("en-US")] // <<< specify the Culture!

// Setting ComVisible to false makes the types in this assembly not visible
// to COM components.  If you need to access a type in this assembly from
// COM, set the ComVisible attribute to true on that type.
[assembly: ComVisible(false)]

// The following GUID is for the ID of the typelib if this project is exposed to COM
[assembly: Guid("45a04dd8-b3c3-4293-ae98-46314d1c6ed9")]

// Version information for an assembly consists of the following four values:
//
//      Major Version
//      Minor Version
//      Build Number
//      Revision
//
// You can specify all the values or you can default the Build and Revision Numbers
// by using the '*' as shown below:
// [assembly: AssemblyVersion("1.0.*")]
[assembly: AssemblyVersion("1.0.0.0")]
[assembly: AssemblyFileVersion("1.0.0.0")]
```
