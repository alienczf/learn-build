export LD_LIBRARY_PATH=lib

main: src/main.c lib/include/libnumber.h lib/libnumber.so
	gcc -Ilib/include -Llib -lnumber src/main.c -o main

lib/libnumber.so: lib/src/libnumber.c
	gcc -shared lib/src/libnumber.c -o lib/libnumber.so

.PHONY : clean
clean:
	-rm -f main lib/*.so

.PHONY : run
run:
	./main
