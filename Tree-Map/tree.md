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

AVL带有平衡条件的BST：每个结点的左子树和右子树的高度最多差1

AVL将树的深度控制在O(lgN)，来改善最坏情况下的时间复杂度

平衡的关键是在插入结点后，可能破坏原来的平衡，若将必须重新平衡的节点叫做x，则不平衡可能出现下面四种情况：
1. x ==> left ==> left ==> insert
2. x ==> right ==> right ==> insert
3. x ==> left ==> rifht ==> insert
4. x ==> right ==> left ==> insert

对于1和2情况，使用单旋转即可恢复平衡；对于3和4情况，需要使用双旋转才可恢复平衡。旋转后，要及时更新节点的高度值

AVL是一种高度平衡的树，每次插入或删除都需要大量的旋转来保持平衡，所以实际应用中更多使用红黑树。

如果插入和删除操作不多，只是查找的话，AVL比红黑树还是要快的。

ADT:
```java

```

***

伸展树保证从空树开始连续M次对树的操作最多花费O(MlnN)时间
 
伸展树是根据数据访问的局部性：
1. 刚刚被访问的节点，极有可能在不久之后再次被访问到；
2. 将被访问的下一个节点，极有可能就处于不久之前被访问过的某个节点的附近；

伸展树不要求保留高度或平衡信息

伸展树的基本思想：当一个节点被访问后，就要经过一系列AVL树的旋转被推到根上

展开操作不仅将访问的节点移动到根处，而且还把访问路径上的大部分节点的深度大致减半

***

红黑树的高度最多是2lg(N+1)

对红黑树的操作最坏情形下花费O(lgN)时间

***

