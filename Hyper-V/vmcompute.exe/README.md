Vmcompute is a system file that assist in Virtualization task. It heavily relys on a exposed kernel object named similar to the calling executable
and can be found with the `\\\\.\\VmCompute` path. Walking the object manager, if you have Virtualization enabled, will show this object exposed.

vmcompute.exe has several exports that are wrappers around IOCTL call routines using. A number of the exported functions end with a DeviceIoControl
call into the kernel.

Follow along:
Walking the Object manager to find running objects

With the provided wor.exe in the Glyph framework, you can enumerate all kernel objects exposed to usermode by supplying the \Device path.

wor.exe first param is the path you would like to search. Passing device should show VmCompute exposed.



