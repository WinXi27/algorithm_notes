# 下面是基础题
# 34. 在排序数组中查找元素的第一个和最后一个位置
```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int l=0;//  小于target的数据  l   r 大于等于target的数据
        int r=nums.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<target){
                l=mid+1;
            }else{
                r=mid-1;
            }
        }
        if(l>=0&&l<nums.size()&&nums[l]==target){
            int begin=l;
            while(begin+1<nums.size()&&nums[begin]==nums[begin+1])begin++;
            return {l,begin};
        }
        return {-1,-1}; 
    }
};
```
# 35. 搜索插入位置
```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        //小于 l r 大于等于
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<target)l=mid+1;
            else r=mid-1;

        }
        return l;
    }
};
```
# 704. 二分查找
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l=0;
        int r=nums.size()-1;
        //小于 l r 大于等于
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<target)l=mid+1;
            else r=mid-1;
        }
        if(l>=0&&l<nums.size()&&nums[l]==target)return l;
        return -1;
    }
};
```
# 744. 寻找比目标字母大的最小字母
```cpp
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        //小于等于 l r 大于
        int l=0;
        int r=letters.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(letters[mid]<=target)l=mid+1;
            else r=mid-1;
        }
        if(l>=0&&l<letters.size())return letters[l];
        return letters[0];
    }
};
```
# 2529. 正整数和负整数的最大计数
```cpp
class Solution {
public:
    int maximumCount(vector<int>& nums) {
        //  小于0  l  r  大于等于0
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<0)l=mid+1;
            else r=mid-1;
        }
        int fushu=l;
        while(l<nums.size()&&nums[l]==0)l++;
        return max(fushu,(int)nums.size()-l);
    }
};
```
# 下面是进阶题目
# 1385. 两个数组间的距离值
```cpp
class Solution {
public:
    int fun(vector<int>& arr2, int target) {
        int l = 0;
        int r = arr2.size() - 1;
        // 小于等于 l r 大于
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (arr2[mid] <= target)
                l = mid + 1;
            else
                r = mid - 1;
        }
        return r;
    }
    int findTheDistanceValue(vector<int>& arr1, vector<int>& arr2, int d) {
        sort(arr2.begin(), arr2.end());
        int ans = 0;
        for (int i = 0; i < arr1.size(); ++i) {
            int temp1 = fun(arr2, arr1[i]); // 小于等于
            if (temp1 == -1) {
                if(abs(arr2[temp1+1]-arr1[i])>d)ans++;
            } else {
                if (abs(arr2[temp1] - arr1[i]) > d &&
                    abs(arr2[temp1 + 1] - arr1[i]) > d) {
                    ans++;
                }
            }
        }
        return ans;
    }
};
```
# 1170. 比较字符串最小字母出现频次
```cpp
class Solution {
public:
    int fun(string s){
        char minn=s[0];
        int cnt=0;
        for(char &c:s){
            if(c<minn){
                minn=c;
                cnt=1;
            }else if(c==minn)
                cnt++;
        }
        return cnt;
    }
    int find01(vector<int>&nums,int target){
        //小于等于 l r 大于
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<=target)l=mid+1;
            else r=mid-1;
        }
        return nums.size()-l;
    }
    vector<int> numSmallerByFrequency(vector<string>& queries, vector<string>& words) {
        vector<int>w;
        for(string &s:words){
            w.push_back(fun(s) );
        }
        sort(w.begin(),w.end() );
        vector<int>ans(queries.size() );
        for(int i=0;i<queries.size();++i){
            ans[i]=find01(w,fun(queries[i]) );
        }
        return ans; 
    }
};
```
# 2300. 咒语和药水的成功对数
```cpp
class Solution {
public:
#define ll long long 
    int fun(vector<int>&nums,int target,ll success){
        int l=0;
        int r=nums.size()-1;
        ///小于 l r 大于等于
        while(l<=r){
            int mid=l+(r-l)/2;
            if((ll)nums[mid]*target<success){
                l=mid+1;
            }else r=mid-1;
        }
        return nums.size()-l;
    }
    vector<int> successfulPairs(vector<int>& spells, vector<int>& potions, long long success) {
        vector<int>ans(spells.size() );
        sort(potions.begin(),potions.end() );
        for(int i=0;i<spells.size();++i){
            ans[i]=fun(potions,spells[i],success);
        }
        return ans;
    }
};
```
# 2389. 和有限的最长子序列
```cpp
class Solution {
public:
    int fun(vector<int>&nums,int target){
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            //小于等于 l r 大于
            int mid=l+(r-l)/2;
            if(nums[mid]<=target)l=mid+1;
            else r=mid-1;
        }
        return r+1;
    }
    vector<int> answerQueries(vector<int>& nums, vector<int>& queries) {
        sort(nums.begin(),nums.end() );
        vector<int>ans(queries.size() );
        for(int i=1;i<nums.size();++i){
            nums[i]+=nums[i-1];
        }
        for(int i=0;i<queries.size();++i){
            int target=queries[i];
            ans[i]=fun(nums,target);
        }
        return ans;
    }
};
```
# 2080. 区间内查询数字的频率
```cpp
class RangeFreqQuery {
public:
    unordered_map<int,vector<int>>idx;
    RangeFreqQuery(vector<int>& arr) {
        for(int i=0;i<arr.size();++i){
            idx[arr[i]].push_back(i);
        }
    }
    int lower_bound01(vector<int>&nums,int target){
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<target)l=mid+1;
            else r=mid-1;
        }
        //小于 l r 大于等于
        return l;
    }
    int upper_bound01(vector<int>&nums,int target){
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<=target)l=mid+1;
            else r=mid-1;
        }
        //小于等于 l r 大于
        return r;
    }
    int query(int left, int right, int value) {
        if(idx.count(value)==0)return 0;
        int idx1=lower_bound01(idx[value],left);//大于=left
        int idx2=upper_bound01(idx[value],right);//小于=right
        return idx2-idx1+1;
    }
};

/**
 * Your RangeFreqQuery object will be instantiated and called as such:
 * RangeFreqQuery* obj = new RangeFreqQuery(arr);
 * int param_1 = obj->query(left,right,value);
 */
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