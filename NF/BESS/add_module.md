1. Add C++ module src and header code in the `$BESS_HOME/core/modules/` folder.
2. Add corresponding grpc message defination in `$BESS_HOME/protobuf` folder.
3. If module requires third-party library, add the flag in `$BESS_HOME/core/Makefile`.
4. Recommendation: comment `build_dpdk()` in build.py to speed up the building process is feasible!
