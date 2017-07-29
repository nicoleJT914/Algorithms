森林：m棵互不相交的树的集合

二叉树：最多两棵子树

满二叉树：叶子只能在最后一层

完全二叉树：二叉树不能出现空档，叶子结点只能出现在最后两层

满二叉树是完全二叉树
***
二叉树的性质：
- 第i层至多有2^(i-1)个结点
- 深度为k（二叉树的层数）的二叉树至多有2^k-1个结点
- 终端结点数为n0，度为2的结点数为n2，则n0=n2+1
- 具有n个结点的完全二叉树的深度为Math.floor(lgn+1)
- 对n个结点的完全二叉树编号为1~n，对i>1的结点，双亲结点为Math.cell(i/2)；左孩子为2i；右孩子为2i+1
***

二叉查找树

每个结点x，左子树所有项小于x，右子树所有项大于x

BST的平均深度是O(lgN)

ADT:
```java

```
BST的平均时间复杂度为O(logn)，但是在最坏的情况下仍然会有O(n)的时间复杂度

在一棵查找二叉树中，所有操作在最坏情况下所需的时间都和树的高度成正比

以上也是设计AVL的初衷

***
