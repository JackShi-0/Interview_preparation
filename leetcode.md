![image-20201221193830407](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20201221193830407.png)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
    int n = nums.size(); 
    for(int i=0;i<n;i++){
         for (int j=i+1;j<n;j++){
              if (nums[i]+nums[j]==target){
                 return {i,j};
                }
            }
    }return{};
    }
};//暴力解法

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> hashtable;
        for (int i = 0; i < nums.size(); ++i) 
           {
            auto it = hashtable.find(target - nums[i]);
            if (it != hashtable.end()) {
                return {it->second, i};
            }
            hashtable[nums[i]] = i;//填入hash表
         }
        return {};
    }
};//hash表
```

![image-20201228190711617](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20201228190711617.png)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
// class Solution {
// public:
//     ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
//     ListNode *head = nullptr, *tail = nullptr;
//     int carry = 0;
//     while (l1 || l2){
//         int n1 = l1 ? l1->val : 0;
//         int n2 = l2 ? l2->val : 0;
//         int sum = n1 + n2 + carry;
//         if (!head){
//             head = tail = new ListNode(sum % 10);
//         }else{
//             tail->next = new ListNode( sum % 10);
//             tail = tail->next;
//         }
//         carry = sum /10;
//         if(l1){l1 = l1->next;}
//         if(l2){l2 = l2->next;}
//     }   
//     if (carry>0){
//         tail->next = new ListNode(carry);
//     }
//     return head;
//     }
// };

class Solution{
    public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2){
        return dfs(l1,l2,0);

    }

    ListNode* dfs(ListNode* l,ListNode* r, int i){
        if(!l && !r && !i) {return nullptr;}
        int sum =(l?l->val:0)+(r?r->val:0)+i;
        ListNode* node = new ListNode(sum % 10);
        node->next = dfs((l?l->next:nullptr),(r?r->next:nullptr),sum/10);
        return node;
    } //递归法
};
```

![image-20201110211838415](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20201110211838415.png)

c

```c
void swap(int *a, int *b) {
  int t = *a;
  *a = *b, *b = t;
}

void reverse(int *nums, int left, int right)

 {   while (left < right) 
          {  swap(nums + left, nums + right);
           left++, right--;  }

  }



void nextPermutation(int *nums, int numsSize) {
  int i = numsSize - 2;
  while (i >= 0 && nums[i] >= nums[i + 1]) {
  i--;
  }
  if (i >= 0) {
      int j = numsSize - 1;
      while (j >= 0 && nums[i] >= nums[j]) {
      j--;}    
      swap(nums + i, nums + j);
}
reverse(nums, i + 1, numsSize - 1);
}


```

c++

```c++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int i = nums.size() - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;
        }
        if (i >= 0) {
            int j = nums.size() - 1;
            while (j >= 0 && nums[i] >= nums[j]) {
                j--;
            }
            swap(nums[i], nums[j]);
        }
        reverse(nums.begin() + i + 1, nums.end());
    }
};
```



![image-20201228212610517](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20201228212610517.png)

```c++
class Solution {
private:
    vector<vector<string>> result;
    vector<string> path;
    void backtracking(const string& s, int startindex){
       if(startindex>= s.size()){
           result.push_back(path);
           return;
       }
       for(int i=startindex; i<s.size();i++){
           if(ispalindrome(s,startindex,i)){
               string str = s.substr(startindex,i-startindex+1);
               path.push_back(str);
           }else{
               continue;
           }
            backtracking(s,i+1);
           path.pop_back();//回溯，撤销处理结果
       } 
               
    }

    bool ispalindrome(const string& s, int start, int end){
    for(int i=start,j=end ; i<j; i++,j--){
        if (s[i]!=s[j]){
            return false;
        }
    }
    return true;
    } //判断是否为回文数
    
public:
    vector<vector<string>> partition(string s) {
        result.clear();
        path.clear();
        backtracking(s,0);
        return result;

    }
}; ////回溯法经典例子
```

![image-20201228212810626](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20201228212810626.png)

```c++
class LRUCache {
public:
    LRUCache(int capacity) {
    capacity_ = capacity;
    }
    
    int get(int key) {
        const auto it = m_.find(key);
        if(it == m_.cend()) return -1;
        cache_.splice(cache_.begin(),cache_,it->second);
        return it->second->second;
    }
    
    void put(int key, int value) {
        const auto it = m_.find(key);

        if(it != m_.cend()){
            it->second->second = value;
            cache_.splice(cache_.begin(),cache_,it->second);return;
        }
        
        if(cache_.size()==capacity_){
            const auto& node = cache_.back();
            m_.erase(node.first);
            cache_.pop_back();
        }

        cache_.emplace_front(key,value);
        m_[key] = cache_.begin();
    }
private:
    int capacity_;
    list<pair<int,int>> cache_;
    unordered_map<int,list<pair<int,int>>::iterator> m_;
};
```

![image-20201112214646636](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20201112214646636.png)

```c
void swap(int *a, int *b){
    int t;
    t=*a;
    *a=*b;*b=t;
}

int* sortArrayByParityII(int* A, int ASize, int* returnSize){
int j=1;
for (int i=0;i<ASize;i=i+2){
    if(A[i]% 2==1){
        while(A[j]%2==1){j=j+2;}
        swap(A+i,  A+j);
    }
}
*returnSize= ASize;
return A;
}
```

![image-20201228190430438](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20201228190430438.png)

```c++
// class Solution {
// public:
//     string convertToBase7(int num) {
//     if (num<0) return "-" + convertToBase7(-1*num);
//     if (num<7) return to_string(num);
//     return convertToBase7(num/7) + to_string(num%7);
//     }
// }; //递归法


class Solution{
    public:
    string convertToBase7(int num) {
        if (num ==0) return "0";
        else{
            string str = "";
            bool flag = false;
            if (num <0) { flag = true; num=-num;}
            while (num){
                str.insert(0,1, char(num % 7 +48)); 
                //str += to_string(num % 7);
                num = num/7;
            }
            if(flag){
                str.insert(0,1,'-');
                //str += '-';
            }
            //reverse(str.begin(),str.end());
            return str;
        }
    }
}; //字符串的用法
```

![image-20201229122421727](C:\Users\99797\AppData\Roaming\Typora\typora-user-images\image-20201229122421727.png)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
// class Solution {
// public:
//     ListNode* reverseList(ListNode* head) {
//      ListNode* cur = NULL , *pre = head;
//      while(pre!=NULL){
//          ListNode* t = pre->next; //保存结点
//          pre->next =cur; //翻转
//          // 更新两个指针
//          cur = pre;
//          pre = t;
//      }
//      return cur;
//     }
// };

class Solution{
    public:
    ListNode* reverseList(ListNode* head){
      if ( head == NULL || head->next == NULL){return head;}
      ListNode* ret = reverseList(head->next);
      head->next->next = head; 
      head->next = NULL;
      return ret;
    }
};
```

