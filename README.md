# nowcoder-to-offer-and-LeetCode
牛客网剑指offer和LeetCode刷题记录

## 牛客网-剑指Offer

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
#### 其他思路

* 暴力法
* 从左下找

    给定二维数组具有两个性质：
    
    1. 每一行都按照从左到右递增的顺序排序
    2. 每一列都按照从上到下递增的顺序排序

    可以得出结论：对于左下角的值 m，m 是该行最小的数，是该列最大的数

    每次将 m 和目标值 target 比较：

    1. 当 m < target，由于 m 已经是该列最大的元素，想要更大只有从列考虑，取值右移一位
    2. 当 m > target，由于 m 已经是该行最小的元素，想要更小只有从行考虑，取值上移一位
    3. 当 m = target，找到该值，返回 true

    用某行最小或某列最大与 target 比较，每次可剔除一整行或一整列

    ```c++
    public class Solution {
        public boolean Find(int target, int [][] array) {
            int rows = array.length;
            if(rows == 0){
                return false;
            }
            int cols = array[0].length;
            if(cols == 0){
                return false;
            }
            // 左下
            int row = rows-1;
            int col = 0;
            while(row>=0 && col<cols){
                if(array[row][col] < target){
                    col++;
                }else if(array[row][col] > target){
                    row--;
                }else{
                    return true;
                }
            }
            return false;
        }
    }
    ```

