```
[["a-b","2-1"]
 ["b-c","3-2"]
]
```

```c++
int maxsubarraywithk(vector<int>& nums, int k){
    int n = nums.size();
    unordered_map<int, int> m;
    m[0] = 0;
    int sum = 0;
    int max_len = 0;
    
    for(int i=0; i<n;i++){
        sum += nums[i];
        auto iter = m.find(sum - k);
        if(iter!= m.end()){
            max_len = max(max_len, i - iter->second);
        }
        m[sum]=i;
    }
    return max_len;
}
```

