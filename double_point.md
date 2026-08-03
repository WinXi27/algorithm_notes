# double_point_leetcode专项练习
保证代码都AC
先看目录
# 5. 最长回文子串
## 动态规划 解法
```cpp
class Solution {
public:
    bool dp[1005][1005];
    string longestPalindrome(string s) {
        if(s.empty() )return "";
        string ans=s.substr(0,1);
        for(int i=0;i<s.size();++i)dp[i][i]=true;
        for(int len=2;len<=s.size();++len){
            for(int i=0;i<=s.size()-len;++i){
                int j=i+len-1;
                //[i][j]
                if(s[i]==s[j]){
                    if(len>2)
                        dp[i][j]=dp[i+1][j-1];
                    else dp[i][j]=true;
                }else dp[i][j]=false;
                //[i][j]
                if(dp[i][j]){
                    ans=s.substr(i,len);
                }
            }
        }
        return ans;
    }
};
```
## 中心拓展 解法
```cpp
class Solution {
public:
    string ans;
    void expand_around_center(string s,int i,int j){
        while(i>=0&&j<s.size()&&s[i]==s[j]){
            i--,j++;
        }
        if(ans.size()<j-i-1){
            ans=s.substr(i+1,j-i-1);
        }return ;
    }
    string longestPalindrome(string s) {
        if(s.empty() )return "";
        for(int i=0;i<s.size();++i){
            expand_around_center(s,i,i);
            if(i+1<s.size() )expand_around_center(s,i,i+1);
        }
        return ans;
    }
};
```
# 11. 盛最多水的容器
```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int l=0;
        int r=height.size()-1;
        int maxx=0;
        while(l<=r){
            maxx=max(maxx,min(height[l],height[r])*(r-l) );
            if(height[l]<height[r]){
                l++;
            }else{
                r--;
            }
        }
        return maxx;
    }
};
```
# 15. 三数之和
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        if(nums.size()<3)return {};
        vector<vector<int>>ans;
        sort(nums.begin(),nums.end() );
        for(int i=0;i<=(int)nums.size()-3;++i){
            if(i-1>=0&&nums[i]==nums[i-1])continue;
            int target=-nums[i];
            //在[i+1,num.size() )找不重复的数据nums[j]和nums[k]使得nums[j]+nums[k]=target 
            int l=i+1;   
            int r=nums.size()-1;  
            if(nums[i]+nums[i+1]+nums[i+2]>0)break;
            if(nums[i]+nums[nums.size()-2]+nums.back()<0 )continue;
            while(l<r){  
                int temp=nums[l]+nums[r]; 
                if(temp>target){  
                    r--;  
                }else if(temp<target){  
                    l++;  
                }else {
                    ans.push_back({nums[i],nums[l],nums[r]});
                    l++,r--;  
                    while(l<r&&nums[l]==nums[l-1])l++;
                    while(l<r&&nums[r]==nums[r+1])r--;
                }
            }  
        } 
        return ans;
    }
};
```
# 16. 最接近的三数之和
```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        if(nums.size()<3)return 0;
        sort(nums.begin(),nums.end() );
        int sum=nums[0]+nums[1]+nums[2];
        for(int i=0;i<=nums.size()-3;++i){
            if(i-1>=0&&nums[i]==nums[i-1])continue;
            int l=i+1;
            int r=nums.size()-1;
            while(l<r){
                int temp=nums[i]+nums[l]+nums[r];
                if(abs(temp-target)<abs(sum-target) ){
                    sum=temp;
                }
                if(temp>target){
                    r--;
                }else if(temp==target){
                    return target;
                }else if(temp<target){
                    l++;
                }
            }
        }
        return sum;
    }
};
```
# 18. 四数之和
```cpp
class Solution {
public:
#define ll long long 
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        if(nums.size()<4)return {};
        vector<vector<int>>ans; 
        sort(nums.begin(),nums.end() );
        for(int i=0;i<=nums.size()-4;++i){
            if(i-1>=0&&nums[i]==nums[i-1])continue;
            ll target1=target-nums[i];
            //[i+1,nums.size() )找三元组和为target1
            for(int j=i+1;j<=nums.size()-3;++j){
                if(j-1>=i+1&&nums[j]==nums[j-1])continue;
                ll target2=target1-nums[j];
                int l=j+1;
                int r=nums.size()-1;
                while(l<r){
                    int temp=nums[l]+nums[r];
                    if(temp==target2){
                        ans.push_back({nums[i],nums[j],nums[l++],nums[r--]});
                        while(l<r&&nums[l]==nums[l-1])l++;
                        while(l<r&&nums[r]==nums[r+1])r--;
                    }else if(temp>target2){
                        r--;
                    }else if(temp<target2){
                        l++;
                    }
                }
            }
        }
        return ans;
    }
};
```
# 19. 删除链表的倒数第 N 个结点
```cpp
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
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode*dummy=new ListNode();
        dummy->next=head;
        ListNode*fast=dummy;
        ListNode*slow=dummy;
        while(n--){
            fast=fast->next;
            if(fast==nullptr)return head;
        }
        while(fast->next){
            fast=fast->next;
            slow=slow->next;
        }
        slow->next=slow->next->next;
        return dummy->next;

    }
};
```
# 26. 删除有序数组中的重复项
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int stack_sz = 0;
        for (int i = 0; i < nums.size(); ++i) {
            if (i - 1 >= 0 && nums[i] == nums[i - 1])
                continue;
            nums[stack_sz++] = nums[i];
        }
        return stack_sz;
    }
};
```
# 27. 移除元素
## 双指针 解法
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        if(nums.empty() )return 0;
        int r=nums.size()-1;
        int l=0;
        while(l<=r){
            while(l<nums.size()&&nums[l]!=val)l++;
            while(r>=0&&nums[r]==val)r--;
            if(l<=r){
                swap(nums[l++],nums[r--]);
            }
        }
        return l;
    }
};
```
## stack 解法
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int stack_sz=0;
        for(int i=0;i<nums.size();++i){
            if(nums[i]!=val)
                nums[stack_sz++]=nums[i];
        }
        return stack_sz;
    }
};
```
# 28. 找出字符串中第一个匹配项的下标
```cpp
class Solution {
public:
    void fun(vector<int>&next,string s){
        for(int i=0;i<next.size();++i){
            if(i==0)next[i]=-1;
            else if(i==1)next[i]=0;
            else{
                int j=i-1;
                //[ j,i]
                while(next[j]!=-1&&s[next[j]]!=s[i-1])j=next[j];
                next[i]=next[j]+1;
            }
        }
    }
    int strStr(string s, string t) {
        vector<int> next(t.size(), 0);
        fun(next,t);
        int i=0;
        int j=0;
        while(i<s.size()&&j<t.size() ){
            if(s[i]==t[j])i++,j++;
            else{
                if(next[j]==-1)i++;
                else j=next[j];
            }
        }
        //[i-t.size(),i)
        if(j==t.size() )return i-(int)t.size(); 
        return -1;
    }
};
```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```

# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
# 
```cpp

```
