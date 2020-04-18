Remote Debugging using Visual Studio
====

[Ref](https://docs.microsoft.com/en-us/visualstudio/debugger/remote-debugging?view=vs-2019)

[Ref](https://www.youtube.com/watch?v=UTRyfRmqJTk&list=PLQPgqnK4E_gp1iWORGo_9MY3BZpYkROv1&index=10)

Download & Install the Remote Tools (on the Remote Machine)
----

- On the remote device or server that you want to debug on, rather than the Visual Studio machine, download and install the remote tools for your version of the Visual Studio.

Set the Remote Debugger (on the Remote Machine)
----

- Start the Remote Debugger from Start Menu


IIS hosting ASP.Net
----

- Install the WebDeploy on the remote machine, so that we can do publish directly from VS (on local machine)

- run "iisreset" if the process is automatically shutdown when idle too long


Dotnet Core
----

- Install ASP.Net core server hosting bundle, because asp.net core use **Kestrel** to host the application, ans IIS (or Nginx, Apache) is just used as a reverse proxy.

- For Dotnet Core, download and install dotnet core runtime on the Remote Machine

Attach the VS to the remote Machine's Process
----

- Just do the regular Debugging session by attaching the VS to the process, in this case, it is the process on the remote machine.