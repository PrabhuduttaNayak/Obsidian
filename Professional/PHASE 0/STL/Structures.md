21/09/2024 11:25

Tags: #Structures

Status:

# Structures


```cpp

#include <bits/stdc++.h>
using namespace std;

struct Person {
    // These are members of the struct
    string name;
    int age;
    char gender;

    // This is a member method or function of the struct
    void printDetails() {
        cout << "Name: " << name << endl;
        cout << "Age: " << age << endl;
        cout << "Gender: " << gender << endl;
    }
};

int main() {
    Person p1;
    // Method 1 of initialization
    p1.name = "Hritik";
    p1.age = 49;
    p1.gender = 'M';

    // Method 2
    Person p2 = {"Mary", 25, 'F'}; // Order must be the same as declaration

    // Printing method
    cout << "Details of p1" << endl;
    cout << "Name: " << p1.name << endl;
    cout << "Age: " << p1.age << endl;
    cout << "Gender: " << p1.gender << endl;
    cout << endl;

    cout << "Details of p2" << endl;
    p2.printDetails(); // Member function calling

    return 0;
}

_________
```
```
#OutPut

Details of p1
Name: Hritik
Age: 49
Gender: M

Details of p2
Name: Mary
Age: 25
Gender: F

```

### Nested Structures

Structures can also be nested inside other structures.


```cpp
struct Address {
    string street;
    string city;
    int zipCode;
};

struct Person {
    string name;
    int age;
    Address address;  // Nested structure
};```

### Arrays of Structures

You can also create arrays of structures to store multiple instances of the same structure type.

```cpp
Person people[3] = {
    {"Alice", 25, 5.5},
    {"Bob", 28, 6.1},
    {"Charlie", 32, 5.8}
};

for (int i = 0; i < 3; i++) {
    cout << people[i].name << " is " << people[i].age << " years old." << endl;
}```

### Struct vs [[Class]]

The main difference between `struct` and `class` is the **default access modifier**:

- In a **struct**, members are **public** by default.
- In a **class**, members are **private** by default.

Structs are often used for **simple data containers**, while classes are used for more complex functionality that includes methods and private data members.

### References