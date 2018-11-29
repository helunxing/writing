#### [探索 初级算法 内容记录](https://leetcode-cn.com/explore/interview/card/top-interview-questions-easy/)
* 数组：26,189,136,350,1
* 字符串：344,387
* 链表：21,206,141
* 二叉树：104,98

#### 1 两数之和
头回见到hashmap的题

#### 19 删除链表倒数第N个
如果前指针在执行while之前已经为空，则引发新情况。现在的优化可能还有提升空间。不到三天，我已经完全忘了这道题怎么做…

#### 21 合并两个有序链表
有序是指升序。 注意两个表中某个表为空的问题，当前代码可能还可以优化。

#### 26 移除有序数组中的重复值
优化了一波时间反而变慢了，时间统计可能有问题。

#### 28 实现strStr()
未考虑串与子串相同的情况
这道题三天还没有做出来。很慌。

#### 34 在排序数组中查找元素的第一个和最后一个位置
```
prel  l   m   r   prer    |
  >   >                   |l=m,repl=l
  >   <                   |l=l&prel aver
              <   <       |r=m,prer=r
              >   <       |r=r&prer aver
```
方法：二分法要注意左等于中，右等于左加一的情况

二分法要特别注意：n和n+1的平均数是n

#### 35 搜索插入位置
特殊情况：列表为空
正常情况下解的类型：0,n
二分法演变结果，肯定会经过l+1=r的状态

#### 53 最大子序和
这道题难度竟然也是简单。默认解法是穷举法吗？

不可先合并再计算，全负数序列无法通过。

最开始我觉得需要这道题需要记录三个量：
*第i个之前最大连续子序和（不必须包括i-1）
*上数是否包含第i-1个数
*以i-1为结尾的最大和

后来在实现的时候，觉得第二个判定的逻辑比较复杂。而且第一个数的开始位置未知，想不出来如何实现
现在把这个想法写出来，发现判定第一个和第三个是否相等就根本不用存第二个量。

所以就在网上找答案了。动态规划法利用了最大序列和的以下特点：第一个数不会是是负数，同理得出，该序列的任一个开头位置与其相同的子序列也不为负数。有了这个重要的特点，就的出了需要记录的状态：
1. i-1为结尾的最大子序列和sum
1. 历史最大子序列和maxsum

步骤如下：
1. 若sum<0,则sum=curr（利用之前说的重要的特点）。否则sum+=curr。
1. 若sum>maxsum，maxsum=sum

由于 重要的特点 ，sum将是一直正确的：如果前一个sum>0，则curr累加即可。否则说明前面的应该抛弃，不放在最优子序列里。

#### 70 爬楼梯
未考虑台阶总数为1，2时。

#### 82 链表重复全删
链表方法：
想明白再写。
需要写后续步骤和特殊情况，想到就马上加。
步骤细节加注释。

#### 83 移除有序链表中的重复值
问题的核心是如何避免访问空指针。 当前节点不为空，保障的是的是顺序下行（此外还有传入值为空表的情况） 当前节点的下节点不为空，保障的是删除节点，如果下节点为空则会异常访问无法比较。

#### 84 翻车现场
与黄金问题相比，此问题的
* 最优子结构：
三种选一，三种分别为：最小值左侧所有的最优解，最小值右侧的最优解，最小值乘范围宽度
* 状态转移公式：
min=min(n,m)
f(n,m)=max(  a(n-m)  , f )
* 边界：
当n=m时，面积为1

递归解法超时，Python在做某些事的时候棘手。遍历特定范围元素值的时候容易搞错，容易混乱。

解决了混乱的python代码仍超时，可能是因为重复计算边界的原因。

混乱解决了依旧超时，也许需要用非递归计算。网上找了个答案依旧超时。非递归朴素算法超时。朴素解法、递归解法都会超时，想不到其他办法了，换语言了。
java递归成功，但成绩不好，排名靠前的有优化。用python写算法可能不是个正确的选择…

方法：
* 如何通过一维的想到二维（85）的解法。
* 思路：先想暴力法，计算过程中，观察题目的特点。
* 递归边界值处理的问题：如果后边界下标的值就是最小值，会出现因前后下标相等产生的数组越界问题。
以后要注意，后边界下标和前边界下标被包括应当相同。不然越界。

lt高效解法阅读：
左大于右、左右相等情况返回值处理。
记录是否是升序。记录最小值下标。
记录升序原因：升序可以用当前值表示当前到最有最大面积

2018年11月9日 对高效算法的分析
优化内容：
问题：为什么检测升序能提高这么多性能？
可能和升序区域使用穷举法遍历效率高有关。

#### 85 最大矩形
给出的数据需要更改。为了方便查询，应当将非零数改为此位置逆扫描方向有多少个数。

#### 98 验证搜索二叉树
未考虑到子树中的最大值最小值。
这道题的优秀解法都没用到hashmap。
问题：看懂优秀解法，尤其是栈实现的。

#### 141 检测链表中的环
方法：在有两种返回值的情况下，如果默认返回值就是包含为空或者仅一个输入之类特殊值的返回值，会使判断逻辑更简单。

#### 153 寻找旋转排序数组中的最小值/顺序移动过的有序数组中的最小值
```
特殊情况：len(nums)为零,sloved
l m r     |
 < <      |上坡，最高点已被错过，和上一步有关
 > <      |min is between l&m.r replace by m.if m is not mid, r=mid-1.else return mid
 < >      |min is between m&r.l replcae by m.if m is not mid, l=mid+1.else return mid
 > >      |不存在

l m r     |
base > <
 =        |l,m is min
   =      |l,r,m is min same
 = =      |same as upon
base < >
 =        |r is min
   =      |r,l,m is min same
 = =      |same as upon
```
0答非所问：题目要求值，我给的是编号。
方法：先把题意写草稿。

2018年11月8日1死循环：未考虑l，r命中min的情况，min偏1赋给l或r不科学：m确定了不是最小的，却把m偏1这个未知的给出去。

2018年11月9日2死循环：未考虑极端值全升序。
方法：将特殊值写道草稿首。

#### 344 反转字符串
https://www.cnblogs.com/JohnTsai/p/5606719.html

#### 350 两个数组的交集2
hashmap实现。记录其出现次数。
探索 数组
如果是排序过的，则类似两个排序链表的合并。
若其中一个的数目远大于另一个，将小的存为哈希表，键值对为值，次数。大的存新哈希表，如果小哈希表中有再建立数据。统计个数。
hash表

#### 374 猜数字
要注意mid在相加除二过程中的溢出问题。