---
title: llvm-Value用法
published: 2025-10-05
description: ''
image: ''
tags: [LLVM编译器开发]
category: 'LLVM编译器开发'
draft: false 
lang: ''
---

# llvm::Value 使用指南

本文档快速介绍了 LLVM 中 `llvm::Value` 的常见用法、重要 API、示例代码和常见陷阱。适合在使用 LLVM IRBuilder/Module/Context 时作为速查手册。

> [!WARNING]
> 本篇内容几乎均由ai生成，给作者做了解用 

## 简短契约
- 输入：LLVM C++ API（IRBuilder、Module、Context 等已可用）
- 输出：对 `llvm::Value` 的典型用法、代码片段、调试与陷阱说明
- 成功标准：能创建常用 Value（常量、函数、全局、指令）、查询和修改使用关系、以及安全地替换或移除 Value。

## 核心概念
- `llvm::Value` 是 LLVM IR 中的抽象基类，表示任何“值"——常量、指令、函数、全局变量、参数等都继承自它。
- `Value` 本身是抽象的：通常以 `Value*` 指针形式使用。
- `User`：`Value` 的子类 `User`（如 `Instruction`、`ConstantExpr`）是含有操作数的实体。`User` 持有对其它 `Value` 的引用。
- 所有 `Value` 都有类型 (`Value->getType()`)，有些有名字 (`getName()` / `setName()`)，并且可以被其他 `Value` 使用（`uses()` / `users()`）。
- 所有权（谁负责释放）：Value 通常由拥有者容器负责（Instruction 属于 BasicBlock，BasicBlock 属于 Function，GlobalVariable 属于 Module），不要手动 delete，使用 LLVM 提供的 eraseFromParent()/dropAllReferences() 等。

## 常见子类
- `Instruction`（继承 `User`） — 运行时可变，存在于 BasicBlock。
- `Constant`（继承 `Value`） — 常量类（`ConstantInt`、`ConstantFP`、`ConstantExpr`、`GlobalValue` 的初始值等）。
- `Function`（继承 `GlobalValue`） — 表示函数（也是 `Value`）。
- `GlobalVariable` / `GlobalValue` — 模块范围符号。
- `Argument` — 函数参数（`Argument*`）。
- `BasicBlock` — 也继承 `Value`（可以作为某些上下文的值）。
- `User` — 可以持有其他 `Value` 的对象（例如 instruction、constantexpr）。

## 重要成员函数（常用）
- 类型/名字
  - `getType()`: `Type*`
  - `hasName()`, `getName()`: `llvm::StringRef`
  - `setName(StringRef)`
- 用法/使用者
  - `uses()`  / `use_begin()` / `use_end()`
  - `users()` （返回迭代器的便捷）
  - `use_empty()`, `hasOneUse()`, `getNumUses()`
  - `replaceAllUsesWith(Value *newV)`
- 运行时/生命周期
  - `dropAllReferences()`（释放 operand 引用）
  - `eraseFromParent()` / `removeFromParent()`（Instruction/Global 的移除）
- 值种类检查 / 类型安全转换（推荐）
  - `llvm::isa<T>(val)`
  - `llvm::dyn_cast<T>(val)`
  - `llvm::cast<T>(val)`（断言失败时 abort）
- 调试 / 打印
  - `value->print(llvm::raw_ostream &)`
  - `llvm::errs() << *value << "\n";`

## 常见场景与示例代码（C++）
> 下面示例假设已包含合适头文件并有 `llvm::LLVMContext &Ctx`, `llvm::Module *M`, `llvm::IRBuilder<> Builder(Ctx)`。

### 1) 创建整数常量
```cpp
#include <llvm/IR/Constants.h>
// 32-bit int constant 42
llvm::Value *c42 = llvm::ConstantInt::get(llvm::Type::getInt32Ty(Ctx), 42);
```

### 2) 创建函数（声明/原型）
```cpp
using namespace llvm;
FunctionType *FT = FunctionType::get(Type::getVoidTy(Ctx), {Type::getInt32Ty(Ctx)}, false);
Function *F = Function::Create(FT, Function::ExternalLinkage, "foo", M);
// 为参数命名
F->getArg(0)->setName("x");
```

### 3) 创建/插入指令（用 IRBuilder）
```cpp
BasicBlock *BB = BasicBlock::Create(Ctx, "entry", F);
Builder.SetInsertPoint(BB);

// 使用函数参数和常量创建 add
Value *arg = F->getArg(0);
Value *c5 = ConstantInt::get(Type::getInt32Ty(Ctx), 5);
Value *sum = Builder.CreateAdd(arg, c5, "sum"); // 返回 Value*（实际是 Instruction*）
```

### 4) 创建全局变量
```cpp
GlobalVariable *g = new GlobalVariable(*M, Type::getInt32Ty(Ctx),
                                       /*isConstant=*/false,
                                       GlobalValue::ExternalLinkage,
                                       ConstantInt::get(Type::getInt32Ty(Ctx), 0),
                                       "g_counter");
```

### 5) 判断 Value 类型并转换到具体子类
```cpp
if (isa<Instruction>(sum)) {
    Instruction *I = cast<Instruction>(sum);
    BasicBlock *parentBB = I->getParent();
    Function *parentF = I->getFunction();
}
if (auto *CI = dyn_cast<CallInst>(someVal)) {
    // CallInst 特有操作
}
```

### 6) 遍历 uses / users
```cpp
// 遍历 use（可以同时获取使用者和被使用的位置）
for (auto &U : sum->uses()) {
    User *user = U.getUser(); // 使用 sum 的 User*
    // 例如若 user 是指令，打印它
    if (auto *inst = dyn_cast<Instruction>(user))
        inst->print(llvm::errs()), llvm::errs() << "\n";
}

// 遍历 users（更简洁）
for (User *u : sum->users()) {
    // ...
}
```

### 7) 替换所有用法
```cpp
Value *newVal = ...;
sum->replaceAllUsesWith(newVal); // 小心：如果你在替换自己，可能导致问题
```

### 8) 打印 Value（调试）
```cpp
llvm::errs() << "Value: "; 
sum->print(llvm::errs());
llvm::errs() << "\n";
```

### 9) 找到 Value 所属 Module
- 对于 `Instruction`：`instr->getFunction()->getParent()` 返回 `Module*`
- 对于 `GlobalValue`：`global->getParent()` 返回 `Module*`
- 对于其他 Value（比如常量或函数参数），需要先 dyn_cast 到具体子类才能获得 Module。

示例：
```cpp
if (auto *I = dyn_cast<Instruction>(V)) {
  Module *mod = I->getFunction()->getParent();
} else if (auto *GV = dyn_cast<GlobalValue>(V)) {
  Module *mod = GV->getParent();
}
```

### 10) 移除/销毁 Value（安全做法）
- Instruction: `I->eraseFromParent()` （会移除指令并释放）
- 如果想先安全断开引用：`I->dropAllReferences(); I->eraseFromParent();`
- GlobalValue: `GV->eraseFromParent()` 或 `GV->removeFromParent()` + `delete`（通常使用 eraseFromParent）

### 11) 常见常量 API
```cpp
Constant *z = Constant::getNullValue(Type::getInt32Ty(Ctx)); // 0
auto *ci = ConstantInt::get(Type::getInt32Ty(Ctx), 7);
auto *fp = ConstantFP::get(Type::getDoubleTy(Ctx), 3.14);
```

### 12) 通过 `llvm::Module::getOrInsertFunction` 插入函数（新 API 返回 FunctionCallee）
```cpp
FunctionCallee fc = M->getOrInsertFunction("printf", FunctionType::get(IntegerType::getInt32Ty(Ctx), PointerType::getInt8PtrTy(Ctx), true));
```

## 诊断未定义引用（与你遇到的错误相关）
`undefined reference to llvm::DisableABIBreakingChecks` 表明链接阶段缺少包含该符号的库（通常在 `LLVMSupport` / monolithic `libLLVM` 的静态库中）。解决步骤：
1. 确认 `find_package(LLVM COMPONENTS ...)` 是否提供了 `LLVM::Support` 或其它组件的导入目标（优先使用导入目标）。
2. 若 `llvm_map_components_to_libnames` 被用来生成 `-lLLVMCore` 风格的标志，确保链接器搜索路径包含 LLVM 的库目录（添加 `link_directories()` 或通过 CMake 导入目标自动处理）。
3. 用 `nm` 在对应的 `.a` / `.so` 中查找符号（示例：`nm -C /usr/lib/libLLVMSupport.a | grep DisableABIBreakingChecks`）。
4. 在 CMake 中优先链接导入目标（`target_link_libraries(target PRIVATE LLVM::Support ...)`）；若不得已使用库名，确保 `-L` 有效并把 `libLLVMSupport.a` 或 `libLLVM.so` 实体加入链接。

## 常见陷阱与建议
- 不要用 `delete` 手动销毁 LLVM Value。使用 `eraseFromParent()` / `dropAllReferences()`。
- 尽量用 `isa/dyn_cast/cast` 代替手工比较 `getValueID()`。
- 导入目标 (`LLVM::X`) 通常比 `-l…` 更稳健、跨平台（CMake 的 `find_package(LLVM COMPONENTS ...)` 会提供）。
- `replaceAllUsesWith` 会修改所有使用位置，要小心在遍历 uses 时同时替换（可能导致迭代失效）。
- 当链接器提示符号未定义，先用 `nm` 检查 LLVM 库是否包含该符号，再检查链接命令是否把对应库或路径传给链接器。