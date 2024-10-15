27/09/2024 10:20

Tags:

Status: [[00_DSA]]  [[Linked List]] 

# Pointer

### **Difference between `NULL` and `nullptr`**

### 1. **`nullptr` (C++11 and later)**

- **Definition**: `nullptr` is a keyword introduced in **C++11** that represents a **null pointer literal**. It is specifically used for pointer types.
- **Type-Safety**: `nullptr` is of type `std::nullptr_t`, which can be implicitly converted to any pointer type but not to integral types. This makes it a **type-safe** null pointer.
- **Preferred in Modern C++**: Since `nullptr` provides better type safety and clarity, it is the **preferred way** to represent a null pointer in modern C++.
### 2. **`NULL` (C)**

- **Definition**: `NULL` is a **macro** traditionally defined as `0` (or `((void*)0)` in some older compilers) in C and C++. It has been used to represent a null pointer.
- **Type Issues**: Since `NULL` is often defined as `0`, it is technically an **integer constant** and not necessarily a pointer type. This can lead to **type ambiguities** or errors, especially in overloaded functions where an integer `0` might be confused with a null pointer.
- **Legacy**: `NULL` has been used historically, especially in **C** and earlier versions of C++. In modern C++, `nullptr` should be used instead.

### Examples 