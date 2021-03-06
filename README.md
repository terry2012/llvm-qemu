# llvm-qemu
Automatically exported from code.google.com/p/llvm-qemu

This project is part of the Google Summer of Code 2007.

The goal of this project is to modify the QEMU dynamic binary translator to use components of the LLVM compiler infrastructure to turn it into a highly optimizing dynamic binary translator in order to increase the performance of QEMU even further.

Instead of directly emitting code for the host architecture QEMU is running on, the target code is first translated to LLVM IR, then a selection of LLVM's optimization functions are applied to the IR and the LLVM JIT is used to generate code from the optimized IR for the host architecture.

Since the translation to LLVM IR, the optimization and the code generation comes at a cost of an increased execution time, it's not feasible to apply this process to any piece of code, else the execution time would be even lower. Especially since on average a program spends 90% of its time within 10% of the code it is critical to get these 10% to execute fast, for the other 90% of the code parts might only execute once or only a few times and the extra time spent to generate the optimized code would not pay off. Therefore the idea is to identify the "hotspots" of the program. The most simple way is to count how many times a piece of code has been executed and performing an optimizing translation once a certain threshold is hit or falling back to the current binary translation of QEMU if not.

Another approach could be to instrument the target code at LLVM IR level to get an execution profile (LLVM already contains support for profiling) and use it to identify hotspots and to be able to perform profile-guided optimizations during runtime. Possibly a combination of both approaches will lead to the biggest overall performance gain. Detailed speed measurements will be performed in order to evaluate the efficiency of the different approaches, especially in comparison to the approach currently used by QEMU.
