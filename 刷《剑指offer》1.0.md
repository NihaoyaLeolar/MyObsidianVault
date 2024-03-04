### 05. 替换空格
[LCR 122. 路径加密](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)
简单的字符串处理，没有啥特别的
### 06. 从尾到头打印链表 
[LCR 123. 图书整理 I](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)
- 模拟：正向遍历存入数组，然后把数组倒过来（可以考虑使用辅助栈，也是一样）
- 反转链表，我直接把链表反转了（肯定不是最优解，为了练习）
- 使用递归（递归本质就是一个栈）写，其实就是先计算链表大小，然后给数组反向填写了，正着写也行。（使用递归都要考虑到递归太深会栈溢出）
### 18. 删除链表的节点 
[LCR 136. 删除链表的节点](https://leetcode.cn/problems/shan-chu-lian-biao-de-jie-dian-lcof/)
- 直接简单模拟编码实现：从链表头开始顺序查找，找到就删除，没啥问题
- 这里主要注意**链表删除操作的思路**： ...-> A -> B -> C ->... 假设是要删除B
	- 一般我之前的思路：获取到B的上一个节点A，将A指向C（主要到删除头节点的情况）
	- 补充一种思路：将C的val以及next赋值到B上（但这要注意到删除尾节点的情况）

### 20. 表示数值的字符串
[LCR 138. 有效数字](https://leetcode.cn/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)
- 之前很多次才过，这次一遍过了，代码思路有提升
- 有限状态机的解法看了个一知半解，没有深入
### 24. 反转链表
[LCR 141. 训练计划 III](https://leetcode.cn/problems/fan-zhuan-lian-biao-lcof/)
- 直接使用迭代写，在06题我就写了，之前2023年8月写的迭代更加简洁点...
- 使用递归写，知道可以用，就是没写出来。需要加强一下对递归的理解和编写思路了！

### 35. 复杂链表的复制
[LCR 154. 复杂链表的复制](https://leetcode.cn/problems/fu-za-lian-biao-de-fu-zhi-lcof/)
- 先复制val和next，正常写就行，就是普通的链表复制（用递归写很简单）。第一遍复制时建立复制和被复制节点的映射，然后第二遍根据映射表复制random。这个很容易想到，之前也是这么想的，就是第一遍复制时用递归更好！（更清晰的是，直接最开始只复制节点，放到映射表中，然后一次性遍历复制）
- 先将链表double再切割！！！ 💡💡💡
### 58 - II. 左旋转字符串
[LCR 182. 动态口令](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)
- 调用substring然后拼接，很简单的想到
- 看书上说，可以利用I的反转函数，**进行三次反转**
### 67. 把字符串转化成整数 
[LCR 192. 把字符串转换成整数 (atoi)](https://leetcode.cn/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)
- 主要是注意考虑特殊情况和边界情况，对于整型越界，可以采除法先倒过来试探

  


