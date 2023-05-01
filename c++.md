<a href="https://isocpp.org/" target="_blank"> 
  <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/cplusplus/cplusplus-original.svg" alt="C++" height="200"/> 
</a>  
  
# C++ Style Guide

## `clang-format`

`clang-format -style=file:../dotfiles/clang-format.yml -i src/**/*.hpp src/**/*.h src/**/*.cpp examples/**/*.cpp`

```yaml
---
Language:        Cpp
# https://clang.llvm.org/docs/ClangFormatStyleOptions.html
AlignConsecutiveAssignments:
  Enabled:         true
  AcrossEmptyLines: true
  AcrossComments:  false
  AlignCompound:   false
  PadOperators:    true
AllowShortIfStatementsOnASingleLine: AllIfsAndElse
# BraceWrapping:
#   BeforeElse:      true
BreakBeforeBraces: Custom
BraceWrapping:
  BeforeElse: true
IndentPPDirectives: BeforeHash%    
```

## Macros, Constants and `#defines`

- Use ALL CAPS for constants 
- Don't use `#defines` if possible
- Use `constexpr` so compiler can double check type

```cpp
#pragma once // simplier than if/endif

#if defined(ARDUINO)
...
#endif

#define VAR 1 // avoid, can cause bugs
constexpr int VAR = 1; // all caps and compiler will double check type usage
```

## Namespaces

## Class

```cpp
class Foo {
public:
  Foo() {}
  ~Foo() {}
  void myFunction() {} // camel case
  
  // inline functions only when 1 or maybe 2 lines of code
  inline int oneliner() {} 
  
  int my_var; // snake case
  
protected:
  int my_hidden_var; // state info user doesn't need access to
};
```

## Structs

```cpp
struct Foo_t {
  int bar;
};
```


## `enum`, `union`, `using` and `typedef`

```cpp
enum Foo_t: uint16_t {A, B, C};

union {
  struct {
     uint8_t hi,lo;
  };
  uint16_t u16;
} Memory_t;

typedef std::vector<int> IntVec_t;
template<typename T, uint16_t MAX_SIZE=64> using myDeque = myVector<T,MAX_SIZE>;
```

## Functions and Methods

- Have function and methods return important and useful info
  - Use `struct` instead of `tuple`
  - Try to keep simple data types in `struct` like `int`, `bool`, `float`, etc
- Stand-a-lone functions use snake case
- No more than 3 indents in a function or create a new function to handle the complexity
- Return as soon as possible to reduce indentation

```c++
struct Status {
  bool ok;
  uint8_t err_no;
  uint8_t packets_found;
};

Status find_packets(const uint8_t *buffer) { // snake case
  // ...
  return Status{true,0,3};
}

bool find_packets2(const uint8_t *buffer) {
  if (buffer == nullptr) return false; // return
  // ...
  return true;
}

// Too much indenting can be confusing on code. Try to 
// limit it to 3 levels. If you need more, then break
// part of your code out into another function.
bool indents(int val) {
  if (val == 0) {
    return false; // 1 indent
  }
  
  for (int i =1; i < 5; ++i) {
    if (val == i) return true; // 2 indent
    if (val/2 == i) {
      if (i == 4) {
        return true; // 3 indents
      }
    }
  }
  return true;
}
```

## Libraries

- Make header only libraries when possible, things compile really fast now a days
- Wrap libraries in a `namespace`

```cpp
// my header.hpp
#pragma once

#if defined(__APPLE__)
  // do something
#elif defined(ARDUINO)
  // do something
  #if defined(ARDUINO_ITSYBITSY_M0)
    // do something
  #elif defined(ARDUINO_ITSYBITSY_M4)
    // do something
  #endif
#elif defined(linux) || defined(UNIX)
  // do something
#else
  // do something
#endif

namespace kevin {

constexpr int COOL_VAR = 33;
constexpr bool DO_NOW = false;

class MyClass {};

static bool myFunction() {} // have to make static

} // end namespace
```
