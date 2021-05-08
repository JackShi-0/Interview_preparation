# 阿里：

## 1318，或运算的最小翻转次数

```c++
int minFlips(int a, int b, int c){
    int ans = 0;
    
    for(int i=0; i< 31 ; ++i){
        int bit_a = (a >> i) & 1;
        int bit_b = (b >> i) & 1;
        int bit_c = (c >> i) & 1;
        if(bit_c == 0){
            ans += bit_a + bit_b;//bit_a和bit_b都必须为0
        }else{
            ans += (bit_a + bit_b == 0);//bit_a和bit_b至少有一个为1//只有他们都为0，才需要一次翻转
        }
    }
    return ans;
    
    while(a || b || c){
        int low_a = a&1, low_b = b&1, low_c = c&1;
        if(low_c == 0)
            ans += low_a + low_b;
        else if(low_c == 1){
            ans += (low_a == 0 && low_b == 0);
        }
        a>>=1, b>>=1, c>>=1; 
    }
    return ans;
}
```

