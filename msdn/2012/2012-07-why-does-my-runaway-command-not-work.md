# Why does my !runaway command not work

Sometimes when I execute: ***!runaway*** in WinDbg I receive the following error:
ERROR:
!runaway: extension exception 0x80004002.  "Unable to get thread times - dumps may not have time information"

The error message says it all, but how do you fix it.  In my case [ProcDump][LINK1] was used to create the dump and it appears that the information required to execute this statement is not collected.  However you can collect this information using ProcDump if you copy the dbghelp.dll which is installed with the [Debugging Tools for Windows][LINK2] into the same directory as the procdump.exe.  Or add the path to the dbghelp.dll to your system path.  Then, when you create the memory dump, it will include the information provided when you execute the ***!runaway*** command.

Be sure to take note of the bit mode.  This means if you are taking a memory dump on a 64bit machine use the 64bit dgbhelp.dll and use the 32bit dbghelp.dll for a 32bit memory dump.

[LINK1]: http://technet.microsoft.com/en-us/sysinternals/dd996900
[LINK2]: http://msdn.microsoft.com/en-us/windows/hardware/gg463009
