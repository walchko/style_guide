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
  
  int my_var; // snake case
  
protected:
  int my_hidden_var; // state info user doesn't need access to
};
```

## Libraries

- Make header only libraries when possible, things compile really fast now a days
- Have function and methods return important info

```cpp
struct Status {
  bool ok;
  uint8_t err_no;
  uint8_t packets_found;
};

Status findPacketsInBuffer(const uint8_t *buffer) {
  // ...
  return Status{true,0,3};
}
```
