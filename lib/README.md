## Static vs Dynamic Linking for build
### Static
```
gcc -c src/libnumber.c -o libnumber.o
ldd libnumber.o
# not a dynamic executable
nm libnumber.o
# 0000000000000000 T add
```

### Dynamic
```
gcc -shared src/libnumber.c -o libnumber.so
ldd libnumber.so
# linux-vdso.so.1 (0x00007ffed27e6000)
# libc.so.6 => /lib64/libc.so.6 (0x00007fea3ca57000)
# /lib64/ld-linux-x86-64.so.2 (0x0000561144475000)
nm libnumber.so
# 0000000000000650 T add
# 0000000000201018 B __bss_start
# 0000000000201018 b completed.6917
#                  w __cxa_finalize@@GLIBC_2.2.5
# 0000000000000550 t deregister_tm_clones
# 00000000000005e0 t __do_global_dtors_aux
# 0000000000200e30 t __do_global_dtors_aux_fini_array_entry
# 0000000000200e40 d __dso_handle
# 0000000000200e48 d _DYNAMIC
# 0000000000201018 D _edata
# 0000000000201020 B _end
# 0000000000000664 T _fini
# 0000000000000620 t frame_dummy
# 0000000000200e28 t __frame_dummy_init_array_entry
# 00000000000006f0 r __FRAME_END__
# 0000000000201000 d _GLOBAL_OFFSET_TABLE_
#                  w __gmon_start__
# 0000000000000670 r __GNU_EH_FRAME_HDR
# 0000000000000510 T _init
#                  w _ITM_deregisterTMCloneTable
#                  w _ITM_registerTMCloneTable
# 0000000000200e38 d __JCR_END__
# 0000000000200e38 d __JCR_LIST__
#                  w _Jv_RegisterClasses
# 0000000000000590 t register_tm_clones
# 0000000000201018 d __TMC_END__
```
