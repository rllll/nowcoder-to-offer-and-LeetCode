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
    class Solution {
    public:
        bool Find(int target, vector<vector<int> > array) {
            int rows = array.size();
            if(rows == 0){
                return false;
            }
            int cols = array[0].size();
            if(cols == 0){
                return false;
            }
            // 左下
            int row = rows-1;
            int col = 0;
            while(row >= 0 && col < cols){
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
### 02 替换空格

#### 题目描述

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

#### 思路

记录字符串中空格的个数，由于“%20”代替“ ”，就相当于每替换一个空格需增加2个位置，替换后的字符串长度需加上2*空格个数。这里采用一个巧妙的方法：从字符串最后一位依次往前，如果不是空格，则让其移到比它后2\*空格个数的位置；如果是空格，则当前空格数减去1，并从它的后2\*当前空格数的位置起的三位连续空间依次填充“%20”。

*需要注意的是，在判断为空格时必须先将空格数减1，再填充。想像一下，如果此时空格数没有减1，因为之前的字符中没有空格，一定是紧接着排列的，空格数不变的话，这个空格也会移到与之前字符紧接的位置，那么就没有空间来填充三位字符串了。*

```c++
class Solution {
public:
	void replaceSpace(char *str,int length) {
        int count = 0;
        for (int i = 0; i < length; i++)
        {
            if (str[i] == ' ')
            {
                count++;
            }
        }
        for (int i = length - 1; i >= 0; i--)
        {
            if (str[i] != ' ')
            {
                str[i+count*2] = str[i];
            }
            else
            {
                count--;
                str[i+count*2] = '%';
                str[i+count*2+1] = '2';
                str[i+count*2+2] = '0';
            }
        }
	}
};
```
#### 其他思路

* 开辟新的数组存放
* python或java中的相关函数（如java的replace()）

### 03 从尾到头打印链表

#### 题目描述

输入一个链表，按链表从尾到头的顺序返回一个ArrayList。

#### 思路

遍历链表，将元素放入栈中，再从栈顶取出

```c++
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        stack<int> s;
        for (ListNode *p = head; p != NULL; p = p->next)
        {
            s.push(p->val);
        }
        vector<int> ArrayList;
        while(!s.empty())
        {
            ArrayList.push_back(s.top());
            s.pop();
        }
        return ArrayList;
    }
};
```
#### 其他思路

尾递归

```c++
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> ArrayList;
    vector<int> printListFromTailToHead(ListNode* head) {
        if (head != NULL)
        {
            printListFromTailToHead(head->next);
            ArrayList.push_back(head->val);
        }
        return ArrayList;
    }
};
```

### 04 重建二叉树

#### 题目描述

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

#### 思路

递归。首先可以确定前序序列第一个数是整课树的根，在中序序列中，遍历找到根，在根左边的序列一定是其左子树，在根右边的序列一定是其右子树。在前序序列中，左子树的序列紧接着根，唯一需要确定是是左子树节点的个数，可以借助于中序序列左子树中序序列实现，右子树前序序列则为剩下部分。

经过这样的划分，我们可以得到左子树前序序列，左子树中序序列，右子树前序序列，右子树中序序列。要确定根的左子树，必须依据左子树的前序序列和中序序列得到左子树；要确定根的右子树，必须依据右子树的前序序列和中序序列得到右子树。所以，可以采用递归的方法，分别求解左子树和右子树。

```c++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        int num = vin.size();
        if (!num)
            return NULL;
        vector<int> pre_left, vin_left, pre_right, vin_right;
        TreeNode *head = new TreeNode(pre[0]);
        int rootindex;
        for (int i = 0; i < num; i++)
        {
            if (vin[i] == pre[0])
            {
                rootindex = i;
                break;
            }
        }
        for (int i = 0; i < rootindex; i++)
        {
            pre_left.push_back(pre[i+1]);
            vin_left.push_back(vin[i]);
        }
        for (int i = rootindex + 1; i < num; i++)
        {
            pre_right.push_back(pre[i]);
            vin_right.push_back(vin[i]);
        }
        head->left = reConstructBinaryTree(pre_left,vin_left);
        head->right = reConstructBinaryTree(pre_right,vin_right);
        return head;
    }
};
```
