# C++ 外部链接性

在 C++ 中，**外部链接性（external linkage）** 指的是一个标识符（变量或函数）在多个翻译单元（translation unit）中可见和可访问的特性。换句话说，具有外部链接性的符号可以在不同的源文件之间共享和引用。

## **外部链接性的特点**

- 变量或函数在不同的源文件中都可以访问。
- 编译器不会为每个翻译单元都创建新的实例，而是多个翻译单元共享同一个定义。
- 需要 `extern` 关键字来声明引用外部符号，但定义时不能加 `extern`。

## **如何实现外部链接性**

### **1. 变量的外部链接性**

```cpp
// a.cpp
#include <iostream>

int g_value = 10;  // 具有外部链接性

void printValue() {
    std::cout << g_value << std::endl;
}
```

```cpp
// b.cpp
#include <iostream>

extern int g_value;  // 声明外部变量，不定义

void printValue();

int main() {
    printValue();  // 访问外部定义的变量
    return 0;
}
```

✅ **解释**

- `g_value` 在 `a.cpp` 中被定义，它具有外部链接性，因此 `b.cpp` 可以使用 `extern int g_value;` 来声明它，而不会重新定义。

### **2. 函数的外部链接性**

```cpp
// a.cpp
#include <iostream>

void showMessage() {  // 具有外部链接性
    std::cout << "Hello, World!" << std::endl;
}
```

```cpp
// b.cpp
extern void showMessage();  // 声明外部函数

int main() {
    showMessage();
    return 0;
}
```

✅ **解释**

- **函数默认具有外部链接性**，所以 `showMessage` 不需要 `extern` 也可以在 `b.cpp` 里使用 `extern void showMessage();` 进行声明。

## **与内部链接性的区别**

- **内部链接性（internal linkage）**：变量或函数仅在**定义它的翻译单元内部可见**，不能被其他文件访问，通常使用 `static` 关键字修饰。
- **外部链接性（external linkage）**：变量或函数可以在多个翻译单元中访问。

### **示例：内部 vs. 外部**

```cpp
// 内部链接性
static int count = 0;  // 仅在本文件可见

// 外部链接性
int globalCount = 0;  // 可被其他文件访问
```

## **关键点总结**

1. **默认情况下**

   - **非** const **全局变量** 和 **非** static **函数** 具有 **外部链接性**。
   - const **全局变量** 具有 **内部链接性**，需要使用 `extern` 才能使其具有外部链接性：
     ```cpp
     extern const int maxValue = 100;  // 让 const 变量具有外部链接性
     ```
   - **类的成员函数** 具有 **外部链接性**，除非被 `static` 或 `inline` 修饰。

2. **使用** static **修饰** 变量或函数时，它们的链接性会变成 **内部链接性**，仅限本文件可用。

3. **使用** extern**关键字** 可以声明外部变量或函数，使其在其他翻译单元中可用，但 `extern` **不能用于定义**。

这样，外部链接性使得 C++ 可以支持多文件编程，并通过 `extern` 进行符号共享。

