Powershell
====

- Get Processes

```powershell
get-process | select-object name, fileversion, productversion, company
```

```
firefox              75.0                                   75.0            Mozilla Corporation
fontdrvhost
fontdrvhost
git-bash
gvim                 8, 1, 282, 0                           8, 1, 282, 0    Vim Developers
HxCalendarAppImm     16.0.12624.20348                       16.0.12624.2... Microsoft Corporation
HxOutlook            16.0.12624.20348                       16.0.12624.2... Microsoft Corporation
HxTsr                16.0.12624.20368                       16.0.12624.2... Microsoft Corporation
ibtsiva
Idle
```

Windows Event Viewer
----

c:\Windows\System32\winevt\Logs