# Learn build
A repo to practice and record fundamentals about build process

## Week 1: build (clang)
please `cd clang` when following the tutorial to avoid path complications
 * Build refers to the **packaging** source files such that they can be executed
 * Includes (in general) these 3 steps:
   * preprocessing

        Expands/Executes Preprocessing directives (lines starting with `#`)
        example:
        ```bash
        cpp lib/src/libnumber.c
        ```
        ```c
        # 1 "lib/src/libnumber.c"
        # 1 "<built-in>"
        # 1 "<command-line>"
        # 31 "<command-line>"
        # 1 "/usr/include/stdc-predef.h" 1 3 4
        # 32 "<command-line>" 2
        # 1 "lib/src/libnumber.c"
        int add(int left, int right){
          return left + right;
        }
        ```
   * compile

        Translates code into executable binary
        ```bash
        gcc -c -I lib/include src/main.c -o main
        ```

        Under the hood:
        ```bash
        nm main
        #                  U add
        # 0000000000000000 T main
        #                  U printf
        ```
        Observe that even though the source code have been converted to asm, 
        some symbols remain Undefined. Internally, they are rendered as 
        unresolved references, to the effect of:
        ```
        ...
        # start of main
        asm
        asm
        ?ref add?
        asm
        asm
        ?ref printf?
        asm
        ...
        ```

   * link

        A linker steps in and resolve these dependencies.
        There are two ways that links can be done, static or dynamic.
        ```bash
        # static
        gcc -I lib/include lib/libnumber.o src/main.c -o main.o
        ```
        to the effect of:
        ```
        ...
        # asm for add elsewhere in file (can be after main)
        asm
        asm
        ret
        ...
        # start of main
        asm
        asm
        jmp address of add              # ?ref add? resolved
        asm
        asm
        jmp address of printf           # ?ref printf? resolved
        asm
        ...
        # asm for print off elsewhere in file (can be before main)
        asm
        asm
        ret
        ...
        ```
        observe that the references are resolved as static addresses.
        With reference to the code base:
        ```bash
        nm main.o
        # 00000000004004f6 T add
        # ...
        ```
        Take note how **add** symbol have a fixed address in file 
        **prior to runtime**.

        In contrast, dynamically links are not be resolved until runtime
        ```bash
        # dynamic
        gcc -I lib/include -L=lib -lnumber src/main.c -o main.so
        ```
        The above code have the effect of:
        ```
        # start of main
        asm
        asm
        ?ref add@number
        asm
        asm
        ?ref printf@stdio
        asm
        ...
        ```
        With reference to the code base:
        ```bash
        nm main.so
        #                  U add
        # ...
        ```
        Note that `add` symbol is **U**nidentified as compared to being in the 
        **T**ext section of the binary. These depedencies must be resolved by 
        runtime or else the binary cannot be executed
        ```bash
        ./main.so
        # ./main.so: error while loading shared libraries: libnumber.so: ...
        ```

        ```bash
        LD_LIBRARY_PATH=lib ./main.so
        # hello world! 3
        ```

