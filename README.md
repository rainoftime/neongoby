Building NeonGoby
===================

Build LLVM 3.0/3.1 and clang 3.0/3.1 from source code.

Build repo rcs. Checkout `git@github.com:wujingyue/rcs.git` and follow the
readme there.

Finally, build Loom:

    ./configure --with-rcssrc=<rcs srouce directory> --with-rcsobj=<rcs object directory> --prefix=`llvm-config --prefix`
    make
    make install

Running NeonGoby
==================

From now on in this manual, we assume the program name is hello, and the alias
analysis we are checking is basicaa.

Generate a bitcode file using clang 3.0:

    clang++ hello.cpp -o hello.bc -c -emit-llvm

Online mode
-----------

    dynaa_insert_alias_checker.py hello basicaa

If `--disable-inline` is specified, the alias checks will not be inlined, and
will be linked in the codegen phase. If `--disable-opt` is specified, the
script will not run the standard compiler optimizations (`-O3`).  Run the
output executable (`hello.ac`). If it aborts or fails asserts, there is an
error in the alias analysis we are checking.

Offline mode
------------

    dynaa_hook_mem.py hello
    ./hello.inst
    mv /tmp/pts hello.pts
    dynaa_check_aa.py hello.bc hello.pts basicaa

Utilities
--------------------

    dynaa_dump_log -log-file hello.pts > hello.log
