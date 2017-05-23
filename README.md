# learn-cpp

## Week 1: Static vs Dynamic building

### Static
Build & Run
```bash
gcc -I lib/include lib/libnumber.o src/main.c -o main.o
./main.o
# hello world! 3
```

Dependencies
```bash
ldd main.o
# linux-vdso.so.1 (0x00007fff965ed000)
# libc.so.6 => /lib64/libc.so.6 (0x00007f8ba10e4000)
# /lib64/ld-linux-x86-64.so.2 (0x0000563e4644b000)
```

Symbols
```bash
nm main.o                      
# 00000000004004f6 T add
# 0000000000601024 B __bss_start
# 0000000000601024 b completed.6917
# 0000000000601020 D __data_start
# 0000000000601020 W data_start
# 0000000000400430 t deregister_tm_clones
# 00000000004004b0 t __do_global_dtors_aux
# 0000000000600e10 t __do_global_dtors_aux_fini_array_entry
# 00000000004005d8 R __dso_handle
# 0000000000600e20 d _DYNAMIC
# 0000000000601024 D _edata
# 0000000000601028 B _end
# 00000000004005c4 T _fini
# 00000000004004d0 t frame_dummy
# 0000000000600e08 t __frame_dummy_init_array_entry
# 0000000000400740 r __FRAME_END__
# 0000000000601000 d _GLOBAL_OFFSET_TABLE_
#                  w __gmon_start__
# 00000000004005f4 r __GNU_EH_FRAME_HDR
# 00000000004003c8 T _init
# 0000000000600e10 t __init_array_end
# 0000000000600e08 t __init_array_start
# 00000000004005d0 R _IO_stdin_used
#                  w _ITM_deregisterTMCloneTable
#                  w _ITM_registerTMCloneTable
# 0000000000600e18 d __JCR_END__
# 0000000000600e18 d __JCR_LIST__
#                  w _Jv_RegisterClasses
# 00000000004005c0 T __libc_csu_fini
# 0000000000400550 T __libc_csu_init
#                  U __libc_start_main@@GLIBC_2.2.5
# 000000000040050a T main
#                  U printf@@GLIBC_2.2.5
# 0000000000400470 t register_tm_clones
# 0000000000400400 T _start
# 0000000000601028 D __TMC_END__
```

### Dynamic
Build & Run
```bash
gcc -I lib/include -L=lib -lnumber src/main.c -o main.so
LD_LIBRARY_PATH=lib ./main.so
# hello world! 3
```

Missing Dependencies
```bash
ldd main.so
# linux-vdso.so.1 (0x00007ffdde3c4000)
# libnumber.so => not found
# libc.so.6 => /lib64/libc.so.6 (0x00007f76211ae000)
# /lib64/ld-linux-x86-64.so.2 (0x000055fea41c0000)
```

Fixed Dependencies
```bash
LD_LIBRARY_PATH=lib ldd main.so
# linux-vdso.so.1 (0x00007ffdb8f8f000)
# libnumber.so => lib/libnumber.so (0x00007f4bb9f57000)
# libc.so.6 => /lib64/libc.so.6 (0x00007f4bb9b72000)
# /lib64/ld-linux-x86-64.so.2 (0x00005603200b3000)
```

Symbols
```bash
nm main.so
#                  U add
# 000000000060102c B __bss_start
# 000000000060102c b completed.6917
# 0000000000601028 D __data_start
# 0000000000601028 W data_start
# 00000000004005e0 t deregister_tm_clones
# 0000000000400660 t __do_global_dtors_aux
# 0000000000600e00 t __do_global_dtors_aux_fini_array_entry
# 0000000000400778 R __dso_handle
# 0000000000600e10 d _DYNAMIC
# 000000000060102c D _edata
# 0000000000601030 B _end
# 0000000000400764 T _fini
# 0000000000400680 t frame_dummy
# 0000000000600df8 t __frame_dummy_init_array_entry
# 00000000004008b8 r __FRAME_END__
# 0000000000601000 d _GLOBAL_OFFSET_TABLE_
#                  w __gmon_start__
# 0000000000400794 r __GNU_EH_FRAME_HDR
# 0000000000400560 T _init
# 0000000000600e00 t __init_array_end
# 0000000000600df8 t __init_array_start
# 0000000000400770 R _IO_stdin_used
#                  w _ITM_deregisterTMCloneTable
#                  w _ITM_registerTMCloneTable
# 0000000000600e08 d __JCR_END__
# 0000000000600e08 d __JCR_LIST__
#                  w _Jv_RegisterClasses
# 0000000000400760 T __libc_csu_fini
# 00000000004006f0 T __libc_csu_init
#                  U __libc_start_main@@GLIBC_2.2.5
# 00000000004006a6 T main
#                  U printf@@GLIBC_2.2.5
# 0000000000400620 t register_tm_clones
# 00000000004005b0 T _start
# 0000000000601030 D __TMC_END__
```
