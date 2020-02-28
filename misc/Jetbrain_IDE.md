## Setting up remote debugging
https://www.jetbrains.com/help/clion/remote-debug.html

## Sometimes, you cannot set breakpoints to in the remote gdb debugging.

The reason possibly be that the `-g` flag is not enabled. Please check the `Makefile` file.
