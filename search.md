二分查找（binary search）是一种基于分治策略的高效搜索算法。它利用数据的有序性，每轮缩小一半搜索范围，直至找到目标元素或搜索区间为空为止
/* 二分查找（双闭区间） */
int binarySearch(int[] nums, int target) {
    // 初始化双闭区间 [0, n-1] ，即 i, j 分别指向数组首元素、尾元素
    int i = 0, j = nums.length - 1;
    // 循环，当搜索区间为空时跳出（当 i > j 时为空）
    while (i <= j) {
        int m = i + (j - i) / 2; // 计算中点索引 m
        if (nums[m] < target) // 此情况说明 target 在区间 [m+1, j] 中
            i = m + 1;
        else if (nums[m] > target) // 此情况说明 target 在区间 [i, m-1] 中
            j = m - 1;
        else // 找到目标元素，返回其索引
            return m;
    }
    // 未找到目标元素，返回 -1
    return -1;
}
/* 二分查找（左闭右开区间） */
int binarySearchLCRO(int[] nums, int target) {
    // 初始化左闭右开区间 [0, n) ，即 i, j 分别指向数组首元素、尾元素+1
    int i = 0, j = nums.length;
    // 循环，当搜索区间为空时跳出（当 i = j 时为空）
    while (i < j) {
        int m = i + (j - i) / 2; // 计算中点索引 m
        if (nums[m] < target) // 此情况说明 target 在区间 [m+1, j) 中
            i = m + 1;
        else if (nums[m] > target) // 此情况说明 target 在区间 [i, m) 中
            j = m;
        else // 找到目标元素，返回其索引
            return m;
    }
    // 未找到目标元素，返回 -1
    return -1;
}
/* 二分查找插入点（无重复元素） */
int binarySearchInsertionSimple(int[] nums, int target) {
    int i = 0, j = nums.length - 1; // 初始化双闭区间 [0, n-1]
    while (i <= j) {
        int m = i + (j - i) / 2; // 计算中点索引 m
        if (nums[m] < target) {
            i = m + 1; // target 在区间 [m+1, j] 中
        } else if (nums[m] > target) {
            j = m - 1; // target 在区间 [i, m-1] 中
        } else {
            return m; // 找到 target ，返回插入点 m
        }
    }
    // 未找到 target ，返回插入点 i
    return i;
}
/* 二分查找插入点（存在重复元素） */
int binarySearchInsertion(int[] nums, int target) {
    int i = 0, j = nums.length - 1; // 初始化双闭区间 [0, n-1]
    while (i <= j) {
        int m = i + (j - i) / 2; // 计算中点索引 m
        if (nums[m] < target) {
            i = m + 1; // target 在区间 [m+1, j] 中
        } else if (nums[m] > target) {
            j = m - 1; // target 在区间 [i, m-1] 中
        } else {
            j = m - 1; // 首个小于 target 的元素在区间 [i, m-1] 中
        }
    }
    // 返回插入点 i
    return i;
}
/* 二分查找最左一个 target */
int binarySearchLeftEdge(int[] nums, int target) {
    // 等价于查找 target 的插入点
    int i = binary_search_insertion.binarySearchInsertion(nums, target);
    // 未找到 target ，返回 -1
    if (i == nums.length || nums[i] != target) {
        return -1;
    }
    // 找到 target ，返回索引 i
    return i;
}
/* 二分查找最右一个 target */
int binarySearchRightEdge(int[] nums, int target) {
    // 转化为查找最左一个 target + 1
    int i = binary_search_insertion.binarySearchInsertion(nums, target + 1);
    // j 指向最右一个 target ，i 指向首个大于 target 的元素
    int j = i - 1;
    // 未找到 target ，返回 -1
    if (j == -1 || nums[j] != target) {
        return -1;
    }
    // 找到 target ，返回索引 j
    return j;
}

哈希优化策略
/* 方法一：暴力枚举 */
int[] twoSumBruteForce(int[] nums, int target) {
    int size = nums.length;
    // 两层循环，时间复杂度为 O(n^2)
    for (int i = 0; i < size - 1; i++) {
        for (int j = i + 1; j < size; j++) {
            if (nums[i] + nums[j] == target)
                return new int[] { i, j };
        }
    }
    return new int[0];
}
/* 方法二：辅助哈希表 */
int[] twoSumHashTable(int[] nums, int target) {
    int size = nums.length;
    // 辅助哈希表，空间复杂度为 O(n)
    Map<Integer, Integer> dic = new HashMap<>();
    // 单层循环，时间复杂度为 O(n)
    for (int i = 0; i < size; i++) {
        if (dic.containsKey(target - nums[i])) {
            return new int[] { dic.get(target - nums[i]), i };
        }
        dic.put(nums[i], i);
    }
    return new int[0];
}

search algo
搜索算法（searching algorithm）用于在数据结构（例如数组、链表、树或图）中搜索一个或一组满足特定条件的元素。
搜索算法可根据实现思路分为以下两类。
通过遍历数据结构来定位目标元素，例如数组、链表、树和图的遍历等。
利用数据组织结构或数据包含的先验信息，实现高效元素查找，例如二分查找、哈希查找和二叉搜索树查找等。
暴力搜索通过遍历数据结构的每个元素来定位目标元素。
“线性搜索”适用于数组和链表等线性数据结构。它从数据结构的一端开始，逐个访问元素，直到找到目标元素或到达另一端仍没有找到目标元素为止。
“广度优先搜索”和“深度优先搜索”是图和树的两种遍历策略。广度优先搜索从初始节点开始逐层搜索，由近及远地访问各个节点。深度优先搜索从初始节点开始，沿着一条路径走到头，再回溯并尝试其他路径，直到遍历完整个数据结构。
自适应搜索利用数据的特有属性（例如有序性）来优化搜索过程，从而更高效地定位目标元素。
“二分查找”利用数据的有序性实现高效查找，仅适用于数组。
“哈希查找”利用哈希表将搜索数据和目标数据建立为键值对映射，从而实现查询操作。
“树查找”在特定的树结构（例如二叉搜索树）中，基于比较节点值来快速排除节点，从而定位目标元素。(需要对数据进行预处理)
线性搜索
通用性较好，无须任何数据预处理操作。假如我们仅需查询一次数据，那么其他三种方法的数据预处理的时间比线性搜索的时间还要更长。
适用于体量较小的数据，此情况下时间复杂度对效率影响较小。
适用于数据更新频率较高的场景，因为该方法不需要对数据进行任何额外维护。
二分查找
适用于大数据量的情况，效率表现稳定，最差时间复杂度为O(logn)
数据量不能过大，因为存储数组需要连续的内存空间。
不适用于高频增删数据的场景，因为维护有序数组的开销较大。
哈希查找
适合对查询性能要求很高的场景，平均时间复杂度为 O（1）
不适合需要有序数据或范围查找的场景，因为哈希表无法维护数据的有序性。
对哈希函数和哈希冲突处理策略的依赖性较高，具有较大的性能劣化风险。
不适合数据量过大的情况，因为哈希表需要额外空间来最大程度地减少冲突，从而提供良好的查询性能。
树查找
适用于海量数据，因为树节点在内存中是分散存储的。
适合需要维护有序数据或范围查找的场景。
在持续增删节点的过程中，二叉搜索树可能产生倾斜，时间复杂度劣化至 O（n）
若使用 AVL 树或红黑树，则各项操作可在O（logn）效率下稳定运行，但维护树平衡的操作会增加额外的开销。
