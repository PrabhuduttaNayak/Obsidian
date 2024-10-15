[[00_DSA]]   [[Linked List]]
#Linked_List #doubley_Linked_List [[Pointer]]

## Declaration

```cpp
struct Node {

    // To store the Value or data.
    int data;

    // Pointer to point the Previous Element
    Node* prev;

    // Pointer to point the Next Element
    Node* next;
  
    // Constructor
    Node(int d) {
       data = d;
       prev = next = nullptr;      
    }
};
```
----

