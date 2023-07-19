# 前 K 个高频元素

[347. 前 K 个高频元素 - 力扣（LeetCode）](https://leetcode.cn/problems/top-k-frequent-elements/)

> 给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。

> 示例 1:
>
> 输入: nums = [1,1,1,2,2,3], k = 2
> 输出: [1,2]
>
> 示例 2:
>
> 输入: nums = [1], k = 1
> 输出: [1]

## 解法

```c++
class Solution {
private:
    map<int,int> mp;
    vector<int> result;
    priority_queue<int,vector<int>,greater<int> >pq;
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        for(int i=0; i<nums.size(); i++){
            if(mp.find(nums[i])!=mp.end())
                mp[nums[i]]++;
            else
                mp[nums[i]] = 1;
        }
        for(auto i:mp){
            if(pq.size()<k)
                pq.push(i.second);
            else{
                if(i.second > pq.top()){ 
                    pq.pop();
                    pq.push(i.second);
                }
            }
        }
        for(auto i:mp){
            if(i.second >= pq.top())
                result.push_back(i.first);
            if(result.size()==k)
                break;
        }
        return result;
    }
};
```

## 笔记

本题的思路如下：

1. 使用哈希表存储各元素出现次数；
2. 对各元素出现次数进行排序；
3. 将排序最大的k个元素取出；

在第二、三步可以维护一个长度为k的优先队列用于排序和取得最大的k个元素。
