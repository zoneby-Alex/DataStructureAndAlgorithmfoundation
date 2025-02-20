堆（heap）是一种满足特定条件的完全二叉树
小顶堆（min heap）：任意节点的值<=其子节点的值。
大顶堆（max heap）：任意节点的值 >=其子节点的值。

最底层节点靠左填充，其他层的节点都被填满。
我们将二叉树的根节点称为“堆顶”，将底层最靠右的节点称为“堆底”。
对于大顶堆（小顶堆），堆顶元素（根节点）的值是最大（最小）的。
优先队列（priority queue），这是一种抽象的数据结构，定义为具有优先级排序的队列。
堆通常用于实现优先队列，大顶堆相当于元素按从大到小的顺序出队的优先队列
/* 初始化堆 */
// 初始化小顶堆
Queue<Integer> minHeap = new PriorityQueue<>();
// 初始化大顶堆（使用 lambda 表达式修改 Comparator 即可）
Queue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
/* 元素入堆 */
maxHeap.offer(1);
maxHeap.offer(3);
maxHeap.offer(2);
maxHeap.offer(5);
maxHeap.offer(4);
/* 获取堆顶元素 */
int peek = maxHeap.peek(); // 5
/* 堆顶元素出堆 */
// 出堆元素会形成一个从大到小的序列
peek = maxHeap.poll(); // 5
peek = maxHeap.poll(); // 4
peek = maxHeap.poll(); // 3
peek = maxHeap.poll(); // 2
peek = maxHeap.poll(); // 1
/* 获取堆大小 */
int size = maxHeap.size();
/* 判断堆是否为空 */
boolean isEmpty = maxHeap.isEmpty();
/* 输入列表并建堆 */
minHeap = new PriorityQueue<>(Arrays.asList(1, 3, 2, 5, 4));
堆实现 大顶堆转换为小顶堆，只需将所有大小逻辑判断进行逆转
堆的存储与表示 用数组来存储堆 使用数组表示二叉树时，元素代表节点值，索引代表节点在二叉树中的位置。节点指针通过索引映射公式来实现
给定索引 i ，其左子节点的索引为 2i+1 ，右子节点的索引为 2i+2 ，父节点的索引为 （i-1）/2（向下整除）。当索引越界时，表示空节点或节点不存在。
/* 获取左子节点的索引 */
int left(int i) {
    return 2 * i + 1;
}
/* 获取右子节点的索引 */
int right(int i) {
    return 2 * i + 2;
}
/* 获取父节点的索引 */
int parent(int i) {
    return (i - 1) / 2; // 向下整除
}
/* 访问堆顶元素 */
int peek() {
    return maxHeap.get(0);
}
元素入堆 给定元素 val ，我们首先将其添加到堆底。添加之后，由于 val 可能大于堆中其他元素，堆的成立条件可能已被破坏，因此需要修复从插入节点到根节点的路径上的各个节点，这个操作被称为堆化（heapify）。从入堆节点开始，从底至顶执行堆化。比较插入节点与其父节点的值，如果插入节点更大，则将它们交换。然后继续执行此操作，从底至顶修复堆中的各个节点，直至越过根节点或遇到无须交换的节点时结束。
/* 元素入堆 */
void push(int val) {
    // 添加节点
    maxHeap.add(val);
    // 从底至顶堆化
    siftUp(size() - 1);
}
/* 从节点 i 开始，从底至顶堆化 */
void siftUp(int i) {
    while (true) {
        // 获取节点 i 的父节点
        int p = parent(i);
        // 当“越过根节点”或“节点无须修复”时，结束堆化
        if (p < 0 || maxHeap.get(i) <= maxHeap.get(p))
            break;
        // 交换两节点
        swap(i, p);
        // 循环向上堆化
        i = p;
    }
}
堆顶元素出堆 堆顶元素是二叉树的根节点，即列表首元素。如果我们直接从列表中删除首元素，那么二叉树中所有节点的索引都会发生变化，这将使得后续使用堆化进行修复变得困难
交换堆顶元素与堆底元素（交换根节点与最右叶节点）。
交换完成后，将堆底从列表中删除（注意，由于已经交换，因此实际上删除的是原来的堆顶元素）。
从根节点开始，从顶至底执行堆化(将根节点的值与其两个子节点的值进行比较，将最大的子节点与根节点交换。然后循环执行此操作，直到越过叶节点或遇到无须交换的节点时结束。)
/* 元素出堆 */
int pop() {
    // 判空处理
    if (isEmpty())
        throw new IndexOutOfBoundsException();
    // 交换根节点与最右叶节点（交换首元素与尾元素）
    swap(0, size() - 1);
    // 删除节点
    int val = maxHeap.remove(size() - 1);
    // 从顶至底堆化
    siftDown(0);
    // 返回堆顶元素
    return val;
}
/* 从节点 i 开始，从顶至底堆化 */
void siftDown(int i) {
    while (true) {
        // 判断节点 i, l, r 中值最大的节点，记为 ma
        int l = left(i), r = right(i), ma = i;
        if (l < size() && maxHeap.get(l) > maxHeap.get(ma))
            ma = l;
        if (r < size() && maxHeap.get(r) > maxHeap.get(ma))
            ma = r;
        // 若节点 i 最大或索引 l, r 越界，则无须继续堆化，跳出
        if (ma == i)
            break;
        // 交换两节点
        swap(i, ma);
        // 循环向下堆化
        i = ma;
    }
}
堆的常见应用
优先队列：堆通常作为实现优先队列的首选数据结构，其入队和出队操作的时间复杂度均为 O(logn) ，而建堆操作为 O(n) ，这些操作都非常高效。
堆排序：给定一组数据，我们可以用它们建立一个堆，然后不断地执行元素出堆操作，从而得到有序数据。
获取最大的 k个元素：这是一个经典的算法问题，同时也是一种典型应用，例如选择热度前 10 的新闻作为微博热搜，选取销量前 10 的商品等。
建堆操作
入堆操作 创建一个空堆，然后遍历列表，依次对每个元素执行“入堆操作”，即先将元素添加至堆的尾部，再对该元素执行“从底至顶”堆化
遍历堆化
将列表所有元素原封不动地添加到堆中，此时堆的性质尚未得到满足。
倒序遍历堆（层序遍历的倒序），依次对每个非叶节点执行“从顶至底堆化”
/* 构造方法，根据输入列表建堆 */
MaxHeap(List<Integer> nums) {
    // 将列表元素原封不动添加进堆
    maxHeap = new ArrayList<>(nums);
    // 堆化除叶节点以外的其他所有节点
    for (int i = parent(size() - 1); i >= 0; i--) {
        siftDown(i);
    }
}
Top-k 问题 给定一个长度为n的无序数组 nums ，请返回数组中最大的k个元素。
  遍历
  排序
  堆
初始化一个小顶堆，其堆顶元素最小。
先将数组的前k个元素依次入堆。从第k个元素开始，若当前元素大于堆顶元素，则将堆顶元素出堆，并将当前元素入堆。
遍历完成后，堆中保存的就是最大的k个元素。
/* 基于堆查找数组中最大的 k 个元素 */
Queue<Integer> topKHeap(int[] nums, int k) {
    // 初始化小顶堆
    Queue<Integer> heap = new PriorityQueue<Integer>();
    // 将数组的前 k 个元素入堆
    for (int i = 0; i < k; i++) {
        heap.offer(nums[i]);
    }
    // 从第 k+1 个元素开始，保持堆的长度为 k
    for (int i = k; i < nums.length; i++) {
        // 若当前元素大于堆顶元素，则将堆顶元素出堆、当前元素入堆
        if (nums[i] > heap.peek()) {
            heap.poll();
            heap.offer(nums[i]);
        }
    }
    return heap;
}