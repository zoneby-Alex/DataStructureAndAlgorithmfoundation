/* 二叉树节点类 */
class TreeNode {
    int val;         // 节点值
    TreeNode left;   // 左子节点引用
    TreeNode right;  // 右子节点引用
    TreeNode(int x) { val = x; }
}
根节点（root node）：位于二叉树顶层的节点，没有父节点。
叶节点（leaf node）：没有子节点的节点，其两个指针均指向 None 。
边（edge）：连接两个节点的线段，即节点引用（指针）。
节点所在的层（level）：从顶至底递增，根节点所在层为 1 。
节点的度（degree）：节点的子节点的数量。在二叉树中，度的取值范围是 0、1、2 。
二叉树的高度（height）：从根节点到最远叶节点所经过的边的数量。
节点的深度（depth）：从根节点到该节点所经过的边的数量。
节点的高度（height）：从距离该节点最远的叶节点到该节点所经过的边的数量。
// 初始化节点
TreeNode n1 = new TreeNode(1);
TreeNode n2 = new TreeNode(2);
TreeNode n3 = new TreeNode(3);
TreeNode n4 = new TreeNode(4);
TreeNode n5 = new TreeNode(5);
// 构建节点之间的引用（指针）
n1.left = n2;
n1.right = n3;
n2.left = n4;
n2.right = n5;
TreeNode P = new TreeNode(0);
// 在 n1 -> n2 中间插入节点 P
n1.left = P;
P.left = n2;
// 删除节点 P
n1.left = n2;
完美二叉树（perfect binary tree）所有层的节点都被完全填满。在完美二叉树中，叶节点的度为 
 ，其余所有节点的度都为 0 ；若树的高度为 2 ，则节点总数为 （2**h+1）-1

二叉树常见的遍历方式包括层序遍历、前序遍历、中序遍历和后序遍历等。
层序遍历本质上属于广度优先遍历（breadth-first traversal），也称广度优先搜索（breadth-first search, BFS），它体现了一种“一圈一圈向外扩展”的逐层遍历方式。
/* 层序遍历 */
List<Integer> levelOrder(TreeNode root) {
    // 初始化队列，加入根节点
    Queue<TreeNode> queue = new LinkedList<>();
    queue.add(root);
    // 初始化一个列表，用于保存遍历序列
    List<Integer> list = new ArrayList<>();
    while (!queue.isEmpty()) {
        TreeNode node = queue.poll(); // 队列出队
        list.add(node.val);           // 保存节点值
        if (node.left != null)
            queue.offer(node.left);   // 左子节点入队
        if (node.right != null)
            queue.offer(node.right);  // 右子节点入队
    }
    return list;
}
前序、中序和后序遍历都属于深度优先遍历（depth-first traversal），也称深度优先搜索（depth-first search, DFS），它体现了一种“先走到尽头，再回溯继续”的遍历方式。
前序 根节点-左子树-左子树左子节点-左子树右子节点-右子树-右子树左子节点-右子树右子节点
中序 左子树左子节点-左子树-左子树右子节点-根节点-右子树左子节点-右子树-右子树右子节点
后序 左子树左子节点-左子树右子节点-左子树-右子树左子节点-右子树右子节点-右子树-根节点
/* 前序遍历 */
void preOrder(TreeNode root) {
    if (root == null)
        return;
    // 访问优先级：根节点 -> 左子树 -> 右子树
    list.add(root.val);
    preOrder(root.left);
    preOrder(root.right);
}
/* 中序遍历 */
void inOrder(TreeNode root) {
    if (root == null)
        return;
    // 访问优先级：左子树 -> 根节点 -> 右子树
    inOrder(root.left);
    list.add(root.val);
    inOrder(root.right);
}
/* 后序遍历 */
void postOrder(TreeNode root) {
    if (root == null)
        return;
    // 访问优先级：左子树 -> 右子树 -> 根节点
    postOrder(root.left);
    postOrder(root.right);
    list.add(root.val);
}
二叉树数组表示
/* 数组表示下的二叉树类 */
class ArrayBinaryTree {
    private List<Integer> tree;
    /* 构造方法 */
    public ArrayBinaryTree(List<Integer> arr) {
        tree = new ArrayList<>(arr);
    }
    /* 列表容量 */
    public int size() {
        return tree.size();
    }
    /* 获取索引为 i 节点的值 */
    public Integer val(int i) {
        // 若索引越界，则返回 null ，代表空位
        if (i < 0 || i >= size())
            return null;
        return tree.get(i);
    }
    /* 获取索引为 i 节点的左子节点的索引 */
    public Integer left(int i) {
        return 2 * i + 1;
    }
    /* 获取索引为 i 节点的右子节点的索引 */
    public Integer right(int i) {
        return 2 * i + 2;
    }
    /* 获取索引为 i 节点的父节点的索引 */
    public Integer parent(int i) {
        return (i - 1) / 2;
    }
    /* 层序遍历 */
    public List<Integer> levelOrder() {
        List<Integer> res = new ArrayList<>();
        // 直接遍历数组
        for (int i = 0; i < size(); i++) {
            if (val(i) != null)
                res.add(val(i));
        }
        return res;
    }
    /* 深度优先遍历 */
    private void dfs(Integer i, String order, List<Integer> res) {
        // 若为空位，则返回
        if (val(i) == null)
            return;
        // 前序遍历
        if ("pre".equals(order))
            res.add(val(i));
        dfs(left(i), order, res);
        // 中序遍历
        if ("in".equals(order))
            res.add(val(i));
        dfs(right(i), order, res);
        // 后序遍历
        if ("post".equals(order))
            res.add(val(i));
    }
    /* 前序遍历 */
    public List<Integer> preOrder() {
        List<Integer> res = new ArrayList<>();
        dfs(0, "pre", res);
        return res;
    }
    /* 中序遍历 */
    public List<Integer> inOrder() {
        List<Integer> res = new ArrayList<>();
        dfs(0, "in", res);
        return res;
    }
    /* 后序遍历 */
    public List<Integer> postOrder() {
        List<Integer> res = new ArrayList<>();
        dfs(0, "post", res);
        return res;
    }
}
二叉树的数组表示主要有以下优点。
数组存储在连续的内存空间中，对缓存友好，访问与遍历速度较快。
不需要存储指针，比较节省空间。
允许随机访问节点。
然而，数组表示也存在一些局限性。
数组存储需要连续内存空间，因此不适合存储数据量过大的树。
增删节点需要通过数组插入与删除操作实现，效率较低。
当二叉树中存在大量 None 时，数组中包含的节点数据比重较低，空间利用率较低。

二叉搜索树（binary search tree）满足以下条件。
对于根节点，左子树中所有节点的值< 根节点的值< 右子树中所有节点的值。
任意节点的左、右子树也是二叉搜索树，即同样满足条件 1. 
二叉搜索树封装为一个类 BinarySearchTree ，并声明一个成员变量 root ，指向树的根节点。
给定目标节点值 num ，可以根据二叉搜索树的性质来查找
声明一个节点 cur ，从二叉树的根节点 root 出发，循环比较节点值 cur.val 和 num 之间的大小关系。
若 cur.val < num ，说明目标节点在 cur 的右子树中，因此执行 cur = cur.right 。
若 cur.val > num ，说明目标节点在 cur 的左子树中，因此执行 cur = cur.left 。
若 cur.val = num ，说明找到目标节点，跳出循环并返回该节点
/* 查找节点 */
TreeNode search(int num) {
    TreeNode cur = root;
    // 循环查找，越过叶节点后跳出
    while (cur != null) {
        // 目标节点在 cur 的右子树中
        if (cur.val < num)
            cur = cur.right;
        // 目标节点在 cur 的左子树中
        else if (cur.val > num)
            cur = cur.left;
        // 找到目标节点，跳出循环
        else
            break;
    }
    // 返回目标节点
    return cur;
}
给定一个待插入元素 num ，保持二叉搜索树“左子树 < 根节点 < 右子树”的性质
查找插入位置：与查找操作相似，从根节点出发，根据当前节点值和 num 的大小关系循环向下搜索，直到越过叶节点（遍历至 None ）时跳出循环。
在该位置插入节点：初始化节点 num ，将该节点置于 None 的位置
二叉搜索树不允许存在重复节点，否则将违反其定义。因此，若待插入节点在树中已存在，则不执行插入，直接返回。
为了实现插入节点，我们需要借助节点 pre 保存上一轮循环的节点。这样在遍历至 None 时，我们可以获取到其父节点，从而完成节点插入操作。
/* 插入节点 */
void insert(int num) {
    // 若树为空，则初始化根节点
    if (root == null) {
        root = new TreeNode(num);
        return;
    }
    TreeNode cur = root, pre = null;
    // 循环查找，越过叶节点后跳出
    while (cur != null) {
        // 找到重复节点，直接返回
        if (cur.val == num)
            return;
        pre = cur;
        // 插入位置在 cur 的右子树中
        if (cur.val < num)
            cur = cur.right;
        // 插入位置在 cur 的左子树中
        else
            cur = cur.left;
    }
    // 插入节点
    TreeNode node = new TreeNode(num);
    if (pre.val < num)
        pre.right = node;
    else
        pre.left = node;
}
在二叉树中查找到目标节点，再将其删除
当待删除节点的度为0时，表示该节点是叶节点，可以直接删除。
当待删除节点的度为1时，将待删除节点替换为其子节点即可。  
当待删除节点的度为2时，我们无法直接删除它，而需要使用一个节点替换该节点。由于要保持二叉搜索树“左子树<根节点<右子树的性质，因此这个节点可以是右子树的最小节点或左子树的最大节点。
找到待删除节点在“中序遍历序列”中的下一个节点，记为 tmp 。
用 tmp 的值覆盖待删除节点的值，并在树中递归删除节点 tmp 。
/* 删除节点 */
void remove(int num) {
    // 若树为空，直接提前返回
    if (root == null)
        return;
    TreeNode cur = root, pre = null;
    // 循环查找，越过叶节点后跳出
    while (cur != null) {
        // 找到待删除节点，跳出循环
        if (cur.val == num)
            break;
        pre = cur;
        // 待删除节点在 cur 的右子树中
        if (cur.val < num)
            cur = cur.right;
        // 待删除节点在 cur 的左子树中
        else
            cur = cur.left;
    }
    // 若无待删除节点，则直接返回
    if (cur == null)
        return;
    // 子节点数量 = 0 or 1
    if (cur.left == null || cur.right == null) {
        // 当子节点数量 = 0 / 1 时， child = null / 该子节点
        TreeNode child = cur.left != null ? cur.left : cur.right;
        // 删除节点 cur
        if (cur != root) {
            if (pre.left == cur)
                pre.left = child;
            else
                pre.right = child;
        } else {
            // 若删除节点为根节点，则重新指定根节点
            root = child;
        }
    }
    // 子节点数量 = 2
    else {
        // 获取中序遍历中 cur 的下一个节点
        TreeNode tmp = cur.right;
        while (tmp.left != null) {
            tmp = tmp.left;
        }
        // 递归删除节点 tmp
        remove(tmp.val);
        // 用 tmp 覆盖 cur
        cur.val = tmp.val;
    }
}
在二叉搜索树中进行中序遍历时，总是会优先遍历下一个最小节点，从而得出一个重要性质：二叉搜索树的中序遍历序列是升序的。