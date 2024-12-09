# C++ Inheritance Concepts

## Table of Contents

1. [Types of Inheritance](#types-of-inheritance)
    1.1 [Public Inheritance](#public-inheritance)  
    1.2 [Protected Inheritance](#protected-inheritance)  
    1.3 [Private Inheritance](#private-inheritance)  
2. [Multiple Inheritance](#multiple-inheritance)  
    2.1 [Issue with Multiple Inheritance](#issue-with-multiple-inheritance)  
    2.2 [Role of Virtual Keyword](#role-of-virtual-keyword)  

---

## 1. Types of Inheritance

### 1.1 Public Inheritance

Public inheritance is the most common type of inheritance in C++. In this type of inheritance:

- All **Public members** of the **Base Class** become **Public Members** in the **Derived Class**.
- All **Protected members** of the **Base Class** become **Protected Members** in the **Derived Class**.
- The **Private members** of the **Base Class** remain inaccessible to the **Derived Class**, as they are not inherited.

Public inheritance is used when the derived class *is a type of* the base class (the "is-a" relationship). This means that the derived class shares functionality with the base class, and there is a clear hierarchy.

---

### 1.2 Protected Inheritance

In **Protected Inheritance**:

- All **Public members** of the **Base Class** become **Protected Members** in the **Derived Class**.
- All **Protected members** of the **Base Class** remain **Protected Members** in the **Derived Class**.
- **Private members** of the **Base Class** are still not accessible in the **Derived Class**.

Protected inheritance is used less frequently, and is typically employed when you need to hide certain parts of the base class from the outside world while still allowing the derived class to access and potentially modify them.

---

### 1.3 Private Inheritance

In **Private Inheritance**:

- All **Public members** of the **Base Class** become **Private Members** in the **Derived Class**.
- All **Protected members** of the **Base Class** become **Private Members** in the **Derived Class**.
- **Private members** of the **Base Class** are not inherited and are inaccessible.

Private inheritance is used when you want to model the "has-a" relationship rather than the "is-a" relationship. It implies that the derived class *contains* the base class but doesn't share the same interface as the base class.

---

## 2. Multiple Inheritance

### 2.1 Issue with Multiple Inheritance

Multiple inheritance occurs when a derived class inherits from more than one base class. While C++ supports multiple inheritance, it can lead to issues such as ambiguity and confusion. A typical scenario of multiple inheritance involves a derived class that inherits from multiple base classes that have overlapping member names or functionalities.

#### Problem 1: Ambiguity in Member Variables

Consider the following example where the derived class inherits from two base classes:

```cpp
class Base1 {
    int a;
};

class Base2 {
    int a;
};

class Derived : public Base1, public Base2 {
    int c;
public:
    Derived(int x, int y, int z) : Base1(x), Base2(y) { c = z; }
    int product() { return a * a * c; } // Error: ambiguity in variable 'a'
};
```

In this case, the variable a from both Base1 and Base2 creates ambiguity, as the compiler doesn't know whether to use Base1::a or Base2::a. To resolve this ambiguity, you can specify which a to use by qualifying it with the class name:

```Cpp
class Derived : public Base1, public Base2 {
    int c;
public:
    Derived(int x, int y, int z) : Base1(x), Base2(y) { c = z; }
    int product() { return Base1::a * Base2::a * c; } // Resolve ambiguity
};
```

#### Problem 2: Virtual Inheritance

![alt text](image.png)

The `virtual` keyword plays an important role in resolving ambiguities in multiple inheritance. Without virtual inheritance, the base class might be inherited more than once by the derived class, leading to multiple copies of the base class' members. Virtual inheritance ensures that only one copy of the base class is inherited.

#### 2.2 Role of Virtual Keyword
In multiple inheritance, the `virtual` keyword ensures that only one instance of the base class is inherited, even if the derived class inherits the base class through multiple paths. This is essential for solving issues like the diamond problem.

Consider the following example where `Base` is virtually inherited by both `Derived1` and `Derived2`:
```Cpp
class Base {
public:
    int a;
    Base(int x) : a(x) {}
};

class Derived1 : virtual public Base {
public:
    Derived1(int x) : Base(x) {}
};

class Derived2 : virtual public Base {
public:
    Derived2(int x) : Base(x) {}
};

class Final : public Derived1, public Derived2 {
public:
    Final(int x) : Derived1(x), Derived2(x) {}
};
```

Here, `Base` is virtually inherited by both `Derived1` and `Derived2`, and thus `Final` only has a single copy of the Base class's members. Without virtual inheritance, `Final` would inherit two separate copies of `Base`, leading to ambiguity when accessing members of `Base`.

The virtual inheritance resolves this issue by ensuring that only one copy of `Base` exists in `Final`.


### Conclusion
Understanding inheritance in C++—including the different types such as public, protected, and private inheritance—is fundamental to mastering object-oriented design. Additionally, while multiple inheritance can be a powerful feature, it should be used carefully to avoid ambiguities, and virtual inheritance is a key tool for solving those issues.
