![[Pasted image 20241002092001.png]]
[[00_DSA]] [[Time Complexity]]  [[Recursion]]
#merge_sort
## Complexity
|  | Best | Worst |
| - | -| - |
| Time | $O(n \cdot log  \cdot n)$ | $O(n \cdot log  \cdot n)$ |
| Space | | $O(n)$ |
```cpp
#include <bits/stdc++.h>
using namespace std;

// Merges two subarrays of arr[].
// First subarray is arr[left..mid]
// Second subarray is arr[mid+1..right]
void merge(vector<int>& arr, int left, 
                     int mid, int right)
{
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // Create temp vectors
    vector<int> L(n1), R(n2);

    // Copy data to temp vectors L[] and R[]
    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];

    int i = 0, j = 0;
    int k = left;

    // Merge the temp vectors back 
    // into arr[left..right]
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        }
        else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // Copy the remaining elements of L[], 
    // if there are any
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    // Copy the remaining elements of R[], 
    // if there are any
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

// begin is for left index and end is right index
// of the sub-array of arr to be sorted
void mergeSort(vector<int>& arr, int left, int right)
{
    if (left >= right)
        return;

    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}

// Function to print a vector
void printVector(vector<int>& arr)
{
    for (int i = 0; i < arr.size(); i++)
        cout << arr[i] << " ";
    cout << endl;
}

// Driver code
int main()
{
    vector<int> arr = { 12, 11, 13, 5, 6, 7 };
    int n = arr.size();

    cout << "Given vector is \n";
    printVector(arr);

    mergeSort(arr, 0, n - 1);

    cout << "\nSorted vector is \n";
    printVector(arr);
    return 0;
}

```

### Merge Sort in LinkedList #unsolved [[Linked List]]

```cpp
#include <bits/stdc++.h>
using namespace std;

struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};
  
void print(ListNode * head){
    cout<<"head-\n";
    while(head){
        cout<<head->val<<" ";
        head=head->next;
    }
    cout<<"\n";
}

ListNode* mergesort(ListNode* head) {
    // Base case: If the list is empty or has only one node, return it as it's already sorted
    if (!head || head->next == nullptr) return head;
    
    // Initialize two pointers for the fast-slow technique
    ListNode* slow = head;
    ListNode* fast = head->next;

    // Using the fast-slow pointer technique to find the middle of the list
    while (fast) {
        fast = fast->next;  // Fast moves two steps
        if (fast) {         // Check to prevent null pointer dereference
            slow = slow->next;  // Slow moves one step
            fast = fast->next;  // Fast moves another step
        }
    }

    // Split the list into two halves
    fast = slow->next;        // fast now points to the start of the second half
    slow->next = nullptr;     // Disconnect the first half from the second half

    // Recursively sort both halves
    slow = mergesort(head);   // Sort the first half
    fast = mergesort(fast);   // Sort the second half

    // Initialize a new list for merging the sorted halves
    head = nullptr;
    ListNode* temp = nullptr;

    // Merge the two sorted lists
    while (slow && fast) {
        if (slow->val <= fast->val) {    // Compare values from both halves
            if (!temp) {                 // If the new list is empty, set the head and temp to slow
                head = slow;
                temp = slow;
                slow = slow->next;
            } else {                     // Otherwise, append slow to the new list
                temp->next = slow;
                temp = slow;
                slow = slow->next;
            }
        } else {
            if (!temp) {                 // If the new list is empty, set the head and temp to fast
                head = fast;
                temp = fast;
                fast = fast->next;
            } else {                     // Otherwise, append fast to the new list
                temp->next = fast;
                temp = fast;
                fast = fast->next;
            }
        }
    }

    // Append the remaining nodes (if any) from either list to the merged list
    if (fast) temp->next = fast;
    if (slow) temp->next = slow;

    // Return the head of the merged sorted list
    return head;
}


  
  

ListNode* GetList(vector<int> &num) {

    ListNode* head = nullptr;

  

    if(num.empty()) {

        return head;

    }

  

    ListNode* cur = head;

    for(int i  = 0; i < (int)num.size(); i++) {

        ListNode* temp = new ListNode(num[i]);

        if(!cur) {

            cur = temp;

            head = cur;

        }

        else {

            cur->next = temp;

            cur = temp;

        }

    }

    return head;

}

  

int main()

{

    ios_base::sync_with_stdio(false);

    cin.tie(0); cout.tie(0);

  

    int n;

    cin >> n;

  

    vector<int> num;

    for(int i = 0; i < n; i++) {

        int x;

        cin >> x;

        num.push_back(x);

    }

  

    ListNode* head = GetList(num);

  

    head = mergesort(head);

  

    while(head) {

        cout << head->val << " ";

        head = head->next;

    }

    cout << "\n";

    return 0;

}
```