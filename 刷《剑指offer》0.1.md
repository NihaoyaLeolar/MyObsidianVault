## 08-08
- [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)
	- 很简单
- [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode.cn/problems/fan-zhuan-dan-ci-shun-xu-lcof/)
	- 做起来很麻烦，字符串处理要考虑很多边界，还有空格多打的问题
	- 看书上说，可以**考虑两层次反转完成**，这点比较妙
- [剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)
	- 调用substring然后拼接，很简单的想到
	- 看书上说，可以利用上一题的反转函数，**进行三次反转**

## 08-10
- [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode.cn/problems/fan-zhuan-dan-ci-shun-xu-lcof/)
	- 参考别人的评论，**善用trim等函数，简化字符串处理的复杂度**！
- [剑指 Offer 20. 表示数值的字符串](https://leetcode.cn/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)
	- 做起来很麻烦，错了很多用例才过，字符串处理还是不够娴熟
- [剑指 Offer 67. 把字符串转换成整数](https://leetcode.cn/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)
	- 还行，题目有点不明白意思，整体简单
- [剑指 Offer 06. 从尾到头打印链表](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)
	- 正向遍历，直接使用stack或者啥啥先装着，然后粘贴出来。
	- 看到一个妙的，**使用递归**，这样就可反转了！
- [剑指 Offer 35. 复杂链表的复制](https://leetcode.cn/problems/fu-za-lian-biao-de-fu-zhi-lcof/)  💡💡💡
	- 还算顺畅，直接用hashmap存储映射，但是就是内存卡空间用的比较多
	- 根据题解，有不需要哈希的方式，直接**创建双倍链表**，然后cut，注意要还原原来的链表！
	- 明天可以试试把第一次的两次遍历改成递归+回溯实现一下！
## 08-14
- [剑指 Offer 18. 删除链表的节点](https://leetcode.cn/problems/shan-chu-lian-biao-de-jie-dian-lcof/)
	- 很简单，删除操作就是使用下一个覆盖上一个的next，注意特殊边界就可以
- [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode.cn/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)
	- 很简单，书上还提到了拓展性。在面试表达的时候，要知道分析模块和变化，会拆解代码，知道只需要改变什么地方！
- [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode.cn/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)   💡💡💡
	- 我想到的是统计链表长度！然后找到倒数第k个是正数第几个！
	- 题解：**使用双指针，两个指针之间相差k**
- [剑指 Offer 25. 合并两个排序的链表](https://leetcode.cn/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)
	- 没啥要记录的，很简单，就是注意特殊边界
	- 有个小改进：然后最后只剩下一个链表的时候，不需要循环复制粘贴了，直接粘贴一个节点就可以
- [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode.cn/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)
	- 暴力双层遍历！最差方法！时间复杂度O(n^2),空间复杂度O(1)
	- 使用set集合，找到重复的就返回！时间复杂度O(n+m),空间复杂度O(n+m)
	- 看书上的差一点的方法：既然是后端对齐，并且链表不能从后往前遍历，使用栈！
		- 这里有一个很重要的分析，让我的整个理解不到位 ———— **链表相交后不会再分离！不然也不叫相同节点了！**
	- 看评论区：讲两个链表末端对齐，然后截去多余的，从头开始找.时间复杂度O(n+m),空间复杂度O(1。书上也只给到这个方法！
	- 题解：太妙了！**直接拼接ab、ba，总会相遇**！时间复杂度O(n+m),空间复杂度O(1）。数学上很好证明，就是要想到拼接这回事！实现时可以不拼接，思路是拼接的！
- [剑指 Offer 57. 和为s的两个数字](https://leetcode.cn/problems/he-wei-sde-liang-ge-shu-zi-lcof/)
	- 用二分法查找就可以了
	- 看了官方题解的，两端双指针查找也可实现，而且比上面快！我的第一感觉就是这个，但是没有严谨想想就pass了...

## 08-15
- [剑指 Offer 09. 用两个栈实现队列](https://leetcode.cn/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)
	- 我想到的办法太简单了，每次读取都要倒两次！可以**等到第二个stack空了才倒**！平时正常从s2读取，从s1输入

>栈是可以实现 O(1) 时间随时取出最小值的，同时不影响 O(1)的 push 和 pop
- [剑指 Offer 30. 包含min函数的栈](https://leetcode.cn/problems/bao-han-minhan-shu-de-zhan-lcof/)
	- 个人不成熟思路：栈+优先队列+set查询是否还在栈内。空间开销过大
	- 题解思路：使用**辅助栈**，与原栈同高，用于存储同高元素入栈时的min / 或者直接入栈时入两个元素：数字与当前栈内最小值（双层栈）。
	- 更优思路：**差值栈** ———— 存入x时，改为存入 data = x - min，并更新min

## 08-16
>之前的栈是单口，但是队列是双口，所以之前记录最大值的方式无效。
>但是队列也可以实现 O(1) 时间随时取出最小值的，只是需要额外空间申请一个单调队列！
>看书上说也可以用两个差值栈来实现，原则上确实可以！
- [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode.cn/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

	- 暴力太慢，这里可以过，但是O(Nk)的复杂度不是很能接受
	- **优先队列法**：每次将二元组{val, index}入大根堆，取出时验证index。O(nlogn)
	- **单调队列法**：维护一个（下标对应值）单调递减的队列，存储数组下标，取出时验证index。O(n)
- [剑指 Offer 59 - II. 队列的最大值](https://leetcode.cn/problems/dui-lie-de-zui-da-zhi-lcof/)
	- 这个比上面的更简单，就是一个纯粹的**单调队列**的题，这里不存下标
		- 其实我想到上面也可以不存下标来做单调队列的，直接每次滑动都进行pop，并且该值在单调队列头则也pop
		- 如果不存储下标做单调队列，要注意，维护单调队列入队时，如果和末端元素相等，也要压入！只有完全小于才弹出！

了解java的linkedlist、queue、dequeue等
- [剑指 Offer 29. 顺时针打印矩阵](https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)
	- 很简单的模拟
- [剑指 Offer 31. 栈的压入、弹出序列](https://leetcode.cn/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)
	- 很简单的模拟

## 08-22
- [剑指 Offer 03. 数组中重复的数字](https://leetcode.cn/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/?envType=study-plan-v2&envId=coding-interviews)
	- 桶计数，时间复杂度O(n)，空间复杂度O(n)
	- 书上有一种，改变原数组，从而省去空间复杂度的方法：把每个元素都对应**移**到应该在的下标对应的位置上！时间复杂度O(n)，空间复杂度0！
	- 另外，如果需要空间复杂度0且不改变原数组，可以有时间复杂度O(nlogn)的办法：每次统计0～n/2一半的数字出现的次数，如果超过了一半，则继续在这一半查找！
- [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode.cn/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/description/?envType=study-plan-v2&envId=coding-interviews)
	- 直接二分就可以，x出现的次数：（x+1第一次出现的下标）-（x第一次出现的下标）
- [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/?envType=study-plan-v2&envId=coding-interviews)
	- **一定要好好思考和翻译题目的条件**！我忽略了数组本身的递增特性：找到内容和下标不相等的地方，前一个就是答案！
- [剑指 Offer 04. 二维数组中的查找](https://leetcode.cn/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/description/?envType=study-plan-v2&envId=coding-interviews)
	- 我的方法是二层维度的二分法，其实严格意义上不是二分，是3/4分，一定要考虑好了再下手，最开始直接1/4有点想当然了。这应该是时间复杂度最低的！
	- 更妙的办法是转45度，看成**二叉树**，每次左右移动查询即可！但是时间复杂度O(m + n)，没有上面低！
		- 以前我的理解：一直循环遍历查找合适范围的右上角，不断删去右列和上列，直到右上角刚好是 target 。如果找不到则不存在。这个有点像二叉搜索树（站在右上角来决策：往左 or 往下）
## 08-23
- 剑指 Offer 11. 旋转数组的最小数字
	- 很简单能想到，但是要举出特殊的用例，仔细分析，flag是选择最左边还是最右边！
	- **当相等的时候，直接省去一个右边界元素！这个是我没想到的！**
- 剑指 Offer 50. 第一个只出现一次的字符
	- 桶计数
	- 书上用的哈希表，

## 09-05
- 剑指 Offer 32 - I. 从上到下打印二叉树
- 剑指 Offer 32 - II. 从上到下打印二叉树 II
# 需要查缺补漏的
----
在刷题时专注刷题，发现一些漏洞先记录下来，抽时间补上！
## Java语法
- 一些set、list、map等的使用，基本语法、时间复杂度

## 底层原理
- 熟悉以上collection的具体底层实现原理
	- 数组桶 vs 哈希表 ？ 空间上还是时间上区别？ 感觉前者肯定更好啊，需要看看底层

## 算法
- 各个排序算法的实现以及对比，需要重新温故一下

