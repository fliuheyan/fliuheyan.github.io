## Tree

### 从数组构建一棵二叉树
[3,9,20,null,null,15,7]
对于当前节点i, 它的left node 为2n+1, right node为2n+2

### 计算一棵树的高度

### 102 binary tree order traversal. 
1. 首先将head节点push到stack当中
2. 利用stack来进行每一层的遍历
3. 定义两个变量tempStack，将当前层的children加入到其中
    tempList,当前层的值
```
while (!stack.isEmpty()) {
    Stack<TreeNode> tempStack = new Stack<>();
    List<Integer> tempList = new ArrayList<>();
    //先遍历stack，同时将当前层中的所有children加入到tempStack当中
    //当前层的值加入到tempList
    for (TreeNode node : stack) {
```

### 107 Binary Tree Level Order Traversal II  
1. 102题数组reverse

### 199. Binary Tree Right Side View
1. 102题，tempStack只存入right child node

### 103	Binary Tree Zigzag Level Order Traversal  

### 94 Binary Tree Inorder Traversal  
`https://mp.weixin.qq.com/s/PwVIfxDlT3kRgMASWAMGhA`

1.「确定递归函数的参数和返回值：」确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。

2.「确定终止条件：」写完了递归算法,  运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。

3.「确定单层递归的逻辑：」确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

### 144 Binary Tree Preorder Traversal  

### 145 Binary Tree Postorder Traversal  

### B Tree

### B+ Tree

### Red Black Tree

### AVL Tree
