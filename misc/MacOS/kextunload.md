## `kextunload` to load or unload kernel extensions. Sometimes we copy commands from websites. You will have `Can't create -b` error.

The problem is usually, the encoding of characters on websites use UTF-8, which makes `kextunload` does not support UTF-8 characters. To solve the problem, simply manually retype the commend in a terminal.
