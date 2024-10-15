13/10/2024 18:39

Tags:

Status:[[00_DSA]]

# Recursion


### Tower of Hanoi #Hanoi

```cpp
#include <iostream>
using namespace std;

void towerOfHanoi(int n, char from_rod, char to_rod, char aux_rod) {
    if (n == 1) {
        cout << "Move disk 1 from rod " << from_rod << " to rod " << to_rod << endl;
        return;
    }
    towerOfHanoi(n - 1, from_rod, aux_rod, to_rod);
    cout << "Move disk " << n << " from rod " << from_rod << " to rod " << to_rod << endl;
    towerOfHanoi(n - 1, aux_rod, to_rod, from_rod);
}

int main() {
    int n;
    cout << "Enter the number of disks: ";
    cin >> n;
    towerOfHanoi(n, 'A', 'C', 'B');  // A, B, C are names of the rods
    return 0;
}

```
### Explanation:

- The function `towerOfHanoi()` moves `n` disks from the source rod (`from_rod`) to the destination rod (`to_rod`) using the auxiliary rod (`aux_rod`).
- The base case occurs when `n == 1`, where we directly move the disk.
- For more than 1 disk, the function calls itself recursively:
    1. Move `n-1` disks from the source rod to the auxiliary rod.
    2. Move the nth disk from the source rod to the destination rod.
    3. Move the `n-1` disks from the auxiliary rod to the destination rod.


---
# LCCM Framework (level, choice ,check, move) #lccm


**Level**   :    how to make progress?
       :    how do we incrementally build solution?
       :    say going row wise

**N Queens** 
![[Pasted image 20241014002141.png]]

![[Pasted image 20241014002432.png]]
```cpp  
int queen[8];

bool check(int row, int col, string str[8]){
    if (str[row][col] == '*') return false; // Check for blocked cell
    for(int prev = 0; prev < row; prev++){
        int pcol = queen[prev];
        if (pcol == col || abs(row - prev) == abs(col - pcol)) return false;
    } //same column and same slope for same diagonal
    return true;
}
  

int rec(string str[8], int lvl){
    if(lvl==8){   //base case
        return 1;
    }
    int ans=0;   //while checking for wach level, if we don't hit the base case then the ans variable is set back to 0

    for(int col=0;col<8;col++){
        if(str[lvl][col]=='.'){
            if(check(lvl,col,str)){
                queen[lvl]=col;   //queen changed because we are using it in check function
                ans+=rec(str,lvl+1);
                queen[lvl]=-1;     //queen reseted
            }   }
    }
    return ans;
}
```