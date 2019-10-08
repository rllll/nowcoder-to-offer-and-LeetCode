# nowcoder-to-offer-and-LeetCode
牛客网剑指offer和LeetCode刷题记录

### 01 二维数组中的查找

#### 题目描述

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

#### 思路

逐行扫描，对每一行用折半查找，时间效率O(nlogn)

```c++
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        int n = array.size();
        for (int i = 0; i < n; i++)
        {
            int l = 0, r = array[0].size()-1;
            while(l <= r)
            {
                int mid = (l+r) / 2;
                if (array[i][mid] < target)
                {
                    l = mid + 1;
                }else if(array[i][mid] > target)
                {
                    r = mid - 1;
                }else{
                    return true;
                }
            }
           
        }
        return false;
    }
};
```