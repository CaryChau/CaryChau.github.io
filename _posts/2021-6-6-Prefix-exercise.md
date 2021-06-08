## 525.连续数组
问题：给定一个二进制数组 nums , 找到含有相同数量的 0 和 1 的最长连续子数组，并返回该子数组的长度。

代码参考:
```
class Solution
{
public:
    int findMaxLength(vector<int> &nums)
    {
        int MaxLength = 0;
        unordered_map<int, int> mp;
        int counter = 0;
        mp[counter] = -1;
        int n = nums.size();
        for (int i = 0; i < n; i++)
        {
            int num = nums[i];
            if (num == 1)
            {
                counter++;
            }
            else
            {
                counter--;
            }
            if (mp.count(counter))
            {
                int prevIndex = mp[counter];
                MaxLength = max(MaxLength, i - prevIndex);
            }
            else
            {
                mp[counter] = i;
            }
        }
        return MaxLength;
    }
};
```
本问题需要我们找出**最长**相同数量的0和1的子数组，以**归纳法**的角度思考，  
1. 从数组大小为2开始推导，只有0和1，那肯定满足要求；
2. 如果数组长度大于2，即可以将整个数组视为无数个01组合的小数组，不管顺序，因为只要数量一致结果总会抵消。接下来，不管01组合是在数组中间部分，还是偏左偏右部分，都应该能找出符合要求的最长子数组。

微分？可能只有中间部分会是符合要求的，我们配合前缀知识，将得到的值存入哈希表。  
或者说，循环每一次都会存储自己对应的前缀和，然后看哈希表里面有无相应的值，有的话则比较前缀和是否都为0，是的话比较索引值大小，没有的话则存进前缀和+索引。