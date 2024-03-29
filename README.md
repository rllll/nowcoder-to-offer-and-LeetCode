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

* 暴力法 O(n^2)
* 从左下找 O(n)

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

时间效率O(n)
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

* 开辟新的数组存放 O(n)
* python或java中的相关函数（如java的replace()） 效率依函数而定

### 03 从尾到头打印链表

#### 题目描述

输入一个链表，按链表从尾到头的顺序返回一个ArrayList。

#### 思路

遍历链表，将元素放入栈中，再从栈顶取出

时间效率O(n)

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

时间效率也是O(n)，但递归消耗更多的空间

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
### 05 用两个栈实现队列

#### 题目描述

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

#### 思路

用第一个栈存放数据，表示入队。当需要出队时，先将第一个栈中数据从栈顶取出放入第二个栈中，从第二个栈中取出栈顶元素为出队元素，再将第二个栈中所有数据从栈顶放入第一个栈（保持入队顺序）。

```c++
class Solution
{
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        while(!stack1.empty())
        {
            stack2.push(stack1.top());
            stack1.pop();
        }
        int top_elem = stack2.top();
        stack2.pop();
        while(!stack2.empty())
        {
            stack1.push(stack2.top());
            stack2.pop();
        }
        return top_elem;
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```

### 06 旋转数组的最小数字

#### 题目描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。
输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。
例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。
NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

#### 思路

输入的是非递减排序的数组的一个旋转，可以看做旋转将最小的值藏在了中间，那么就很好做了，直接从前往后遍历，如果不满足递增顺序，第一个不满足的数就是最小的值。当然，考虑到数组没有翻转，那么第一个数就是最小值。

时间效率O(n)
```c++
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        int length = rotateArray.size();
        if (length == 0)
            return 0;
        else
        {
            int minNum = rotateArray[0];
            for (int i = 1; i < length; i++)
            {
                if (rotateArray[i] < rotateArray[i-1])
                {
                    minNum = rotateArray[i];
                    break;
                }
            }
            return minNum;
        }
    }
};
```

#### 其他思路

* 排序后输出第一个值

    c++里面用的是sort函数，效率为O(nlogn)

* 优先队列O(n)

```c++
class Solution {
public:
    int minNumberInRotateArray(vector<int> rotateArray) {
        if(rotateArray.empty())
            return 0;
        priority_queue<int, vector<int>, greater<int>> result;
        for(int i = 0; i < rotateArray.size(); i++)
        {
            result.push(rotateArray.at(i));
        }
        return result.top();
    }
};
```

### 07 斐波那契数列

#### 题目描述

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。
n<=39

#### 思路

这题递归会超时，所以直接递推。

时间效率O(n)

```c++
class Solution {
public:
    int Fibonacci(int n) {
        int f[n+1];
        f[0] = 0;
        f[1] = 1;
        f[2] = 1;
        for (int i = 3; i <= n; i++)
        {
            f[i] = f[i-1] + f[i-2];
        }
        return f[n];
    }
};
```
#### 其他思路

* <span id = "Fibonacci">优化存储</span>

    其实我们可以发现每次就用到了最近的两个数，所以我们可以只存储最近的两个数：

    sum存储第 n 项的值

    one 存储第 n-1 项的值

    two 存储第 n-2 项的值

    时间复杂度O(n)

    空间复杂度O(1)

    ```c++
    class Solution {
    public:
        int Fibonacci(int n) {
            if(n == 0){
                return 0;
            }else if(n == 1){
                return 1;
            }
            int sum = 0;
            int two = 0;
            int one = 1;
            for(int i=2;i<=n;i++){
                sum = two + one;
                two = one;
                one = sum;
            }
            return sum;
        }
    };
    ```

* 持续优化

    利用 sum 存储第 n-1 项，例如当计算完 f(5) 时 sum 存储的是 f(5) 的值，当需要计算 f(6) 时，f(6) = f(5) + f(4)，sum 存储的 f(5)，f(4) 存储在 one 中，由 f(5)-f(3) 得到。

    时间复杂度O(n)

    空间复杂度O(1)

    ```c++
    class Solution {
    public:
        int Fibonacci(int n) {
            if(n == 0){
                return 0;
            }else if(n == 1){
                return 1;
            }
            int sum = 1;
            int one = 0;
            for(int i=2;i<=n;i++){
                sum = sum + one;
                one = sum - one;
            }
            return sum;
        }
    };
    ```

* 矩阵法

    时间效率O(logn)

    [原理推导](https://blog.csdn.net/u012061345/article/details/52224623#commentBox)
    
    java代码实现
    ```java
    class Mat { // 矩阵对象
        int n = 2;
        int m[][] = new int[n][n];
        public Mat mul(Mat a) { // 矩阵乘法
            Mat b = new Mat();
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++)
                    for (int k = 0; k < n; k++)
                        b.m[i][j] += this.m[i][k] * a.m[k][j];
            }
            return b;
        }
    }
    public class Solution {
        public static int Fibonacci(int n) {
            if(n==0){
                return 0;
            }
            Mat ans = new Mat();
            for (int i = 0; i < ans.n; i++) { //单位矩阵初始化
                ans.m[i][i] = 1;
            }
            Mat base = new Mat();
            base.m[0][0] = base.m[0][1] = base.m[1][0] = 1; // base矩阵初始化
            base.m[1][1] = 0;
            n -= 1;
            while (n > 0) { // 快速幂：求base矩阵的n-1次方
                if ((n & 1) != 0) {
                    ans = ans.mul(base);
                }
                n >>= 1;
                base = base.mul(base);
            }
            return ans.m[0][0];
        }
    }
    ```

* [知乎上的七大解法](https://zhuanlan.zhihu.com/p/53781455)

### 08 跳台阶

#### 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

#### 思路

跳到n级台阶，要么是从n-1级跳1级到达，要么是从n-2级跳2级到达。

假设青蛙跳到n级台阶有F[n]种跳法，容易得到递推式F[n] = F[n-1] + F[n-2]。

n = 1时，F[1] = 1

n = 2时，F[2] = 2

时间效率O(2^n)

```c++
class Solution {
public:
    int jumpFloor(int number) {
        if (number == 1)
            return 1;
        else if (number == 2)
            return 2;
        else
        {
            return jumpFloor(number-2) + jumpFloor(number-1);
        }
    }
};
```

#### 其他思路

* 直接递推

* [优化存储](#Fibonacci)（见上一题斐波那契数列）

### 09 变态跳台阶

#### 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

#### 思路

还是求递推式，不过有了一些变化：（依旧假设跳上n级台阶有F[n]种跳法）

F[1] = 1

F[2] = F[1] + 1

F[3] = F[2] + F[1] + 1 = 2F[2]

F[4] = F[3] + F[2] + F[1] + 1 = 2F[2] + F[2] + F[2] = 4F[2] = 2^2*F[2]

...

F[n] = 2^(n-2)*F[2] = 2^(n-1)

答案显而易见

为了快速得到2^(n-1)，采用快速幂运算求解，时间效率O(logn)

```c++
class Solution {
public:
    int jumpFloorII(int number) {
        number -= 1;
        int ans = 1, base = 2;
        while(number)
        {
            if (number & 1)
                ans *= base;
            base *= base;
            number >>= 1;
        }
        return ans;
    }
};
```

#### 其他思路

到达n级台阶，可以从1...n-1跳一步，假设跳上n级台阶有F[n]种跳法：

F[n] = F[n-1] + F[n-2] + ... + F[1]

F[n-1] = F[n-2] + F[n-3] + ... + F[1]

两式相减，得到F[n] = 2F[n-1]

已知F[1] = 1，可用递推或递归实现，这里给出递推写法，时间效率O(n)

```c++
class Solution {
public:
    int jumpFloorII(int number) {
        int n = 1;
        for (int i = 2; i <= number; i++)
            n *= 2;
        return n;
    }
};
```