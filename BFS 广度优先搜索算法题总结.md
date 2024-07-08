---
tags:
  - 算法题
---
一般用来解决最短情况的问题
树的层序遍历也是这个思路！
# 通用实现方式
---
BFS大致的实现方式，就是使用**队列**来进行图的**层序遍历**！
例如：[1306. 跳跃游戏 III](https://leetcode.cn/problems/jump-game-iii/)
```java
Queue<> q;
Set<> set;   //记录是否已经被访问过

q.offer(起始节点);
set.add(起始节点);
while(!q.isEmpty()) {
	
	q.poll() //取出本节点
	
	// 判断是否为终止条件
	// 相应处理
	
	// 将下一层（不在set中）节点入队，并计入set
}
```


但是对于需要有额外要求的bfs，比如需要最短步数记录的这种，就需要更加复杂的实现方式。一般考虑两种：

## 1. 双队列
使用两层队列来倒每一层，能够清晰地看到每一层的遍历、下一层的导入
```java
Queue<> q1;
Set<> set;   //记录是否已经被访问过
int step = 0;

q1.offer(起始节点);
set.add(起始节点);
while(true) {
	Queue<> q2;
	step++;
	//遍历一层
	while(!q1.isEmpty()) {
		q.poll() //取出本节点
		// 判断是否为终止条件
		// 相应处理
		// 将下一层（不在set中）节点入q2队，并计入set
	}

	//准备下一层
	if(q2.isEmpty()) {
		//搜索结束，终止
	}
	// 将q2全部倒入q1
}
```
## 2. 自设节点的单队列
通过节点来存储step等信息，加入队列后能通过节点中的字段来判断层数和其他信息，自定义，更加灵活
```java
class Node { 
	int index; 
	int step; 
	Node(int index, int step) { this.index = index; this.step = step; } 
}

Queue<Node> q;
Set<> set;   //记录是否已经被访问过

q1.offer(new Node(起始位置， 0));
set.add(起始节点)
while(!q.isEmpty()) {
	q.poll() //取出本节点
	
	// 判断是否为终止条件
	// 相应处理
	
	// 将下一层（不在set中）节点入队，入队时会创Node，也就会有step的记录
}
```

# 注意细节
---
对于不同题，使用上面的模版即可解决，但是要注意一些特别的调整：
1. 遍历下一层的节点，节点一般是常数个。如果可能达到O(N)的复杂度，即使加了set防止重复进入也有可能超时。因为极端情况下，每次都要判断所有可能节点的被访问情况，就算不进入，也会使得时间复杂度上一个台阶。**要避免重复的多次一样的set判断**！
	- 解决方式：对这种情况的题，具体分析重复的原因，使用方法识别并剪掉
	- 比如：[1345. 跳跃游戏 IV](https://leetcode.cn/problems/jump-game-iv/)、[LCP 09. 最小跳跃次数](https://leetcode.cn/problems/zui-xiao-tiao-yue-ci-shu/)
1. 对于是否被访问的情况记录，一般来说简单记录节点入set即可。但是有一些特殊情况，导致**即使每次访问到一样的节点，可能会出现不同的子节点情况，这样可能会让一些路线被忽略掉**。
	- 解决方式：需要对访问情况做更加细微的记录。这时候就要对set进行特别的设计。
	- 比如：[1654. 到家的最少跳跃次数](https://leetcode.cn/problems/minimum-jumps-to-reach-home/)