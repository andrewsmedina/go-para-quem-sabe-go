# GCFLAGS

```
$ go build -gcflags
``

Usado para passar parâmetros para o compilador.
`go tool compile -help` list todas os parâmetros suportados
pelo compilador.

```
$ go build -gcflags="-N -l"
```

```
 -%	debug non-static initializers
  -+	compiling runtime
  -B	disable bounds checking
  -C	disable printing of columns in error messages
  -D path
    	set relative path for local imports
  -E	debug symbol export
  -I directory
    	add directory to import search path
  -K	debug missing line numbers
  -N	disable optimizations
  -S	print assembly listing
  -V	print compiler version
  -W	debug parse tree after type checking
  -asmhdr file
    	write assembly header to file
  -bench file
    	append benchmark times to file
  -blockprofile file
    	write block profile to file
  -buildid id
    	record id as the build id in the export metadata
  -c int
    	concurrency during compilation, 1 means no concurrency (default 1)
  -complete
    	compiling complete package (no C or assembly)
  -cpuprofile file
    	write cpu profile to file
  -d list
    	print debug information about items in list; try -d help
  -dolinkobj
    	generate linker-specific objects; if false, some invalid code may compile (default true)
  -dwarf
    	generate DWARF symbols (default true)
  -dynlink
    	support references to Go symbols defined in other shared libraries
  -e	no limit on number of errors reported
  -f	debug stack frames
  -goversion string
    	required version of the runtime
  -h	halt on error
  -i	debug line number stack
  -importcfg file
    	read import configuration from file
  -importmap definition
    	add definition of the form source=actual to import map
  -installsuffix suffix
    	set pkg directory suffix
  -j	debug runtime-initialized variables
  -l	disable inlining
  -linkobj file
    	write linker-specific object to file
  -live
    	debug liveness analysis
  -m	print optimization decisions
  -memprofile file
    	write memory profile to file
  -memprofilerate rate
    	set runtime.MemProfileRate to rate
  -msan
    	build code compatible with C/C++ memory sanitizer
  -mutexprofile file
    	write mutex profile to file
  -nolocalimports
    	reject local (relative) imports
  -o file
    	write output to file
  -p path
    	set expected package import path
  -pack
    	write package file instead of object file
  -r	debug generated wrappers
  -race
    	enable race detector
  -s	warn about composite literals that can be simplified
  -shared
    	generate code that can be linked into a shared library
  -std
    	compiling standard library
  -traceprofile file
    	write an execution trace to file
  -trimpath prefix
    	remove prefix from recorded source file paths
  -u	reject unsafe code
  -v	increase debug verbosity
  -w	debug type checking
  ```