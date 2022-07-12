## Makefile to compile and run the project (a PINS Compiler).
## NOTE: Not tested for UNIX systems.

# Compiler and virtual machine variables.
JAVAC       = javac
JAVA        = java
JAVAC_FLAGS = -g:none -Xlint:all -encoding utf-8

# Find user operating system, and set the appropriate flag.
# Used to know which operations to use.
ifeq ($(OS),Windows_NT)
	detected_os = Windows
else
	detected_os = $(shell uname)
endif

# Assign variables based on operating system.
ifeq ($(detected_os),Windows)
	RM   = del /s /f /q
	SRCS = $(shell dir /s /b *.java)
	MAIN = pins\Compiler.class
	OUT  = out
	PRGS = ../../prg

	FND = dir /s /b
	SEP = \\
	STR = 
else
	RM   = rm -rf
	SRCS = $(shell find ./src/pins -name "*.java")
	MAIN = ./pins/Compiler.class
	OUT  = ./out
	PRGS = ../../prg

	FND = find
	SEP = /
	STR = ./
endif

# Flags used to run the test specified.
SRC    = permutations-test
TARGET = imclin

ifeq ($(detected_os),Windows)
	PINS = $(addprefix ",$(addsuffix ",$(notdir $(shell $(FND) $(STR)prg$(SEP)*.pins))))
else
	PINS = $(notdir $(shell $(FND) $(STR)prg$(SEP)*.pins))
endif

.PHONY: run clean
.SILENT: clean

# Rules for compilation.
pins/Compiler.class: $(SRCS)
	@$(JAVAC) $(JAVAC_FLAGS) --source-path $(SRCS) -d $(OUT) 

default: $(SRCS:.java=.class)

run: 
	$(JAVA) -Dfile.encoding=UTF-8 -cp $(OUT) pins.Compiler --src-file-name=$(PRGS)/$(SRC).pins --target-phase=$(TARGET)

# Run to rule a single test. Identical to the run rule, but recompiles the program.
test: pins/Compiler.class
	$(JAVA) -Dfile.encoding=UTF-8 -cp $(OUT) pins.Compiler --src-file-name=$(PRGS)/$(SRC).pins --target-phase=$(TARGET)

# Rule to recompile the program and run all tests in the prg directory.
tests: pins/Compiler.class
ifeq ($(OS),Windows_NT)
	$(info "Running tests...")
	@(FOR %%d IN ($(PINS)) DO (\
		echo. & \
		echo Running test: %%d & \
		$(JAVA) -Dfile.encoding=UTF-8 -cp $(OUT) pins.Compiler --target-phase=$(TARGET) --src-file-name=$(PRGS)/%%d  \
	)) || exit 0
#      || exit 0 is needed incase the last test throws an error. 
#      If it does, makefile understands that as an error with the command and 
#      will throw an error without a description.
	$(info "Done.")
else
	$(info "Running tests...")
	@for d in $(PINS); do \
		$(JAVA) -Dfile.encoding=UTF-8 -cp $(OUT) pins.Compiler --src-file-name=$(PRGS)/$$d --target-phase=$(TARGET); \
	done || true
#      || true is needed incase the last test throws an error. 
#      If it does, makefile understands that as an error with the command and 
#      will throw an error without a description.
	$(info "Done.")
endif
 
clean:
	@$(RM) *.class

