For the makefile to work, it has to be placed inside the
pins22 directory, which should contain the ./src and ./prg
directories. Note that the makefile isn't tested on UNIX-like
systems.

------------------------------------------------------------

To compile the project using the makefile, use

	make

to clean the .class files using the makefile, use

	make clean

, to run the project using the makefile, use

	make run

, to run a specific test using the makefile, use

	make test SRC=<test-name[without the .pins extension]> TARGET=<target-phase>

and to run all tests using the makefile, use

	make tests

in the terminal/cmd.

Note that all tests run by 'make test' or 'make tests' must 
be located inside a folder called prg.

------------------------------------------------------------

Note that the default flags with which to run the project are

	TARGET=imclin
	SRC=permutations-test

To use different flags, run the project using 

	make run TARGET=<target-phase> SRC=<file-name>