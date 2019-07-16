## This document introduces the steps for creating new actors.

1. Create the source and header file of the actor (`*.h` and `*.c`).
2. Create the actor structure (`actor_[actor_name]`) which contains the states.
3. Implement the for functions: `[actor_name]_init`, `[actor_name]_mm_init`, `[actor_name]_run, [actor_name]_standalone_run`.
4. Add actor type id in the `shared.h` header file.
5. Add actor spawn code in the `middleman/actor.c` file to intialize the actor when running.
6. Add middleman attachment code to `middleman/actor.c` to attach the actor to the middleman.
7. Add action code to `middleman/mm.c`
