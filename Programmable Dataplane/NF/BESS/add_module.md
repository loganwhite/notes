1. Add C++ module src and header code in the `$BESS_HOME/core/modules/` folder.
2. Add corresponding grpc message defination in `$BESS_HOME/protobuf/module_msg.proto` file. **Note that the name of the message should be [Classname]Arg, otherwise, the system cannot find the corresponding message.**
3. If module requires third-party library, add the flag in `$BESS_HOME/core/Makefile`.
4. Recommendation: comment `build_dpdk()` in build.py to speed up the building process is feasible!
