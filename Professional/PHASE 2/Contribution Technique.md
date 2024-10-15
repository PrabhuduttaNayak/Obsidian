2024-10-07 14:32

Tags: #sliding_window 

Status: [[Sliding Window]] [[Strings]] [[00_DSA]]

>[!Tip]
> - Such questions are generally solved by Permutation Counting
> - Count contribution of each letter and add to the total answer
> - Keep a `Prev_Occ[]` vector, could be 1D or 2D
#### Description #Question_Type #Count_distinct_Char_SubString 
[[Contribution Technique]]
#unsolved #Contribution_Technique #Counting

Given a string S consisting of the lowercase character of length N. Score of a string is the number of distinct characters present in the string. Like the score of "character" is 6.

Find the sum of the score of all substring of S.

##### Input Format

The first line contains _T_, the number of test cases _(1<=T<=10)_.

The first line of each test case contains an integers _N,_ size of the string, _1<=N<=10^5._

The second line of each test case contains a string _S_ of length _N_.

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
signed main(){
    ios_base::sync_with_stdio(false);cin.tie(0);cout.tie(0);;
    ll testcases;
    cin>>testcases;
    while(testcases--){
        ll n;
        cin>>n;
        string s;
        cin>>s;
        
        ll prev[26];
        for(ll i=0;i<26;i++)
            prev[i] = -1;
            
        long long ans = 26*((n*(n+1))/2);
        for(ll i=0;i<n;i++){
            ll lenNotWithChar = i - prev[s[i]-'a']-1;
            ans = ans - ((lenNotWithChar*(lenNotWithChar+1))/2); //nC2 between 0-char and char - char
            prev[s[i]-'a'] = i;
        }
        
        for(ll i=0;i<26;i++){
            ll lenNotWithChar = n - prev[i]-1;
            ans = ans - ((lenNotWithChar*(lenNotWithChar+1))/2); //nC2 between char and n 
        }
        cout<<ans<<"\n";
    }
    
}```

We need to calculate the contribution of each letter from ‘a’ to ‘z’ to the final answer. For the answer, we will consider the case for ‘a’, you need to do the same for each letter from ‘a’ to ‘z’. If the count of ‘a’ is anything more than 0 in a subarray, it would contribute 1 to the final answer (since we are taking count of distinct characters). So, we can first add the total number of subarrays, N*(N+1)/2 to the final answer and then subtract those subarrays which don’t have the character ‘a’ in it. This would give us the contribution for ‘a’ in the final answer. Do the same for all characters from ‘a’ to ‘z’.

`Time Complexity per test case: O(N)

----

### Count Unique Char in Substrings #Question_Type #Count_Unique_Char_SubString 
#unsolved #Contribution_Technique #Counting


##### Description

Given a string _S_ consisting of the lowercase character of length N. Score of a string is the number of unique characters present in the string( characters which are only present once in the string). Like score of "character" is 3 {h,t,e}.

Find the sum of the score of all substring of S.

##### Input Format

The first line contains _T_, the number of test cases _(1<=T<=10)_.

The first line of each test case contains an integers _N,_ size of the string, _1<=N<=10^5._

The second line of each test case contains a string _S_ of length _N_.
>[!Hint]
>We need to calculate the contribution of each letter from ‘a’ to ‘z’ to the final answer. For the answer, we will consider the case for ‘a’, you need to do the same for each letter from ‘a’ to ‘z’. For a particular ‘a’ to contribute to the final answer, it should exist only once in the subarray (that is how it will become unique). So to calculate the number of subarrays having that ‘a’ only once, we can find its closest left ‘a’ and its closest right ‘a’. The left point of the subarray should be between the closest left ‘a’ and the current ‘a’ and similarly, the right point of the subarray should be between the current ‘a’ and the closest right ‘a’. To find the number of subarrays including current ‘a’, we can multiply the possible values of the left and right points. Do this for each character ‘a’ present in the string and then the same for each character from ‘a’ to ‘z’.

Time Complexity per test case: O(N)
```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
signed main(){
   ios_base::sync_with_stdio(false);cin.tie(0);cout.tie(0);;
   ll testcases;
   cin>>testcases;
   while(testcases--){
       ll n;
       cin>>n;
       string s;
       cin>>s;
       
       vector < ll >  occurence[26]; //2D vector
       
       for(ll i=0;i<26;i++)
           occurence[i].push_back(-1);        //first occ   
       for(ll i=0;i<n;i++)
           occurence[s[i]-'a'].push_back(i); //all middle occ
       for(ll i=0;i<26;i++)
           occurence[i].push_back(n);        //last occ
           
       ll ans = 0;
       for(ll i=0;i<26;i++){
           for(ll j=1;j<(int)occurence[i].size()-1;j++){
               ans+=(occurence[i][j]-occurence[i][j-1])*(occurence[i][j+1]-occurence[i][j]); //multiplying both unique sides i.e mxn
           }
       }
       cout<<ans<<"\n";
   }
}```

---
