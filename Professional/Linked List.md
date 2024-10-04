28/09/2024 18:16

Tags:[[00_DSA]] #pointer #Linked_List 


Status: [[Pointer]] 
CHECK OUT [[Doubley Linked List]]


# Singley Linked List #Singly_Linked_List

>[!Note]
>Difference Between Struct and Classes is that we can't use OOP concepts in Struct.
>
#### Creating  a LinkedList 
```cpp
// Definition of a Node in a singly linked list
struct Node {
  
    // Data part of the node
    int data;

    // Pointer to the next node in the list
    Node* next;

    // Constructor to initialize the node with data
    Node(int data)
    {
        this->data = data;
        this->next = nullptr;
    }
};
```

-----
#### Printing LinkedList #print_linked_list
```cpp
// C++ Function to traverse and print the elements of the linked
// list
void traverseLinkedList(Node* head)
{
    // Start from the head of the linked list
    Node* current = head;

    // Traverse the linked list until reaching the end
    // (nullptr)
    while (current != nullptr) {
      
        // Print the data of the current node
        cout << current->data << " ";

        // Move to the next node
        current = current->next;
    }

    cout << std::endl;
}
```
-----
#### Searching in a LinkedList #search_list 

```cpp
// Function to search for a value in the Linked List
bool searchLinkedList(struct Node* head, int target)
{
    // Traverse the Linked List
    while (head != nullptr) {

        // Check if the current node's
        // data matches the target value
        if (head->data == target) {
            return true; // Value found
        }

        // Move to the next node
        head = head->next;
    }

    return false; // Value not found
}
```
----
#### Finding Length in a LL
```cpp
// Function to search for a value in the Linked List
bool searchLinkedList(struct Node* head, int target)
{
    // Traverse the Linked List
    while (head != nullptr) {

        // Check if the current node's
        // data matches the target value
        if (head->data == target) {
            return true; // Value found
        }

        // Move to the next node
        head = head->next;
    }

    return false; // Value not found
}
```
----
#### Insertion at the Beginning of Singly Linked List #insert_beginning_ll

```cpp
// C++ function to insert a new node at the beginning of the
// linked list
Node* insertAtBeginning(Node* head, int value)
{
    // Create a new node with the given value
    Node* newNode = new Node(value);

    // Set the next pointer of the new node to the current
    // head
    newNode->next = head;

    // Move the head to point to the new node
    head = newNode;

    // Return the new head of the linked list
    return head;
}

```
----

#### Insertion at the End of Singly Linked List

```cpp
// C++ Function to insert a node at the end of the linked
// list
Node* insertAtEnd(Node* head, int value)
{
    // Create a new node with the given value
    Node* newNode = new Node(value);

    // If the list is empty, make the new node the head
    if (head == nullptr) 
        return newNode;

    // Traverse the list until the last node is reached
    
    Node* current = *head;  //Dereferenced *check below 
    
    while (current->next != nullptr) {
        current = current->next;
    }

    // Link the new node to the current last node
    current->next = newNode;
    return head;
}

```

#### **Dereferencing `*head`:**
- `*head` means "dereference the pointer `head`" and access the **node it points to**.
- By dereferencing `head`, you can get to the **actual node** (an object of type `Node`) that contains the data and a pointer to the next node.
- If you only use `head`, you're just manipulating the pointer itself, not the node it refers to. To work with the node, you need to dereference it (`*head`).

 
----


#### Insertion at a Specific Position of the Singly Linked List

```cpp
// Function to insert a Node at a specified position
// without using a double pointer
Node* insertPos(Node* head, int pos, int data)
{
    if (pos < 1) {
        cout << "Invalid position!" << endl;
        return head;
    }

    // Special case for inserting at the head
    if (pos == 1) {        
        Node* temp = new Node(data);
        temp->next = head;
        return temp;
    }

    // Traverse the list to find the node
    // before the insertion point
    Node* prev = head;
    int count = 1;
    while (count < pos - 1 && prev != nullptr) {
        prev = prev->next;
        count++;
    }

    // If position is greater than the number of nodes
    if (prev == nullptr) {
        cout << "Invalid position!" << endl;
        return head;
    }

    // Insert the new node at the specified position
    Node* temp = new Node(data);
    temp->next = prev->next;
    prev->next = temp;

    return head;
}

```
----
#### Deletion at the Beginning of Singly Linked List #delete_beginning
```cpp
// C++ Function to remove the first node of the linked
// list
Node* removeFirstNode(Node* head)
{
    if (head == nullptr)
        return nullptr;

    // Move the head pointer to the next node
    Node* temp = head;
    head = head->next;

    delete temp;

    return head;
}
```
----

#### Deletion at the End of Singly Linked List #delete_end

```cpp
// C++ Function to remove the last node of the linked list
Node* removeLastNode(Node* head)
{
    if (head == nullptr)
        return nullptr;
 
    if (head->next == nullptr) {  //only one element in the LL
        delete head;
        return nullptr;
    }
 
    // Find the second last node
    Node* second_last = head;
    while (second_last->next->next != nullptr)
        second_last = second_last->next;
 
    // Delete last node
    delete (second_last->next);
 
    // Change next of second last
    second_last->next = nullptr;
 
    return head;
}

```
----

#### Deletion at a Specific Position of Singly Linked List

```cpp
// C++ function to delete a node at a specific position
Node* deleteAtPosition(Node* head, int position)
{
    // If the list is empty or the position is invalid
    if (head == nullptr || position < 1) {
        return head;
    }

    // If the head needs to be deleted
    if (position == 1) {
        Node* temp = head;
        head = head->next;
        delete temp;
        return head;
    }

    // Traverse to the node before the position to be
    // deleted
    Node* current = head;
    for (int i = 1; i < position - 1 && current != nullptr;
         i++) {
        current = current->next;
    }

    // If the position is out of range
    if (current == NULL || current->next == nullptr) {
        return;
    }

    // Store the node to be deleted
    Node* temp = current->next;

    // Update the links to bypass the node to be deleted
    current->next = current->next->next;

    // Delete the node
    delete temp;
  
    return head;
}

```
----
#### Reverse a LinkedList #reverse_linked_list

```cpp
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while (curr) {
        ListNode* nextTemp = curr->next;
        curr->next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```
---


### Examples #Rotate_List_K

#### Given a singly linked list, rotate the list to the right by _K_ places.

```cpp
ListNode* rotateList(ListNode* head, int K) {

    if(!head) return NULL;
    if(!K) return head;

    ListNode *tail;
    ListNode *curr=head;
    int n=0;

    while(curr) { //->next != nullptr
        n++;
        tail=curr; //securing the tail pointer
        curr=curr->next;
    }

    K%=n;
    curr=head;

    for(int i = 0; i < n - K - 1; i++) curr = curr->next;

    tail->next = head; //completed the circle
    ListNode *temp = curr->next;
    curr->next = NULL;

    return temp;  
}
```

### Slow Fast Pointer Technique #Slow_Fast_Pointer

**Used to Find the middle element of a LinkedList**
Common mistake:
```cpp
while(pfast!=NULL){
	pslow=pslow->next;
	pfast=pfast->next->next;
}
```
*~here if pfast-> next is NULL then this will result in Compilation Error~*
To avoid that better to use 2 loops and check at each level

```cpp
while(pfast!=NULL){
	pfast=pfast->next;
	while(pfast!=NULL){
		pslow=pslow->next;
		pfast=pfast->next;
	}
}
```

```cpp
while (pfast != nullptr && pfast->next != nullptr) { 
		pfast = pfast->next->next; pslow = pslow->next; 
	}
```

``
### Floyd Cycle detection in a LinkedList #Cycle_LinkedList

```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {}
};

bool hasCycle(ListNode *head) {
    if (!head || !head->next) return false;

    ListNode *slow = head, *fast = head;

    while (fast && fast->next) {
        slow = slow->next;            // Move slow by one
        fast = fast->next->next;      // Move fast by two

        if (slow == fast)             // Cycle detected
            return true;
    }
    return false;                     // No cycle
}
```

### Explanation:

1. **Initialization**: Two pointers, `slow` and `fast`, are initialized at the head.
2. **Traversal**: The `slow` pointer moves one step at a time, while the `fast` pointer moves two steps.
3. **Cycle Detection**: If `slow` and `fast` meet, there is a cycle in the list.
4. **Termination**: If `fast` reaches the end (`nullptr`), then there's no cycle.

This approach works in **O(n)** time and uses **O(1)** space.

### Steps to find the cycle's starting node: #cycle_start_linkedlist

1. **Detect the cycle** using the `slow` and `fast` pointers.
2. Once they meet, **reset one pointer to the head** of the list.
3. Move both pointers **one step at a time** until they meet again. The point where they meet is the start of the cycle.
```cpp
ListNode *detectCycle(ListNode *head) {
    if (!head || !head->next) return nullptr;

    ListNode *slow = head, *fast = head;

    // Detect cycle
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {
            // Cycle detected, reset one pointer to the head
            slow = head;
            while (slow != fast) {
                slow = slow->next;
                fast = fast->next;
            }
            // Both pointers meet at the start of the cycle
            return slow;
        }
    }

    return nullptr;  // No cycle
}
```
**The reason this works because, distance from (fast/slow pointer and head) to the beginning of the cycle i.e $x$ is same.**
fast pointer travels twice the distance to slow pointer, 
- say $x$ is the constant distance from head to the beginning of cycle
- a1 is the number of cycles fast pointer travel
- a2 is the number of cycles slow pointer travel
-  m is the common distance both travel after the finding the beginning of the cycle and meet
- y is the length of the cycle
(Dis fast): $x$+a1y+m = 2x(Dis Slow): 2($x$ +a2y+m)
solving we get $x$=k* y-m (where k is some constant)  # hence proved


### Alternate Folding of LinkedList #Alternate_Folding

The **alternate folding of a linked list** problem involves rearranging a given linked list such that the nodes alternate between the first half and the second half of the list. For example, if the input is `1 -> 2 -> 3 -> 4 -> 5 -> 6`, the output should be rearranged as `1 -> 6 -> 2 -> 5 -> 3 -> 4`.

```cpp
#include <iostream>
using namespace std;

struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

// Function to reverse the linked list
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while (curr) {
        ListNode* nextTemp = curr->next;
        curr->next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}

// Function to fold the linked list in alternate fashion
void alternateFold(ListNode* head) {
    if (!head || !head->next) return;

    // Step 1: Find the middle of the list
    ListNode *slow = head, *fast = head;
    while (fast->next && fast->next->next) {
        slow = slow->next;
        fast = fast->next->next;
    }

    // Step 2: Split the list into two halves
    ListNode* secondHalf = slow->next;
    slow->next = nullptr;  // End of the first half

    // Step 3: Reverse the second half of the list
    secondHalf = reverseList(secondHalf);

    // Step 4: Merge the two halves by alternating
    ListNode* firstHalf = head;
    while (secondHalf) {
        ListNode* temp1 = firstHalf->next;
        ListNode* temp2 = secondHalf->next;
        firstHalf->next = secondHalf;
        secondHalf->next = temp1;
        firstHalf = temp1;
        secondHalf = temp2;
    }
}

// Function to print the linked list
void printList(ListNode* head) {
    while (head) {
        cout << head->val << " ";
        head = head->next;
    }
    cout << endl;
}

int main() {
    // Example list: 1 -> 2 -> 3 -> 4 -> 5 -> 6
    ListNode* head = new ListNode(1);
    head->next = new ListNode(2);
    head->next->next = new ListNode(3);
    head->next->next->next = new ListNode(4);
    head->next->next->next->next = new ListNode(5);
    head->next->next->next->next->next = new ListNode(6);

    cout << "Original List: ";
    printList(head);

    alternateFold(head);

    cout << "Folded List: ";
    printList(head);

    return 0;
}

```


---

### Deep Copy of Random Pointer Copy #Random_Pointer 
### 3 pass Algorithm #3_Pass_Algorithm  #ratt_lo

![[Pasted image 20241001012602.png]]

**main property is `current->next->random = current->random->next`**

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode* random;
    ListNode(int x) : val(x), next(nullptr), random(nullptr) {}
};
ListNode* copyRandomList(ListNode* head) {
    if (!head) return nullptr;

    // Step 1: Insert new nodes after each original node
    ListNode* current = head;
    while (current) {
        ListNode* copy = new ListNode(current->val);
        copy->next = current->next;
        current->next = copy;
        current = copy->next;
    }

    // Step 2: Assign random pointers for the copied nodes
    current = head;
    while (current) {
        if (current->random) {
            current->next->random = current->random->next; //main property
        }
        current = current->next->next;
    }

    // Step 3: Separate the original and copied lists
    current = head;
    ListNode* newHead = head->next;
    ListNode* copy = newHead;

    while (current) {
        current->next = current->next->next;
        if (copy->next) {
            copy->next = copy->next->next;
        }
        current = current->next;
        copy = copy->next;
    }

    return newHead;
}

```

### C++ Code for XOR Linked List:
#### Key Functions:
 **`XOR(Node* a, Node* b)`**
- This function takes two node pointers and returns the XOR of their addresses.
- It uses the `reinterpret_cast` to convert the pointers to an integer type (`uintptr_t`), performs the XOR operation, and then converts the result back to a pointer.

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* npx;  // XOR of next and previous node addresses
};

// XOR of two node addresses
Node* XOR(Node* a, Node* b) {
    return reinterpret_cast<Node*>(
        reinterpret_cast<uintptr_t>(a) ^ reinterpret_cast<uintptr_t>(b)
    );
}

// Insert a node at the beginning
void insert(Node** head, int data) {
    Node* newNode = new Node();
    newNode->data = data;

    // New node is inserted at the beginning, so its npx will be XOR of NULL and the current head
    newNode->npx = XOR(nullptr, *head);

    // If the linked list is not empty, update the npx of the current head
    if (*head != nullptr) {
        Node* next = XOR((*head)->npx, nullptr);
        (*head)->npx = XOR(newNode, next);
    }

    // Update head to the new node
    *head = newNode;
}

// Traverse the XOR linked list
void printList(Node* head) {
    Node* curr = head;
    Node* prev = nullptr;
    Node* next;

    cout << "List: ";
    while (curr != nullptr) {
        cout << curr->data << " ";

        // Get the address of the next node
        next = XOR(prev, curr->npx);

        // Update prev and curr for next iteration
        prev = curr;
        curr = next;
    }
    cout << endl;
}

// Reverse the entire XOR linked list
Node* reverseList(Node* head) {
    Node* curr = head;
    Node* prev = nullptr;
    Node* next;

    // Traverse until the last node
    while (curr != nullptr) {
        next = XOR(prev, curr->npx);
        prev = curr;
        curr = next;
    }

    return prev;  // prev will now point to the new head of the reversed list
}

// Section-wise reverse XOR linked list: Reverse the first k nodes
Node* reverseSection(Node* head, int k) {
    Node* curr = head;
    Node* prev = nullptr;
    Node* next;
    Node* sectionHead = head;
    int count = 0;

    // Reverse the first k nodes
    while (curr != nullptr && count < k) {
        next = XOR(prev, curr->npx);
        curr->npx = XOR(prev, next);
        prev = curr;
        curr = next;
        count++;
    }

    if (sectionHead != nullptr) {
        sectionHead->npx = XOR(prev, curr);
    }

    return prev;  // prev will be the new head of the reversed section
}

int main() {
    Node* head = nullptr;

    insert(&head, 10);
    insert(&head, 20);
    insert(&head, 30);
    insert(&head, 40);
    insert(&head, 50);

    printList(head);

    // Reverse the entire list
    head = reverseList(head);
    cout << "Reversed List: ";
    printList(head);

    // Section-wise reverse (first 3 nodes)
    head = reverseSection(head, 3);
    cout << "Section-wise Reversed (first 3 nodes): ";
    printList(head);

    return 0;
}
```
---

### Reverse K numbers in a list #reverse_k #unsolved 

Given a singly linked list and an integer _K_, reverse the nodes of the list _K_ at a time and returns its head.

It's guaranteed that the _K_ is divisible by the length of the linked list
```cpp

ListNode* reverse(ListNode* head, int K) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    ListNode* nextTemp = nullptr;
    int count = 0;

    // Reverse 'K' nodes or till the end of the list

    while (curr != nullptr && count < K) {
        nextTemp = curr->next;
        curr->next = prev;
        prev = curr;
        curr = nextTemp;
        count++;
    }

    // Return the new head (which was the Kth node from the original list)
    return prev;
}

ListNode* reverseList(ListNode* head, int K) {
if (head == nullptr || K == 1) return head;
  
    ListNode* curr = head;
    ListNode* prevTail = nullptr;
    ListNode* newHead = nullptr;
  
    // While there are still nodes left in the list
    while (curr != nullptr) {
        ListNode* groupHead = curr;  // Starting point of the current group
        ListNode* groupTail = curr;  // To keep track of the last node of the group
        int count = 0;
  
        // Check if there are K nodes in the current group
        while (count < K && groupTail != nullptr) {
            groupTail = groupTail->next;
            count++;
        }

        // If less than K nodes left, no need to reverse
        if (count < K) break;
  
        // Reverse the current group of K nodes
        ListNode* reversedHead = reverse(groupHead, K);
  
        // Set the new head if it's the first group
        if (newHead == nullptr) {
            newHead = reversedHead;
        }
  
        // Link the previous group's tail to the reversed head of the current group
        if (prevTail != nullptr) {
            prevTail->next = reversedHead;
        }
  
        // Update prevTail to the original head of the current group, which is now the last node
        prevTail = groupHead;
  
        // Move the current pointer to the next grou[]
        curr = groupTail;

    }
  
    // If there are remaining nodes that weren't reversed, link them to the last reversed group
    if (prevTail != nullptr) {
        prevTail->next = curr;
    }
  
    return newHead;
  
}

```

---
### Merge K sorted Linked List #Merge_K_LinkedList

```cpp
// Function to merge two sorted linked lists into one sorted linked list
ListNode *mergeFunc(ListNode *h1, ListNode *h2) {

    ListNode *nHead = nullptr;  // to store the head of the merged linked list
    ListNode *cur = nullptr, *temp = nullptr;  //  to traverse and merge lists

    // If one of the lists is empty, return the other one
    if (h1 == nullptr) {
        return h2;
    }
    if (h2 == nullptr) {
        return h1;
    }

    // Loop through both lists until one of them is fully traversed
    while (h1 && h2) {
        // Compare the current nodes of both lists and pick the smaller one
        if (h1->val <= h2->val) {
            temp = h1;  // Select node from the first list
            h1 = h1->next;  // Move to the next node in the first list
        } else {
            temp = h2;  // Select node from the second list
            h2 = h2->next;  // Move to the next node in the second list
        }

        // If this is the first node being added to the merged list, set nHead and cur
        if (nHead == nullptr) {
            cur = temp;  // Current node becomes the first node
            nHead = cur;  // Initialize the head of the merged list
        } else {
            cur->next = temp;  // Link the selected node to the merged list
            cur = cur->next;  // Move the current pointer to the next node
        }
    }

    // If there are remaining nodes in h1, append them to the merged list
    while (h1) {
        cur->next = h1;  // Link the remaining nodes from h1
        cur = cur->next;  // Move the current pointer to the next node
        h1 = h1->next;  // Move to the next node in h1
    }

    // If there are remaining nodes in h2, append them to the merged list
    while (h2) {
        cur->next = h2;  // Link the remaining nodes from h2
        cur = cur->next;  // Move the current pointer to the next node
        h2 = h2->next;  // Move to the next node in h2
    }

    // Return the head of the merged linked list
    return nHead;
}

// Function to merge K sorted linked lists
ListNode *mergeKLists(vector<ListNode *> head) {
    // If there's only one list, return it directly
    if ((int)head.size() == 1) {
        return head[0];
    }

    ListNode *mergeHead = nullptr;  // Pointer to store the head of the merged list

    // Iterate over the list of heads, merging one list at a time
    for (int i = 0; i < (int)head.size(); i++) {
        mergeHead = mergeFunc(mergeHead, head[i]);  // Merge the current list with the previously merged list
    }

    // Return the head of the fully merged list
    return mergeHead;
}

}
```

>[!Learn]
>Keep a `temp` pointer, to save the solved LL
>Keep a `curr` pointer, to build the required LL using the `temp` pointer



