20/10/2024 10:41

Tags: [[Sieve of Eratosthenes]] [[00_DSA]] 

Status: #factorization #prime_factors

# Fast Factorization

We will use precomputation to help us calculate faster

Maximum number of factors a number can have is log2(N)  (2 is base) because N=2^count

- We precompute all the smallest prime factors of all numbers
```cpp
for(int i=2;i<=N;i++){
	sp[i]=i;  //initially all are equal to index
}
for(int i=2;i<=n;i++){
	if(sp[i]==i){
		for(int j=2*i;j<=N;j+=i){ //here we make them equal to their minimum factor
			if(sp[j]==j)sp[j]=i;	
		}
	}  //TC is O(NloglogN) becuase its like sieve here
}
```

- Now we factorize the number using the sp[] array 
```cpp
vector<int> primeFact(int x){
	vector<int> ans;
	while(x>1){
		ans.push_back(sp[x]);
		x/=sp[x];
	}
	return ans;  //Tc is O(logN) becuase maximum log(N) factors possible
}
```

### Examples