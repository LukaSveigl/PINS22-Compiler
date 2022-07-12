# PINS22-Compiler
A compiler for the PINS'22 programming language.

---

## 1. Description

This is the implementation of a front-end of a compiler for the **PINS'22 programming language**, created for a CS Undergraduate course in **Compilers and Virtual machines.** The template for the compiler was kindly provided to us by our professor, dr. Boštjan Slivnik. The specification for the programming language can be found in *[pins21-specification-SLO.pdf](pins21-specification-SLO.pdf)*, and the grammar for the language can be found in *[grammar.txt](grammar.txt)*. Some examples of pins programs can be found in *[pins22/prg](pins22/prg/)*.

---

## 2. Building and running

The compiler can be built using the Makefile provided in the pins22 directory, or using an IDE of your choice. Recommended Java version is atleast Java 14.

To run the compiler, two command line flags need to be specified, the first being `--target-phase=<phase>`, which specifies up to which compilation phase the source program should be compiled, and the second being `--src-file-name=<file-name>`, which specifies which .pins file should be compiled.

---

## 3. [The compiler](pins22/src/pins/Compiler.java)

The compiler carries out the compilation phase by phase in the following order:
1. Lexical analysis
2. Syntactic analysis
3. Semantic analysis (name resolving) 
4. Semantic analysis (type checking) 
5. Memory layout 
6. Intermediate code generation 
7. Intermediate code linearization 

### 3.1 [Lexical analysis (lexan):](pins22/src/pins/phase/lexan/LexAn.java)

This phase takes a source file as the input, reads it character by character and tries to combine those into lexemes. When a valid lexeme is found, it is returned as a **Symbol**, which contains the information about the lexeme, the lexical token, and the location in the source file. 

### 3.2 [Syntactic analysis (synan):](pins22/src/pins/phase/synan/SynAn.java)

This phase takes the symbols returned by the lexical analyser one by one, and tries to build grammatical productions out of them. It does this by using an **LL(1) recursive descent parser**. The output of this phase is an AST of the source file.

### 3.3 [Semantic analysis (seman):](pins22/src/pins/phase/seman/SemAn.java)

#### 3.3.1 [Name resolving:](pins22/src/pins/phase/seman/NameResolver.java)

This phase takes the AST returned by the syntactic analyser, and resolves the names of types, variables and functions. It does so in 5 phases:
1. defined type names,
2. usages of those type names, 
3. definitions of variables, 
4. definitions of function names,
5. the usages of function names.

This is because the type names can be used in variable/function definitions, etc. The output of this phase is an AST of the source file with added name attributes.

#### 3.3.2 [Type checking:](pins22/src/pins/phase/seman/TypeChecker.java)

This phase takes the AST returned by the name resolver, and checks if all the types in the program are valid (as specified by the specification). It does so in the same 5 phases as the name resolver. The output of this phase is an AST of the source file with added name and type attributes.

### 3.4 [Memory layout (memory):](pins22/src/pins/phase/memory)

This phase takes the AST returned by the semantic analyser, and computes the memory layout (frames and accesses (variables and parameters)). It does so by checking all declarations and accesses in the AST, and computing the memory frames for functions, and accesses (relative or absolute) for variables and function parameters. The output of this phase is a memory layout model.

### 3.5 [Intermediate code generation:](pins22/src/pins/phase/imcgen)

This phase takes the AST and the memory layout of the source file, and generates the tree intermediate representation of the file, using functions as entry points for the code generator. The code generator calls the expression or statement generator, depending on what is being constructed currently. The output of this phase is a tree intermediate representation of the source file.

### 3.6 [Intermediate code linearization:](pins22/src/pins/phase/imclin)

This phase takes the intermediate code returned by the IMC generator and linearizes and canonizes it. It also contains an interpreter for the linearized code, which is used for running the source file. 

---

Keep in mind, that this is nothing more than a learning project and there is still plenty of room for improvement. 
