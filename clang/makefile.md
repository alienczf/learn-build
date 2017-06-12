## Intro to Makefiles
Once a `makefile` or `Makefile` is available in a folder  
one can call `make` to execute its contents

### Basic Makefile syntax
```make
# remember to add full path for files
target .. : prerequisites ...
    recipe

# useful for cleanup run etc
.PHONY : target
target .. : 
    target command to run

```
