## 数组

### 二分查找

[704. 二分查找](https://leetcode.cn/problems/binary-search/)

第一种写法：

左闭右闭 [left,right]

1. while(start <= end)  start == end 是有意义的
2. 不要使用 ++ -- 去操作边界，而是使用 +1 -1这种操作

第二种写法:

左闭右开

1. while(start < end)



[35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

1. 注意 while 是 while(left < right) 还是 while(left <= right)
2. While 的条件会影响最终结果是 res  还是res+1  还是res-1



[34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

思路：通过二分法，找两遍，分别找到左右边界；找左边界和右边界时控制 == 时指针的走向

当找完左边界时就可以进行校验，因为 左边界可能出现 没找到值 或者 left比数组长度大 (因为 midIndex + 1 的操作)；看需不需要继续找右边界
不需要考虑找左边界的时候 会不会找到 元素组中点往右的元素，因为结果的范围不一定是中点往外扩散的



[69. x 的平方根 ](https://leetcode.cn/problems/sqrtx/)

1. 第一点要知道是使用二分法的解题思路

2. 第二点，和普通的二分不同，这个需要一个 result 的全局变量去存储推进前的 left 的值，因为保留整数，所以往前推进可能大于结果，所以在推进前先保留

   

[367. 有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)

想到是使用二分查找来做



### 双指针

[283. 移动零](https://leetcode.cn/problems/move-zeroes/)

不能想着 找到 0元素，然后把0后面的元素都往前进一位，把0排到最后；这样会有一个问题，就是如果 0 后面的元素也是0，变换之后，从0后面的元素开始，那么这个 0 就没有放到队尾

因为维护一个慢指针，遍历数组，找到 不为0 就把值赋值 到慢指针，然后 到最后慢指针后面的值 全部 赋值为0



[11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

首尾双端指针，谁小谁移动；左指针高度更小，左指针往右移动；右指针高度更小，右指针往左移动



[15. 三数之和](https://leetcode.cn/problems/3sum/)
解题思想：固定一个元素，通过 双指针 找到另外两个数
易错点：需要多次去重，1. 固定元素去重 2.双指针找到结果是去重
固定元素遍历条件是：[0,n-2] 因为要找的是三元组，第一个元素最后能取的值就在 倒数第三个




[42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

对于在任意i点上能接到的雨水，取决于 min(左边最高, 右边最高) - height[i]

双指针 left right，向中间逼近；维护 leftMax rightMax 也就是左右两边最高的点；因为维持了左右两边的最大值，所以也就确定当前 left right 的左右边界，而当前高度与这边高度的最大值的差值就是当前可以接到的雨水；

为什么不需要处理 当前点被淹没的情况，因为已经考虑了维护了最大值，最大值就是边界，水不能比边界还高；

height[left] < height[right] 就是判断谁更矮，因为水只能到更矮的点

```java
class Solution {
    public int trap(int[] height) {
        int left = 0, right = height.length - 1;
        int leftMax = 0, rightMax = 0;
        int ans = 0;
        while (left < right) {
            if (height[left] < height[right]) {
                leftMax = Math.max(leftMax, height[left]);
                ans += leftMax - height[left];
                left++;
            } else {
                rightMax = Math.max(rightMax, height[right]);
                ans += rightMax - height[right];
                right--;
            }
        }
    }
}
```



### 移除元素

快慢指针，滑动窗口 最重要的一条原则：**右指针扩张窗口，左指针在满足条件时收缩窗口**

一定不要同时考虑 快指针 慢指针 在每一个条件怎么走，这样会搞得很乱

分开考虑，快指针在什么之后扩张窗口，慢指针在什么时候缩小窗口

Slow - fast 的区间是可以被覆盖的区间

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        for (int right = 0; right < nums.length; right++) {
            // 扩张窗口
            // if 或者 while 缩减窗口
        }
        // 返回结果
    }
}
```

滑动窗口

> 在数组 / 字符串上维护一个连续区间 `[left, right]`，并且这个区间必须满足某个条件

快慢指针

> 两个指针以不同策略移动，用于原地操作或区间扫描 是否有「窗口含义」并不重要



[27. 移除元素](https://leetcode.cn/problems/remove-element/)





[26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

思路：快慢指针



[283. 移动零](https://leetcode.cn/problems/move-zeroes/)

最后填充0 需要单独做



[977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

大的值一定来自两端，左指针 右指针 平方后比较，存入结果，操作指针



[59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

解题思路：

`top`：上边界 `bottom`：下边界 `left`：左边界 `right`：右边界

每一圈：
 **右 → 下 → 左 → 上**

右：left <= right

下：top <= botton

左：需要先判断 top <= bottom 防止在“只剩一行”的情况下，重复遍历同一行

上：需要先判断  left <= right 防止在“只剩一行”的情况下，重复遍历同一行

控制边界，修改边界



[48. 旋转图像](https://leetcode.cn/problems/rotate-image/)

先把整个二维数组转置，沿着对角线交换值，然后翻转每一行

对角线 就是 ( i , i+1)



[240. 搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)

[74. 搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/)

从左上角开始找，找不到，因为不能判断怎么走

从右上角开始找



[34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

不应该单独去找 left right 元素；

问题在于：

- `target` **可能完全不在 mid 左边**
- 左右边界查找 **必须基于整个数组**
- 你这个拆法 **破坏了单调性假设**

二分查找的前提是：**在完整有序区间里找边界**

找到左右两点的方式是同一个数组，通过控制在 == target 的指针走向找到左右两点

找到left 节点时 需要判断是否 == target 再构建结果



[33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

根据 nums[mid] 和  nums[left]  nums[right] 的数值的大小比较，判断那边是有序的

根据 mid 把数组分割，肯定有一边是有序的

```java
class Solution {
    public int search(int[] nums, int target) {
        while(left <= right){
            int mid;
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] < nums[right]){
              // 右半边有序
              // 通过右半边值范围判断指针往哪走
                   if (nums[mid] < target && target <= nums[right]){
                     // 往右半区找
                     left = mid + 1;
                   } else{
                     // 往左半区找
                     right = mid - 1;
                   } 
            }else{
              // 1. 右半边无序 -> 左半边有序
              // 通过右半边值范围判断指针往哪走
                   if(nums[left] <= target && target < nums[mid]) {
                     // 往左半区找
                     right = mid-1;
                   }else{
                     // 往右半区找
                     left = mid+1;
                   } 
            }
        }
        return -1;
    }
}
```



[153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

核心思想：找到无序的那一段

注意 这里的 while (left < right) 和 right = mid

为什么要 mid，因为 mid也属于无序段的一部分，而 left < right 是因为使用 right = mid，这里的判断条件要改变为 <

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            // 最小值一定在无序的一侧
            if (nums[mid] > nums[right]) {
                // mid 在左侧递增区间，最小值在右边
                left = mid + 1;
            } else {
                // mid 在右侧递增区间，mid 可能就是最小值
                right = mid;
            }
        }
        return nums[left];
    }
}
```





### 刷题

[189. 轮转数组](https://leetcode.cn/problems/rotate-array/)

全局翻转，再局部翻转； 注意 k 需要对数组长度取模；避免 k 大于数组长度的情况



[238. 除了自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

维护一个数组，保存每个 i 的左乘积，因为是顺序累乘的，所以可以获得每个节点左乘积

再从后往前遍历，遍历获得右累乘积，再与左乘积 相乘，得到答案



[73. 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/)

使用 第一行和第一列 表明 这一行这一列是否需要置0

第一行，第一列需要额外的标志 判断需不需要全置0


31. 下一个排列
解题思路：
1. 找到从右往左第一个不是递增的前一个元素 例如：[1,2,3,8,7,6,4] 就找到 3
2. 找到从右往左 第一个 nums[i] 大的元素  就找到 4
3. 交换 结果为 [1,2,4,8,7,6,3]
4. 反转右边找到的序列 ：[1,2,4,3,7,7,8] 为什么要反转，因为是升序序列，如果不反转，就不是下一个序列



[88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)

从nums1 的后面开始操作

如果 nums2 耗尽了，nums1 可以原样设置，所以不需要管 nums1 的元素是否耗尽，只需要管 nums2 的元素是否耗尽，体现在代码中就是

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;
        while (j >= 0) {
            if (i >= 0 && nums1[i] > nums2[j]) {
                nums1[k--] = nums1[i--];
            } else {
                nums1[k--] = nums2[j--];
            }
        }
    }
}
```





### 数组总结

数组的几种解题思想：

二分查找：控制左右边界，注意判断 left+1 还是 right-1 要明确 left right 指针的含义

双指针：通过一个快指针和慢指针在一个for循环下完成两个for循环的工作

滑动窗口：right 指针负责扩充，left指针负责在满足条件的时候 缩小窗口

模拟行为：确定好边界，在边界里面做操作；确定好循环不变量







## 滑动窗口

滑动窗口的铁律：**右指针一直增大，左指针判断要不要往右缩小窗口**



[3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

使用 char 数组 判断是否已经存在



[438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

使用 char 数组 判断是否是一致的



## 前缀和

[560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)

核心思想：前缀和，preSum += num[i] ; 

```java
map.containsKey(preSum - k)
是判断存不存在前缀和 等于 preSum 和 k 的差值；
因为前缀和的计算肯定是连续的，如果存在，就可以连续 这个前缀和 存在的数量，就是在当前节点往前推有多少个子数组 等于 target
map 则是保存前缀和 <value,count> 信息，因为不同前缀和的值可能是一样的  
```

初始化：map.put(0, 1);  ，前缀和为 0 出现 1 次（很关键）



[437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)

使用 dfs，并且使用一个 map 保存当前 dfs 路径的前缀和，通过 prefixMap.get(cur - target) 获取 结果，注意，记得在后序顺序进行回溯，删除当前节点的 前缀和

```java
class Solution {
    int res = 0;

    public int pathSum(TreeNode root, int targetSum) {
        Map<Long, Integer> prefix = new HashMap<>();
        prefix.put(0L, 1);
        dfs(root, 0L, targetSum, prefix);
        return res;
    }

    private void dfs(TreeNode node, long cur, int target, Map<Long, Integer> prefix) {
        if (node == null) return;
        cur += node.val;
        res += prefix.getOrDefault(cur - target, 0);
        prefix.put(cur, prefix.getOrDefault(cur, 0) + 1);
        dfs(node.left, cur, target, prefix);
        dfs(node.right, cur, target, prefix);
        prefix.put(cur, prefix.get(cur) - 1); // 回溯
    }
}
```







## 链表

[203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

解题 思路/技巧：虚拟头节点



[206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

解题 思路/技巧：虚拟头节点



[24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

还是可以 虚拟头节点操作，

~~只不过在遍历的过程中 头节点会发生变化；所以会创建两个，一个 vHead 一个 cur，最开始他们是一样的，都是虚拟头节点，睡着遍历，cur会往右移动，而 vHead 保持不变~~

更优雅的解决方式：不需要去定义多个 临时Node 去表达多个指针，只需要抓住啊 preV 节点，然后交换后面的两个节点就行

```java
while (prev.next != null && prev.next.next != null)
```





[19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

虚拟头节点 快慢指针



[160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

先计算两个链表的长度，长的先走完差值，从同一起点出发



[141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

快慢指针 可以同起点，只是 需要先走一步，再进行比较



[142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

> 快慢指针第一次相遇的节点 ≠ 环的入口

>  快慢指针 起点要相同

> 快慢指针能确定是否存在环 需要找到环的入口则需要再次 head 和 slow 同时走，第一次相遇的节点一定是环的入口
>
> 在快慢指针中保存 slow 节点



[234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

快慢指针找到中点，然后翻转后半段链表，然后比较

比较的时候遍历第二段，因为第二段可能比第一段少一个节点(链表长度为奇数的情况下)



[2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

最后一个节点完了之后，如果还需要进位，需要补一个节点



[148. 排序链表](https://leetcode.cn/problems/sort-list/)

使用 归并排序



## 哈希表

> hash碰撞，多个值映射到 hash 表的同一个位置
>
> 解决 hash 碰撞的两种方式：
>
> 拉链法：在 hash 碰撞的位置使用 链表存储
>
> 线性探测：保证tableSize大于dataSize,碰撞了就找下一个 为空的位置
>
> Hash 表最大的用处：空间换时间



**哈希表都是用来快速判断一个元素是否出现集合里**



**虽然map是万能的，但是有的时候可以 使用 数组 或者 Set**



[242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

使用 26 个字母的槽位 int[26]  index: 字符(ascii)



[49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

字符串转为 char[] 排序之后 存入map；containsKey



[128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

不能先排序，再使用滑动窗口计算，因为排序的时间复杂度时 O(nlogn)

先把所有元素放到set中，然后从每个元素开始寻找最长连续序列



## 字符串

[344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

双指针



[151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

先全局翻转，再局部粉转，再去除多余空格



## 栈/单调栈

[20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

如果是 '(' '{' '[' 直接入栈

否则直接校验：

1. 如果 ')' '}' ']',且栈为空，直接为 false
2. 如果入栈 元素 不能匹配栈顶元素，直接为 false



[739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)

不能根据每个点往后去判断比该天温度高的结果，因为测试级 可能出现 99 .... 99 的case，导致超出时间限制

使用栈，依次入栈，比较，出栈，当前节点入栈

栈中的元素 是 index，不是 nums[index]



[1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

栈解法



[84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)

单调递增栈

**第一点（触发清算）：**

> 在数组末尾加入一个高度为 0 的假柱子，
>  用来强制触发所有未结算柱子的出栈计算。

**第二点**：

当遇到一个 **严格小于栈顶高度** 的柱子时，
 说明栈顶柱子的 **右边界被确定**，此时将栈顶弹出并结算：

- 高度 = 被弹出柱子的高度
- 左边界 = 弹出后新的栈顶
- 右边界 = 当前下标

每次只结算 **一根被弹出的柱子**，
 持续弹栈，直到栈重新满足单调递增



单调递增栈，当遇到比栈顶元素小时，开始清算，而且未压入当前元素的栈，清算逻辑为：栈顶元素和栈顶 - 1 元素的清算



## 堆

**堆是一棵完全二叉树，树中每个结点的值都不小于（或不大于）其左右孩子的值**

如果父亲结点是大于等于左右孩子就是大顶堆，小于等于左右孩子就是小顶堆



堆在 java 中的实现是 优先级队列

```java
PriorityQueue<int[]> pq = new PriorityQueue<>((pair1, pair2) -> pair2[1] - pair1[1]);

add() 添加元素
poll() 堆头弹出元素
  
poll 弹出的数据是有序的  

pair2[1] - pair1[1] 大顶堆
pair1[1] - pair2[1] 小顶堆
```



## 二叉树

#### 解题入口

首先判断两点

1. **遍历**：是否可以遍历树的所有节点获得答案，通常需要配置外部变量
2. **分解**：是否可以定义一个递归函数，通过子问题（子树）的答案推导出原问题的答案，并充分利用这个函数的返回值
3. **前中后序**：如果单独抽出一个二叉树节点，它需要做什么事情？需要在什么时候（前/中/后序位置）做



#### 前中后序的含义以及区别

<img src="https://labuladong.online/images/algo/binary-tree-summary/2.jpeg" alt="img" style="zoom: 25%;" />

前序位置的代码在刚刚进入一个二叉树节点的时候执行；tips: **在构建二叉树时通常使用前序**

后序位置的代码在将要离开一个二叉树节点的时候执行； tips: **可以获得前两次递归的返回值**

中序位置的代码在一个二叉树节点左子树都遍历完，即将开始遍历右子树的时候执行  tips: **在搜索树中中序遍历是语序的，同时，右中左也是有序的**



#### 题目解析

[236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

后序遍历

```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) {
            return root;
        }
        return left != null ? left : right;
    }
```



[101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

因为是判断轴对称，所以判断的值是否相同，不能直接使用 == 判断



[104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

是否可以简化为子问题：是，每个节点的最大深度为左右子节点的最大深度 + 1

使用什么遍历：需要获得子节点的结果，使用后序遍历



[98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

解法一：使用二叉树搜索树 中序遍历 是严格升序的性质， 保存遍历中的 preVal,

解法二：使用上下边界



[230. 二叉搜索树中第 K 小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)

k 要单独提到 成员变量，不能在递归中遍历，因为在递归中 k减少不会 传递到 递归栈帧弹出之后的值

因为要是 中序遍历 ，k 不能出现在方法参数中 递减



[543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

使用什么遍历还是分解：可以分解为子问题吗？子节点的直径 和 父节点的直径没有关系；因为不能累加；拿使用遍历，获得每个节点的直径，并使用一个全局变量，比较获得最大值

使用什么遍历：后序遍历，因为当前节点的直径可以拆分为左右两个子节点的最大深度 + 1，因为要获取左右两个节点的最大深度，得到最大值，在遍历中获得的值和全局的值相比较，得到最大值；

也就是其实拆分为了 获得每个节点左右子节点的最大深度，然后在遍历的过程顺带求取了直径



[116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)

不过不使用层序遍历，使用递归的方案

根据题意：如果直接使用遍历的方式，可以把每个节点的左节点指向右节点，但是当不是同一层，不是同一父节点却不能相连；

由于是完美二叉树的关系，可以把 同一层的相邻两个节点当成一个父节点，此时，当前父节点的左子节点为左节点的右子节点，反之依然



[114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)

是否可以简化为子问题：可以，每个节点的左子节点转化为右子节点，原本的右子节点挂到 原先左子节点的最右边的叶子节点上

迭代写法：找到 左子树的 最右叶子节点，把左子树挂上去

递归写法：右 → 左 → 根

```java
class Solution {
    TreeNode prev = null;
    public void flatten(TreeNode root) {
        if (root == null) return;
        flatten(root.right);
        flatten(root.left);
        root.right = prev;
        root.left = null;
        prev = root;
    }
}
```





#### 构造二叉树

[105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```java
preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
```

这里有一个很巧妙的两点：

1. 从前序中获取根节点的值在中序的index，在可以切割开中序序列，从而分化为子问题求解
2. 中序的index 同样可以切分 前序数组

**因为在「前序 + 中序」的约束下，`index` 恰好等于左子树节点的数量**

```java
preorder = [ 根 | 左子树 | 右子树 ]
inorder  = [ 左子树 | 根 | 右子树 ]
```

不使用 Arrays.copyOfRange() 的方式，使用下标的方式：

注意一点，起点 和 终点 不再是 0 和 length() , 而是方法参数

```java
int leftSize = inIndex - inStart;
// 得出左边节点的大小
```





[106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

```java
输入：inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
```

取 3 为 root 节点，在 inorder 中获得 index = 2; 一样可以切分 postorder,

```java
前序： 根 → 左子树（全部） → 右子树（全部）
中序： 左子树（全部） → 根 → 右子树（全部）
后序： 左子树（全部） → 右子树（全部）→ 根
```



## 回溯算法

回溯是递归的副产品，只要有递归就会有回溯；回溯法解决的都是在集合中递归查找子集；

**回溯的关键在于「状态变化 + 状态恢复」**

| 类型 | 本质                     |
| ---- | ------------------------ |
| 组合 | 从候选集中选若干个       |
| 子集 | 对每个元素：选 / 不选    |
| 排列 | 在剩余元素中选一个       |
| 切割 | 在字符串上选切割点       |
| 棋盘 | 在棋盘状态空间中选下一步 |

快速判断：

- ❓ **用 startIndex 吗？**
  - 子集 / 组合 / 切割 → 是
  - 排列 → 否
- ❓ **同层去重还是路径去重？**
  - 同层 → HashSet 在递归内
  - 路径 → used[] / visited
- ❓ **能不能事后校验？**
  - 能 → 普通回溯
  - 不能 → 必须提前剪枝（如括号生成）

回溯模版：

```java
void backtracking(参数) {
    if (终止条件) {
        存放结果; // 一定是「当前状态已经构成一个合法解」
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

递归是往下找节点; for 是遍历这一层的所有可能性；

剪枝 = 在“当前状态”下，提前判断某个分支不可能产生合法解，从而停止继续搜索

也就是说剪枝 可以在终点判断 和 for 循环中 加条件



[51. N 皇后](https://leetcode.cn/problems/n-queens/)

Row 在参数中制定，col 在回溯中指定

先判断是否合规，再进行 回溯操作



[78. 子集](https://leetcode.cn/problems/subsets/)

回溯加入 startIndex，控制回溯范围



[17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

在回溯方法中加入 index ，用以获得当前拿取那个数字对应的字符列表

使用 StringBuilder 存储 回溯结果



[39. 组合总和](https://leetcode.cn/problems/combination-sum/)

需要 startIndex 减枝，不然会有重复结果
在回溯中：
如果需要重复元素就是：
```java
  for (int i = start; i < candidates.length; i++) {
      path.add(candidates[i]);
      backtrack(candidates, path, i, target - candidates[i]);
      path.remove(path.size() - 1);
  }
```
如果不需要重复元素就是：
```java
  for (int i = start; i < candidates.length; i++) {
      path.add(candidates[i]);
      backtrack(candidates, path, i + 1, target - candidates[i]);
      path.remove(path.size() - 1);
  }
```


[22. 括号生成](https://leetcode.cn/problems/generate-parentheses/)

错误想法：把所有结果罗列出来。再进行 valid;复杂度爆炸

解题思想：在任意前缀中：右括号数量 ≤ 左括号数量，总左括号数量 ≤ n，只要满足这两个约束，最终长度到 2n，就是一个合法结果。

```java
    private void backtrack(StringBuilder sb, int open, int close, int n) {
        // 终止条件
        if (sb.length() == 2 * n) {
            res.add(sb.toString());
            return;
        }

        // 还能放左括号
        if (open < n) {
            sb.append('(');
            backtrack(sb, open + 1, close, n);
            sb.deleteCharAt(sb.length() - 1);
        }

        // 右括号不超过左括号
        if (close < open) {
            sb.append(')');
            backtrack(sb, open, close + 1, n);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
```



[79. 单词搜索](https://leetcode.cn/problems/word-search/)

不是一下子找到整个path,而是把每一个单元当成 起点，去找，同时维护一个 k-index,代表这一轮要找的什么 字母；

由于不能重复使用，所以要把走过的 单元 设置为 # ，回退时恢复现状



[131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

这是“切割位置”的回溯，不是“字符选择”的回溯

核心思想：

```java
startIndex 固定当前切割起点，
从它开始向后尝试所有可能的切割点，
切一段合法的，就把 startIndex 移到新的位置，
一直切到字符串末尾为止。
```



[46. 全排列](https://leetcode.cn/problems/permutations/)

通过 set 去重；

不能不要path，因为不同顺序是不同结果，需要 path 和 set 同时使用



[77. 组合](https://leetcode.cn/problems/combinations/)

回溯；

由于要回溯，也就是删除节点，使用 LinkedList 比较方便



[90. 子集 II](https://leetcode.cn/problems/subsets-ii/)

先排序



[491. 非递减子序列](https://leetcode.cn/problems/non-decreasing-subsequences/)

回溯里的 `HashSet`，如果定义在 for 循环外、递归内，
 那它一定是在做「同一层去重」



## 贪心算法

局部最优 获得 全局最优

[121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

其实还是使用的动态规划，通过前一步的获利情况判断是保持之前买还是当前步骤买，每一步都计算最终总获利

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length == 0) return 0;
        int[] res = new int[prices.length];
        res[0] = -prices[0];
        int max = 0;
        for (int i = 1; i < prices.length; i++) {
            // 持有股票的最大收益
            res[i] = Math.max(res[i - 1], -prices[i]);
            // 在第 i 天卖出
            max = Math.max(max, res[i - 1] + prices[i]);
        }
        return max;
    }
}

贪心写法：
  class Solution {
    public int maxProfit(int[] prices) {
        int res = 0;
        int min = prices[0];
        for(int i = 0;i < prices.length;i++){
            min = Math.min(min,prices[i]);
            res = Math.max(res,prices[i] - min);
        }
        return res;
    }
}
```



[55. 跳跃游戏](https://leetcode.cn/problems/jump-game/)

维护一个最远可达的变量，如果 i > 最远可达，那就说明不能到最后

```java
class Solution {
    public boolean canJump(int[] nums) {
        int maxReach = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > maxReach) {
                return false;
            }
            maxReach = Math.max(maxReach, i + nums[i]);
        }
        return true;
    }
}
```





## 动态规划

每一步操作 和 上一步操作有关联；而贪心是没有关联的，每一步都取最优；

所以**动态规划中每一个状态一定是由上一个状态推导出来的** 就是找出关系

动态规划五部曲：

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组



[118. 杨辉三角](https://leetcode.cn/problems/pascals-triangle/)

把  List<List<Integer>> res = new ArrayList<>(); 当成动规数组，

状态转移方程为 row.add(res.get(i - 1).get(j - 1) + res.get(i - 1).get(j));

注意每一行的一头一尾的值都为1



[70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

从前往后递推；不能使用递归的方式，因为会重复计算



[746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/)

Dp[i] 的含义是：在该台阶往上跳完 1步或者 2步之后需要花费的钱 (已经加入了当前跳的cost),所以要返回 dp[length-1] dp[length-2]



[62. 不同路径](https://leetcode.cn/problems/unique-paths/)

起点到起点，本身就是 **一条路径** ; 所以 int[0] [0] 的值为 1



[63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/)

在初始化 dp 的时候，出现一个障碍物，后面都为 0

外层遍历行； 内层遍历列



[343. 整数拆分](https://leetcode.cn/problems/integer-break/)

递推公式：

```java
        for(int i = 3; i <= n; i++) {
            for(int j = 1; j <= i-j; j++) {
                // 这里的 j 其实最大值为 i-j,再大只不过是重复而已，
                //并且，在本题中，我们分析 dp[0], dp[1]都是无意义的，
                //j 最大到 i-j,就不会用到 dp[0]与dp[1]
                dp[i] = Math.max(dp[i], Math.max(j*(i-j), j*dp[i-j]));
                // j * (i - j) 是单纯的把整数 i 拆分为两个数 也就是 i,i-j ，再相乘
                //而j * dp[i - j]是将 i 拆分成两个以及两个以上的个数,再相乘。
            }
        }
```



[96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

使用 动态规划

```java
        for (int i = 2; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                //对于第i个节点，需要考虑1作为根节点直到i作为根节点的情况，所以需要累加
                //一共i个节点，对于根节点j时,左子树的节点个数为j-1，右子树的节点个数为i-j
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
```



> 以上这两个题目都是，依赖于之前的所有数据，而不只是依赖于之前的一两个数据



### 子元素相关

这种找子元素，子元素之和，子串的题目，一定要确认 dp[i] 或者 dp\[i][j] 的含义是什么

在确认好含义是什么，就想怎么初始化，其次 动态转移方程，最后确定遍历顺序



[53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

使用动态规划



[300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

> dp[i] 的含义：在 num[i] 的元素之前，最长的递增数组；nums[i] 可以接在任意 j < i 且 nums[j] < nums[i] 的位置后面
>
> 初始化：
>
> 状态转移方程：也就是 当 i 元素出现，需要遍历之前的所有状态，因为 当前 nums[i] 可以接在任何 nums[j] < nums[i] 的位置后面
>
> 遍历顺序： for (int i = 0; i < n; i++) {
>                            for (int j = 0; j < i; j++)

错误写法：把当前的最长状态量往后移动是错误的，可能会漏掉一些组合

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums.length == 1) return 1;
        int[] dp = new int[nums.length];
        dp[0] = 1;
        dp[1] = nums[1] > nums[0] ? 2 : 1;
        int maxLIS = Math.max(dp[0],dp[1]);
        for(int i = 2;i < nums.length;i++){
            int reOne = nums[i] > nums[i-1] ? dp[i-1] + 1  : 1;
            int reTwo = nums[i] > nums[i-2] ? dp[i-2] + 1  : 1;
            dp[i] = Math.max(reOne,reTwo);
            maxLIS = Math.max(maxLIS,dp[i]);
        }
        return maxLIS;
    }
}
```

> **dp[i] = 以 nums[i] 结尾的最长递增子序列长度**
>
> nums[i] 可以接在任意 j < i 且 nums[j] < nums[i] 的位置后面

正确解法：

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);
        int res = 1;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            res = Math.max(res, dp[i]);
        }
        return res;
    }
}

// 解释
for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
是否存在这种反例 [5, 4, 3, 6] nums[i] > nums[j] 这种比法是不是 把 8也算入在哪了
5、4、3 都比 6 小，
那是不是“全部都算进来了”？
LIS 不累加分支
它只选 一条最长路径  
```



[152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/)

错误代码：

这 **只考虑了一种情况**：「当前最大乘积一定来自前一个最大乘积 × 当前数」

但 **最大乘积子数组** 有一个致命特性：**负数 × 负数 = 正数**

```java
class Solution {
    public int maxProduct(int[] nums) {
        if (nums.length == 1)
            return nums[0];   
        int[] dp = new int[nums.length];
        Arrays.fill(dp,1);
        dp[0] = nums[0];
        int max = dp[0];
        for (int i = 1; i < nums.length; i++) {
            if(nums[i] == 0){
                continue;
            }
              dp[i] = dp[i-1] * nums[i];
              max = Math.max(dp[i],max);
        }
        return max;
    }
}
```

正确代码：维护之前的 最大值 和 最小值 和 res

```java
class Solution {
    public int maxProduct(int[] nums) {
        int maxDp = nums[0];
        int minDp = nums[0];
        int res = nums[0];
        for (int i = 1; i < nums.length; i++) {
            int cur = nums[i];
            if (cur < 0) {
                int tmp = maxDp;
                maxDp = minDp;
                minDp = tmp;
            }
          // maxDp * cur 当前元素 接在前一个区间后面
          // cur 直接从当前元素重新开始一个新区间
            maxDp = Math.max(cur, maxDp * cur);
            minDp = Math.min(cur, minDp * cur);
            res = Math.max(res, maxDp);
        }
        return res;
    }
}
```



[5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

> 这两个元素挨着  且相等  那就是回文字串
>
> 这两个元素相等  且他们之间的元素是一个回文串  dp\[i][j] 中间字串的情况是  dp\[i+1][j-1]
>
> dp\[i][j] 含义：在字符串 s 中的 [i][j]区间 是否是 回文字符串
>
> 1. 初始化：dp\[i]\[i] = true; 只有一个字符的子串一定是回文
2. 为什么要 int j = i + 1；在这控制 子串的 范围是 \[i,j],所以j 要比 i大，为什么是 > 而不是 >=,因为 = 的情况在初始化已经被处理了
3. 为什么要  j - i <= 2；因为 s.charAt(i) == s.charAt(j)；所以 如果 j - i = 1 or 2 都可以直接判定结果
4. 这里不能 只使用 dp[i + 1][j - 1] 去替代 j - i <= 2
   dp[i + 1][j - 1] 针对 长串判断，也就是 dp[i + 1][j - 1] 有实际意义的情况
   j - i <= 2 针对短串，针对 dp[i + 1][j - 1] 没有实际意义的情况
   例如：i = 1 j = 2 ,此时如果使用 dp[2][1] 去判断，就没有意义，字符串中没有 [2,1]的子串
5. 保留 start_index，因为结果返回的是子串，而不是长度
> 

```java
class Solution {
    public String longestPalindrome(String s) {
        boolean[][] dp = new boolean[s.length()][s.length()];
        String resStr = "";
        for (int i = s.length() - 1; i >= 0; i--) {
            for (int j = i; j <= s.length() - 1; j++) {
                if (s.charAt(i) == s.charAt(j) && (j - i <= 1 || dp[i + 1][j - 1]))
                    dp[i][j] = true;
                if (dp[i][j] == true && (j - i + 1) > resStr.length()) {
                    resStr = s.substring(i, j + 1);
                }
            }
        }
        return resStr;
    }
}
```



[1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)

> dp\[i][j] 的含义：text1 的前 i 个字符 和 text2 的前 j 个字符 的 最长公共子序列长度

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        // text1 的前 i 个字符 和 text2 的前 j 个字符 的 最长公共子序列长度
        int[][] dp = new int[text1.length() + 1][text2.length() + 1];
        for (int i = 1; i <= text1.length(); i++) {
            for (int j = 1; j <= text2.length(); j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[text1.length()][text2.length()];
    }
}
```



[72. 编辑距离](https://leetcode.cn/problems/edit-distance/)

> dp\[i][j] 的含义 把 word1 的前 i 个字符 变成 word2 的前 j 个字符 所需要的最少操作数

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        // 把 word1 的前 i 个字符 变成 word2 的前 j 个字符 所需要的最少操作数
        int[][] dp = new int[m + 1][n + 1];

        // 初始化，全部删除操作
        for (int i = 0; i <= m; i++)
            dp[i][0] = i;
        for (int j = 0; j <= n; j++)
            dp[0][j] = j;

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
                }
            }
        }
        return dp[m][n];
    }
}
```







### 背包问题

#### 01背包问题

>01 背包：**物品在外 容量倒序 防止复用**

01背包问题就是 ： 放入一次或者不放，得到极值

<img src="https://file1.kamacoder.com/i/algo/20210110103003361.png" alt="动态规划-背包问题1" style="zoom:50%;" />

递推公式：

```java
dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
```

解题步骤：

1. 确定dp数组  dp[i] 或者 dp\[i]\[j]  并确定含义
2. 初始化dp数组
3. 确定 dp 递推公式  
4. 根据递推公式判断 数据从哪里计算得出，确定递归顺序



一维dp数组遍历顺序

```java
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}

dp[j - weight[i]] 来自哪里？j 是从 大 → 小；所以 dp[j - weight[i]] 还没被第 i 个物品更新
它仍然表示：“只使用前 i-1 个物品时的最优解”；因此，第 i 个物品只会被用 一次

for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = 0; j <= weight[i]; j++) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
这样是错误的，因为 这样写 会把同一个物品用多次，问题语义被破坏；
为什么会同一个物品使用多次，因为 dp[j - weight[i]] 在同一轮已经包含第 i 个物品，导致状态被重复叠加


也不能反过来，像这样
for(int i = 0; i < bagWeight; i++) { // 遍历背包
    for(int j = 0; j < weight.size(); j++) { // 遍历物品
    }
}
错误点在于：dp[j] 的状态无法区分“用了哪些物品” 
在 01 背包中，隐含的二维状态是：dp[i][j] = 用前 i 个物品，容量 j 的最大价值
压缩成一维后，必须靠循环顺序来维持 i 的边界


```

为什么可以使用一维数组，因为可以复用；

而且**遍历背包的顺序是不一样的**，**倒序遍历是为了保证物品i只被放入一次**

倒序遍历背包容量 = 当前物品只写一次，永远读不到自己



[416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

先获得 sum / 2 = target; 再使用 01背包问题，看能不能填满 这个背包

dp[j] 表示：在当前可用的数字中，背包容量为 j 时，能凑出的最大子集和 

因为 把 target 分成两份了，所以一定要找最大的子集和，没有其他隐含结果集



#### 完全背包问题

完全背包问题就是 ： 放入n次或者不放，得到极值

[279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

每一个 n 的结果 可以由 前面的结果获得，有点类似前缀和

```java
// 物品
for( int i = 1; i <= n ; i++){
  for( int j = i *i; j <= n;j++){
    if (dp[j - i * i] != Integer.MAX_VALUE) {
      dp[j] = Math.min(dp[j], dp[j - i * i] + 1);
    }
  }
}
```



[322. 零钱兑换](https://leetcode.cn/problems/coin-change/)

初始化：由于要找最小，所以初始为 最大，也就是 amount + 1, 因为 coins[i] > 0

动归：背包大小是 coins[j] 而不是 j

两种写法：

```java
//先遍历背包
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        // 背包
        for(int i = 0; i <= amount; i++){
            // 物品
            for(int j = 0; j < coins.length;j++ ){
                if(coins[j] > i){
                    continue;
                }
                dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}

//先遍历物品
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        // 物品
        for (int i = 0; i < coins.length; i++) {
            // 背包
            for (int j = coins[i]; j <= amount; j++) {
                    dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```



[139. 单词拆分](https://leetcode.cn/problems/word-break/)

还是 完全背包问题



## 单调栈

**通常是一维数组，要寻找任一个元素的右边或者左边第一个比自己大或者小的元素的位置**

用一个“保持单调顺序的栈”，在一次线性遍历中，快速找到每个元素左右第一个比它大或小的元素



[239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

使用单调栈维护 窗口中的最大值



## 排序算法

#### 选择排序

时间复杂度：O(n^2)

稳定性：不稳定

两个for循环，外层循环确定比较的基点，里层从基点+1 往后遍历，找到比基点小的值; 目的是要找到最小的值，所以里层循环也要遍历到最后；然后交换 基点值 和 找到最小的值 的值

外层循环确定 排序点放的位置，里层循环找到最小值

```java
public int[] selectionSort(int[] nums) {
    if(nums == null || nums.length == 0) {
        return nums;
    }
    
    for (int i = 0; i < nums.length - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[j] < nums[minIndex]) {
                minIndex = j;
            }
        }
        if (minIndex != i) {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
    }
    return nums;
}
```



#### 插入排序

时间复杂度：O(n^2)

排序稳定性：稳定

把数组分为两部分，一部分是已经排好序的，一部分是没有排好序的；外层循环 找到待排序的点，里层(while)找到 比待排序小的index；注意，里层在寻找的时候，要把已经排好序的但是比 待排序值 的位置往后移动，腾位置给待排序的值

类似斗地主时整理扑克牌

```java
public int[] insertionSort(int[] nums) {
    if (nums == null || nums.length == 0) {
        return nums;
    }

    for (int i = 1; i < nums.length; i++) {
        int current = nums[i];
        int j = i - 1;
        while (j >= 0 && current < nums[j]) {
            nums[j + 1] = nums[j];
            j--;
        }
        nums[j + 1] = current;
    }

    return nums;
}
```





#### 快速排序

时间复杂度：O(nlogn)

排序稳定性：不稳定

递归方式；把大问题分化为小问题，最小是两个节点，马上可以得出结果；

选定一个基点；把比基点小的放到左边，比基点大的放到右边；然后把 基点放到中间的位置，因为只承诺左右两边比base大或者小，所以换了不会导致乱序

```java
class Solution {
    public int[] sortArray(int[] nums) {
        quickSort(nums, 0, nums.length - 1);
        return nums;
    }

    public void quickSort(int[] nums, int start, int end) {
        if (start >= end) {
            return;
        }
        int base = nums[start];
        int i = start + 1;
        int j = end;
        // 为什么可以 i++ j-- ;不会产生覆盖吗
        // 当 nums[i] >= base 时，nums[i] 交换 nums[j],由于 i 没有 ++，所以下一轮校验的就是换过来的 nums[j] 此时在num s[i] 位置上
        while (i <= j) {
            if (nums[i] <= base) {
                i++;
            } else {
                swap(nums, i, j);
                j--;
            }
        }
        // while (i <= j) 退出时，一定是 left = right + 1; 而不是 left == right
        swap(nums, start, j);
        // 所以真正的分界线时 right 也就是 这里的 j
        quickSort(nums, start, j - 1);
        quickSort(nums, j + 1, end);
    }

    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}

```

迭代写法

```java
class Solution {
    public int[] sortArray(int[] nums) {
        quickSort(nums);
        return nums;
    }

    private void quickSort(int[] nums) {
        Deque<int[]> stack = new ArrayDeque<>();
        stack.push(new int[]{0, nums.length - 1});

        while (!stack.isEmpty()) {
            int[] range = stack.pop();
            int l = range[0], r = range[1];
            if (l >= r) continue;

            int base = nums[l];
            int left = l + 1;
            int right = r;

            while (left <= right) {
                if (nums[left] <= base) {
                    left++;
                } else {
                    swap(nums, left, right);
                    right--;
                }
            }

            // pivot 放到最终位置
            swap(nums, l, right);

            // 子区间
            stack.push(new int[]{l, right - 1});
            stack.push(new int[]{right + 1, r});
        }
    }

    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```





#### 归并排序

递归分问题，拿到递归结果合结果

```java
class Solution {
    public int[] sortArray(int[] nums) {
        if (nums.length == 1)
            return nums;
        int length = nums.length;
        int mid = length / 2;

        int[] sub1 = sortArray(Arrays.copyOfRange(nums, 0, mid));
        int[] sub2 = sortArray(Arrays.copyOfRange(nums, mid, length));

        return merge(sub1, sub2);
    }

    private int[] merge(int[] nums1, int[] nums2) {
        int[] res = new int[nums1.length + nums2.length];
        int i = 0, j = 0, k = 0;

        while (i < nums1.length && j < nums2.length) {
            if (nums1[i] <= nums2[j]) {
                res[k++] = nums1[i++];
            } else {
                res[k++] = nums2[j++];
            }
        }

        // 处理剩余部分
        while (i < nums1.length) {
            res[k++] = nums1[i++];
        }
        while (j < nums2.length) {
            res[k++] = nums2[j++];
        }

        return res;
    }

}
```







#### 冒泡排序



#### 希尔排序



#### 堆排序



## 图论

图的遍历方式：

1. 深度优先遍历 dfs 需要回溯

   ```java
   void dfs(参数) {
       处理节点
       dfs(图，选择的节点); // 递归
       回溯，撤销处理结果
   }
   ```

   

2. 广度优先遍历 bfs 解决两个点之间的最短路径问题

   ```java
       private void bfs(char[][] grid, int i, int j) {
           Queue<int[]> queue = new LinkedList<>();
           queue.offer(new int[]{i, j});
           while (!queue.isEmpty()) {
               int[] cur = queue.poll();
           }
   ```

   



[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

使用dfs，把找到的 == 1 的节点置为 0



[994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/)

题意是 每个腐烂橘子都会传染，所以不能使用简单的 bfs，那就不是每个坏橘子在同时传染了



遍历：获取新鲜橘子的数量，把坏橘子的位置入栈

bfs对栈的每个橘子进行bfs，bfs一层就是一分钟

bfs中每一个方式如果是新鲜橘子，那么新鲜橘子数量 -1

最后判断新鲜橘子的数量是否为0



## 技巧

[136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

异或的本质就是：成对抵消，孤立留下

> 异或满足交换律、结合律
> a ^ b ^ a = (a ^ a) ^ b = 0 ^ b = b

```java
class Solution {
    public int singleNumber(int[] nums) {
       int sum = 0;
        for (int num : nums) {
            sum ^= num;
        }
        return sum;
    }
}
```



[169. 多数元素](https://leetcode.cn/problems/majority-element/)

> 多数元素出现次数 > n / 2
>  → 把它和其他元素两两抵消，最后剩下的一定是它
>
> 投票，相等 + 1，不想等 -1

```java
class Solution {
    public int majorityElement(int[] nums) {
        int candidate = 0;
        int count = 0;
        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }
        return candidate;
    }
}
```



[287. 寻找重复数](https://leetcode.cn/problems/find-the-duplicate-number/)

- 数组长度 `n + 1`
- 数字范围 `1 ~ n`
- **只有一个重复的数**（可能出现多次）
- **不能修改数组**
- **O(1) 额外空间**

 **标准解法：快慢指针（Floyd 判圈算法）**

把数组看成一个 **隐式链表**：

把数组看成这样： 下标 i  ──▶  nums[i]  也就是nnext(i) = nums[i];

- 下标是「节点」
- `nums[i]` 是「next 指针」

```xml
i -> nums[i]
```

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0];
        int fast = nums[nums[0]];
        // 1. 找到相遇点
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }
        // 2. 找到环入口
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
}
```

