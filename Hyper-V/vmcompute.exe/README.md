Vmcompute is a system file that assist in Virtualization task. It heavily relys on a exposed kernel object which name Is still undocumented. Walking the object manager, if you have Virtualization enabled, will show this object exposed.

vmcompute.exe has several modules that export functions that are handlers for IOCTL routines.

I will seperate the directories by dlls that are loaded at Runtime
