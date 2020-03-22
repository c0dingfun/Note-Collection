SQL Server - LocalDB
====

- Quick rundown of the sqllocaldb command

- find out availability of localdb

```sql
    $ sqllocaldb i
    MSSQLLocalDB
    ProjectsV13
    TBD
```

- create and start an instance of localdb, name "TEST"

```sql

    $ sqllocaldb create "TEST" -s
    LocalDB instance "TEST" created with version 13.1.4001.0.
    LocalDB instance "TEST" started.
```

- check the status/info about a localdb instance "TEST"

```sql

    $ sqllocaldb i TEST
    Name:               TEST
    Version:            13.1.4001.0
    Shared name:
    Owner:              DESKTOP-XXXX\kenny
    Auto-create:        No
    State:              Running
    Last start time:    3/21/2020 5:08:26 PM
    Instance pipe name: np:\\.\pipe\LOCALDB#869FE2D6\tsql\query
```

- connection string for localdb instance "TEST"

```
    "LocalDB" = "Server=(localdb)\TEST; Integrated Security=true;"
```

- stop a localdb instance "TEST"

```sql
    $ sqllocaldb stop TEST -i
    LocalDB instance "TEST" stopped.

    $ sqllocaldb i TEST
    Name:               TEST
    Version:            13.1.4001.0
    Shared name:
    Owner:              DESKTOP-xxxx\kenny
    Auto-create:        No
    State:              Stopped
    Last start time:    3/21/2020 5:08:26 PM
    Instance pipe name:
```

- delete a localdb instance, "TEST"

```sql
    $ sqllocaldb delete TEST
    LocalDB instance "TEST" deleted.
```

- localdb help

```sql
    $ sqllocaldb -?
    Microsoft (R) SQL Server Express LocalDB Command Line Tool
    Version 13.0.1601.5
    Copyright (c) Microsoft Corporation.  All rights reserved.

    Usage: SqlLocalDB operation [parameters...]

    Operations:

    -?
        Prints this information

    create|c ["instance name" [version-number] [-s]]
        Creates a new LocalDB instance with a specified name and version
        If the [version-number] parameter is omitted, it defaults to the
        latest LocalDB version installed in the system.
        -s starts the new LocalDB instance after it's created

    delete|d ["instance name"]
        Deletes the LocalDB instance with the specified name

    start|s ["instance name"]
        Starts the LocalDB instance with the specified name

    stop|p ["instance name" [-i|-k]]
        Stops the LocalDB instance with the specified name,
        after current queries finish
        -i request LocalDB instance shutdown with NOWAIT option
        -k kills LocalDB instance process without contacting it

    share|h ["owner SID or account"] "private name" "shared name"
        Shares the specified private instance using the specified shared name.
        If the user SID or account name is omitted, it defaults to current user.

    unshare|u ["shared name"]
        Stops the sharing of the specified shared LocalDB instance.

    info|i
        Lists all existing LocalDB instances owned by the current user
        and all shared LocalDB instances.

    info|i "instance name"
        Prints the information about the specified LocalDB instance.

    versions|v
        Lists all LocalDB versions installed on the computer.

    trace|t on|off
        Turns tracing on and off

    SqlLocalDB treats spaces as delimiters. It is necessary to surround
    instance names that contain spaces and special characters with quotes.
    For example:
    SqlLocalDB create "My LocalDB Instance"

    The instance name can sometimes be omitted, as indicated above, or
    specified as "". In this case, the reference is to the default LocalDB
    instance "MSSQLLocalDB".

```
