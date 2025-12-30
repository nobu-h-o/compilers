# Compiler Learning Series: 1cc → 2cc → 3cc

## Overview

This project demonstrates the evolution of compiler technology through three complete implementations:

- **[1cc](https://github.com/nobu-h-o/1cc)**: Foundation - Hand-written everything (tokenizer, parser, x86-64 codegen)
- **[2cc](https://github.com/nobu-h-o/2cc)**: Professional Tools - Flex/Bison frontend, ARM64 backend
- **[3cc](https://github.com/nobu-h-o/3cc)**: Modern Infrastructure - LLVM backend, C++, multi-architecture

Each compiler is a fully functional, educational implementation that progressively introduces industry-standard tools and techniques.

## Progression


| Stage | **1cc** | **2cc** | **3cc** |
|-------|---------|---------|---------|
| **Lexer** | Hand-written tokenizer | **Flex** (pattern matching) | Flex |
| **Parser** | Recursive descent | **Bison** (formal grammar) | Bison |
| **Language** | C | C | **C++** |
| **Code Gen** | x86-64 Assembly | ARM64 Assembly | **LLVM IR** |
| **Target** | x86-64 only | ARM64 only | **Multi-architecture** |
| **Build** | Make | Make | **CMake** |
| **Lines of Code** | ~800 | ~600 | ~900 |

## Architecture Diagrams

### 1cc Pipeline
```
Source Code: "x = 5 + 3"
    ↓
┌─────────────────┐
│   Tokenizer     │  tokenize.c (hand-written)
│  Character scan │  [IDENT("x"), PUNCT("="), NUM(5), PUNCT("+"), NUM(3)]
└────────┬────────┘
         ↓
┌─────────────────┐
│     Parser      │  parse.c (recursive descent)
│  Build AST      │  ASSIGN(LVAR("x"), ADD(NUM(5), NUM(3)))
└────────┬────────┘
         ↓
┌─────────────────┐
│  Code Gen       │  codegen.c (x86-64 assembly)
│  Stack-based    │  mov rax, 5 / push rax / mov rax, 3 / pop rdi / add rax, rdi
└─────────────────┘
```

### 2cc Pipeline
```
Source Code: "x = 5 + 3"
    ↓
┌─────────────────┐
│     Flex        │  lexer.l (pattern matching)
│   yylex()       │  [IDENT, ASSIGN, NUM, ADD, NUM]
└────────┬────────┘
         ↓
┌─────────────────┐
│     Bison       │  parser.y (LALR parser)
│   yyparse()     │  AST: ASSIGN(LVAR, ADD(NUM, NUM))
└────────┬────────┘
         ↓
┌─────────────────┐
│  Code Gen       │  codegen.c (ARM64 assembly)
│  Register-based │  mov w0, #5 / str w0, [sp] / ldr w1, [sp] / add w0, w0, w1
└─────────────────┘
```

### 3cc Pipeline
```
Source Code: "x = 5 + 3"
    ↓
┌─────────────────┐
│     Flex        │  lexer.l (pattern matching)
│   yylex()       │  [IDENT, ASSIGN, NUM, ADD, NUM]
└────────┬────────┘
         ↓
┌─────────────────┐
│     Bison       │  parser.y (LALR parser)
│   yyparse()     │  AST: ASSIGN(LVAR, ADD(NUM, NUM))
└────────┬────────┘
         ↓
┌─────────────────┐
│  LLVM Codegen   │  codegen.cpp (LLVM C++ API)
│  IR Builder     │  %0 = add i32 5, 3 / store i32 %0, i32* %x
└────────┬────────┘
         ↓
┌─────────────────┐
│  LLVM Backend   │  (automatic multi-arch)
│  Optimization   │  → x86-64, ARM64, RISC-V, etc.
└─────────────────┘
```

## Resources

### Learning Compiler Construction
- [低レイヤを知りたい人のためのCコンパイラ作成入門](https://www.sigbus.info/compilerbook) (Japanese, primary inspiration)
- [Crafting Interpreters](https://craftinginterpreters.com/) (English, excellent tutorial)
- [Engineering a Compiler](https://www.elsevier.com/books/engineering-a-compiler/cooper/978-0-12-088478-0) (Textbook)

### Assembly and Architecture
- [Intel 64 Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)
- [ARM Architecture Reference Manual](https://developer.arm.com/documentation/)
- [System V AMD64 ABI](https://github.com/hjl-tools/x86-psABI/wiki/x86-64-psABI-1.0.pdf) (calling conventions)

### Tools Documentation
- [Flex Manual](https://github.com/westes/flex)
- [Bison Manual](https://www.gnu.org/software/bison/manual/)
- [LLVM Documentation](https://llvm.org/docs/)
- [LLVM Kaleidoscope Tutorial](https://llvm.org/docs/tutorial/) (excellent LLVM intro)

### Related Projects
- [chibicc](https://github.com/rui314/chibicc) - Small C compiler by Rui Ueyama
- [8cc](https://github.com/rui314/8cc) - Another C compiler by Rui Ueyama
- [TCC](https://bellard.org/tcc/) - Tiny C Compiler by Fabrice Bellard
