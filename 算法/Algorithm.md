

# 算法

- 因为是递归，使用函数后可认为左右子树已经算出结果



## 常用技巧

### 输入

在Java中，`next`和`nextLine`方法用于读取用户的输入，它们的主要区别在于对输入内容中空格的处理方式。

- `next`方法：
  - 它会自动地消除有效字符之前的空格，只返回输入的字符。
  - 当遇到有效字符时，它会继续读取直到遇到空格、Tab键或Enter键等结束符。
  - 如果输入的字符序列中包含空格，`next`方法会忽略这些空格，直到遇到有效字符。
  - `next`方法不会得到带有空格的字符串。
- `nextLine`方法：
  - 它返回的是Enter键之前的所有字符，包括输入的任何空白字符。
  - `nextLine`方法可以接受所有字符，包括空格，直到遇到回车键。
  - `nextLine`方法能够得到带有空格的字符串。

### 基本数据结构

```java
public class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { 
        this.val = val; 
    }
    ListNode(int val, ListNode next) { 
        this.val = val; 
        this.next = next; 
    }
}

public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { 
        this.val = val; 
    }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```



```java
public V getOrDefault(Object key, V defaultValue) 
//当Map集合中有这个key时，就返回这个key对应的value值，如果没有这个key就返回默认值defaultValue
//map中若没有key则把key的value设置为0, 若有key则把key的value设置为原有值上+1
map.put(key, map.getOrDefault(key, 0) + 1);
```



### 集合和数组

**集合转数组：**使用集合的 `toArray(T[] array)`，传入的是类型完全一致、长度为 0 的空数组。

```java
String [] s= new String[]{
    "dog", "lazy", "a", "over", "jumps", "fox", "brown", "quick", "A"
};
List<String> list = Arrays.asList(s);
Collections.reverse(list);
//没有指定类型的话会报错
s=list.toArray(new String[0]);
```

**数组转集合：**使用工具类 `Arrays.asList()` 把数组转换成集合时，不能使用其修改集合相关的方法， 它的 `add/remove/clear` 方法会抛出 `UnsupportedOperationException` 异常。

`Arrays.asList()`在平时开发中还是比较常见的，我们可以使用它将一个数组转换为一个 `List` 集合。

```java
String[] myArray = {"Apple", "Banana", "Orange"};
List<String> myList = Arrays.asList(myArray);
//上面两个语句等价于下面一条语句
List<String> myList = Arrays.asList("Apple","Banana", "Orange");
```

下面我们来总结一下使用注意事项。

**1、`Arrays.asList()`是泛型方法，传递的数组必须是对象数组，而不是基本类型。**

```java
int[] myArray = {1, 2, 3};
List myList = Arrays.asList(myArray);
System.out.println(myList.size());//1
System.out.println(myList.get(0));//数组地址值
System.out.println(myList.get(1));//报错：ArrayIndexOutOfBoundsException
int[] array = (int[]) myList.get(0);
System.out.println(array[0]);//1
```

当传入一个原生数据类型数组时，`Arrays.asList()` 的真正得到的参数就不是数组中的元素，而是数组对象本身！此时 `List` 的唯一元素就是这个数组，这也就解释了上面的代码。

我们使用包装类型数组就可以解决这个问题。

```java
Integer[] myArray = {1, 2, 3};
```

**2、使用集合的修改方法: `add()`、`remove()`、`clear()`会抛出异常。**

```java
List myList = Arrays.asList(1, 2, 3);
myList.add(4);//运行时报错：UnsupportedOperationException
myList.remove(1);//运行时报错：UnsupportedOperationException
myList.clear();//运行时报错：UnsupportedOperationException
```

`Arrays.asList()` 方法返回的并不是 `java.util.ArrayList` ，而是 `java.util.Arrays` 的一个内部类,这个内部类并没有实现集合的修改方法或者说并没有重写这些方法。

```java
List myList = Arrays.asList(1, 2, 3);
System.out.println(myList.getClass());//class java.util.Arrays$ArrayList
```



**集合判空：**判断所有集合内部的元素是否为空，使用 `isEmpty()` 方法，而不是 `size()==0` 的方式。

**集合转 Map：**在使用 `java.util.stream.Collectors` 类的 `toMap()` 方法转为 `Map` 集合时，一定要注意当 value 为 null 时会抛 NPE 异常。

**集合遍历：**

**集合去重：**可以利用 `Set` 元素唯一的特性，可以快速对一个集合进行去重操作，避免使用 `List` 的 `contains()` 进行遍历去重或者判断包含操作。



## 二分查找

### 最基本的二分查找算法

```java
int binary_search(int[] nums, int target) {
    int left = 0, right = nums.length - 1; 
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1; 
        } else if(nums[mid] == target) {
            // 直接返回
            return mid;
        }
    }
    // 直接返回
    return -1;
}
```

### 寻找左侧边界的二分查找

```java
int left_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定左侧边界
            right = mid - 1;
        }
    }
    // 判断 target 是否存在于 nums 中
    if (left < 0 || left >= nums.length) {
        return -1;
    }
    // 判断一下 nums[left] 是不是 target
    return nums[left] == target ? left : -1;
}
```

### 寻找右侧边界的二分查找

```java
int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定右侧边界
            left = mid + 1;
        }
    }
    // 判断 target 是否存在于 nums 中
    // if (left - 1 < 0 || left - 1 >= nums.length) {
    //     return -1;
    // }
    
    // 由于 while 的结束条件是 right == left - 1，且现在在求右边界
    // 所以用 right 替代 left - 1 更好记
    if (right < 0 || right >= nums.length) {
        return -1;
    }
    return nums[right] == target ? right : -1;
}
```

## 并查集

Union-Find 算法的复杂度可以这样分析：构造函数初始化数据结构需要 O(N) 的时间和空间复杂度；连通两个节点 `union`、判断两个节点的连通性 `connected`、计算连通分量 `count` 所需的时间复杂度均为 O(1)。

- 用 `parent` 数组记录每个节点的父节点，相当于指向父节点的指针，所以 `parent` 数组内实际存储着一个森林（若干棵多叉树）。
- 在 `find` 函数中进行路径压缩，保证任意树的高度保持在常数，使得各个 API 时间复杂度为 O(1)。使用了路径压缩之后，可以不使用 `size` 数组的平衡优化。

```java
class UnionFind {
    // 连通分量个数
    private int count;
    // 存储每个节点的父节点
    private int[] parent;

    // n 为图中节点的个数
    public UnionFind(int n) {
        this.count = n;
        parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    // 将节点 p 和节点 q 连通
    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        
        if (rootP == rootQ)
            return;
        
        parent[rootQ] = rootP;
        // 两个连通分量合并成一个连通分量
        count--;
    }

    // 判断节点 p 和节点 q 是否连通
    public boolean connected(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        return rootP == rootQ;
    }

    public int find(int x) {
        if (parent[x] != x) {
            //路径压缩
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    // 返回图中的连通分量个数
    public int count() {
        return count;
    }
}
```

### 被围绕的区域（力扣130）

给你一个 M×N 的二维矩阵，其中包含字符 `X` 和 `O`，让你找到矩阵中**四面**被 `X` 围住的 `O`，并且把它们替换成 `X`。必须是四面被围的 `O` 才能被换成 `X`，也就是说边角上的 `O` 一定不会被围，进一步，与边角上的 `O` 相连的 `O` 也不会被 `X` 围四面，也不会被替换。

可以把那些不需要被替换的 `O` 看成一个拥有独门绝技的门派，它们有一个共同「祖师爷」叫 `dummy`，这些 `O` 和 `dummy` 互相连通，而那些需要被替换的 `O` 与 `dummy` 不连通。

首先要解决的是，根据我们的实现，Union-Find 底层用的是一维数组，构造函数需要传入这个数组的大小，而题目给的是一个二维棋盘。

这个很简单，二维坐标 `(x,y)` 可以转换成 `x * n + y` 这个数（`m` 是棋盘的行数，`n` 是棋盘的列数），**敲黑板，这是将二维坐标映射到一维的常用技巧**。

其次，我们之前描述的「祖师爷」是虚构的，需要给他老人家留个位置。索引 `[0.. m*n-1]` 都是棋盘内坐标的一维映射，那就让这个虚拟的 `dummy` 节点占据索引 `m * n` 好了。

```java
void solve(char[][] board) {
    if (board.length == 0) return;

    int m = board.length;
    int n = board[0].length;
    // 给 dummy 留一个额外位置
    UF uf = new UF(m * n + 1);
    int dummy = m * n;
    // 将首列和末列的 O 与 dummy 连通
    for (int i = 0; i < m; i++) {
        if (board[i][0] == 'O')
            uf.union(i * n, dummy);
        if (board[i][n - 1] == 'O')
            uf.union(i * n + n - 1, dummy);
    }
    // 将首行和末行的 O 与 dummy 连通
    for (int j = 0; j < n; j++) {
        if (board[0][j] == 'O')
            uf.union(j, dummy);
        if (board[m - 1][j] == 'O')
            uf.union(n * (m - 1) + j, dummy);
    }
    // 方向数组 d 是上下左右搜索的常用手法
    int[][] d = new int[][]{{1,0}, {0,1}, {0,-1}, {-1,0}};
    for (int i = 1; i < m - 1; i++) 
        for (int j = 1; j < n - 1; j++) 
            if (board[i][j] == 'O')
                // 将此 O 与上下左右的 O 连通
                for (int k = 0; k < 4; k++) {
                    int x = i + d[k][0];
                    int y = j + d[k][1];
                    if (board[x][y] == 'O')
                        uf.union(x * n + y, i * n + j);
                }
    // 所有不和 dummy 连通的 O，都要被替换
    for (int i = 1; i < m - 1; i++) 
        for (int j = 1; j < n - 1; j++) 
            if (!uf.connected(dummy, i * n + j))
                board[i][j] = 'X';
}

class UnionFind {
    // 见上文
}
```

这段代码很长，其实就是刚才的思路实现，只有和边界 `O` 相连的 `O` 才具有和 `dummy` 的连通性，他们不会被替换。

其实用 Union-Find 算法解决这个简单的问题有点杀鸡用牛刀，它可以解决更复杂，更具有技巧性的问题，**主要思路是适时增加虚拟节点，想办法让元素「分门别类」，建立动态连通关系**。

###  等式方程的可满足性（力扣990）

给你一个数组 `equations`，装着若干字符串表示的算式。每个算式 `equations[i]` 长度都是 4，而且只有这两种情况：`a==b` 或者 `a!=b`，其中 `a,b` 可以是任意小写字母。你写一个算法，如果 `equations` 中所有算式都不会互相冲突，返回 true，否则返回 false。

比如说，输入 `["a==b","b!=c","c==a"]`，算法返回 false，因为这三个算式不可能同时正确。

再比如，输入 `["c==c","b==d","x!=z"]`，算法返回 true，因为这三个算式并不会造成逻辑冲突。

**核心思想是，将 `equations` 中的算式根据 `==` 和 `!=` 分成两部分，先处理 `==` 算式，使得他们通过相等关系各自勾结成门派（连通分量）；然后处理 `!=` 算式，检查不等关系是否破坏了相等关系的连通性**。

```java
boolean equationsPossible(String[] equations) {
    // 26 个英文字母
    UF uf = new UF(26);
    // 先让相等的字母形成连通分量
    for (String eq : equations) {
        if (eq.charAt(1) == '=') {
            char x = eq.charAt(0);
            char y = eq.charAt(3);
            uf.union(x - 'a', y - 'a');
        }
    }
    // 检查不等关系是否打破相等关系的连通性
    for (String eq : equations) {
        if (eq.charAt(1) == '!') {
            char x = eq.charAt(0);
            char y = eq.charAt(3);
            // 如果相等关系成立，就是逻辑冲突
            if (uf.connected(x - 'a', y - 'a'))
                return false;
        }
    }
    return true;
}

class UF {
    // 见上文
}
```

## 排序算法

### 各种排序算法的比较

- 每趟可以确定一个值的排序算法：冒泡排序，选择排序，快速排序
- 空间复杂度较高的排序算法：快速排序O(log n)，归并排序O(n)，基数排序O(n + k)

| 排序算法     | 平均时间复杂度 | 最好情况时间复杂度 | 最坏情况时间复杂度 | 空间复杂度   | 稳定性     |
| ------------ | :------------- | ------------------ | ------------------ | ------------ | ---------- |
| 冒泡排序     | O(n^2)         | O(n)               | O(n^2)             | O(1)         | 稳定       |
| 选择排序     | O(n^2)         | O(n^2)             | O(n^2)             | O(1)         | 不稳定     |
| 插入排序     | O(n^2)         | O(n)               | O(n^2)             | O(1)         | 稳定       |
| **希尔排序** | **O(n log n)** | **O(n log n)**     | **O(n^2)**         | **O(1)**     | **不稳定** |
| **归并排序** | **O(n log n)** | **O(n log n)**     | **O(n log n)**     | **O(n)**     | **稳定**   |
| **快速排序** | **O(n log n)** | **O(n log n)**     | **O(n^2)**         | **O(log n)** | **不稳定** |
| **堆排序**   | **O(n log n)** | **O(n log n)**     | **O(n log n)**     | **O(1)**     | **不稳定** |
| 计数排序     | O(n + k)       | O(n + k)           | O(n + k)           | O(n + k)     | 稳定       |
| 桶排序       | O(n + k)       | O(n)               | O(n^2)             | O(n + k)     | 稳定       |
| 基数排序     | O(d * (n + k)) | O(d * (n + k))     | O(d * (n + k))     | O(n + k)     | 稳定       |

<img src = "Algorithmimages\sort.JPG">

### 快速排序（固定选择基准元素）

快速排序是一种高效的排序算法，它基于分治法（Divide and Conquer）的思想。其主要思想是选择一个基准元素（pivot），将数组分成两部分，一部分的元素都小于基准元素，另一部分的元素都大于等于基准元素。然后递归地对这两部分进行排序，直到整个数组有序。

以下是快速排序的基本步骤：

1. **选择基准元素（Pivot）：** 从数组中选择一个元素作为基准元素。通常情况下选择第一个元素、最后一个元素、中间元素或随机元素作为基准元素。
2. **划分（Partitioning）：** 将数组中小于基准元素的元素放在基准元素的左边，大于等于基准元素的元素放在基准元素的右边，基准元素本身放在其最终的位置上。这一步操作完成后，基准元素处于它最终的位置，它不再改变位置。
3. **递归排序：** 递归地对基准元素左右两边的子数组进行排序。对左右子数组重复步骤1和步骤2，直到整个数组有序。

快速排序的特点包括：

- **时间复杂度：** 平均情况下，快速排序的时间复杂度为**O(nlogn)**，其中n是数组的大小。

  在最好的情况下，每次选择的基准元素刚好能将数组分成大小大致相等的两部分。这种情况下，快速排序的时间复杂度是最优的，为**O(nlogn)**。

  最坏情况通常发生在**数组已经基本有序的情况下**，而选择的基准元素恰好是数组中的最小或最大元素，导致划分非常不平衡，其中一个子数组为空，另一个子数组包含了所有的元素。这种情况下，快速排序的时间复杂度达到了最差情况，为**O(n^2)**。

- **空间复杂度：** 快速排序的空间复杂度通常为 **O(logn)**，其中 n 是数组的大小。

- **稳定性：** 快速排序是一种**不稳定**的排序算法，相同元素的相对位置在排序之后可能会改变。

```java
public static void main(String[] args) {
    quickSort(nums, 0, nums.length - 1);    
}

// 快速排序方法
public static int[] quickSort(int[] nums, int low, int high) {
    if (low < high) {
        // pi 是划分元素的索引，nums[pi] 正确地放置在最终排序数组中的位置
        int pi = partition(nums, low, high);

        // 递归地对划分的两个子数组进行排序
        quickSort(nums, low, pi - 1);
        quickSort(nums, pi + 1, high);
    }
    return nums; // 返回排序后的数组
}

// 划分方法，以基准元素为准将数组划分为两个子数组
public static int partition(int[] nums, int low, int high) {
    // 选择最后一个元素作为基准元素
    int pivot = nums[high];
    int i = low - 1; // i 是小于基准元素的区域的边界

    // 遍历数组，将小于基准元素的元素放到左边，大于等于基准元素的元素放到右边
    for (int j = low; j < high; j++) {
        if (nums[j] < pivot) {
            i++;
            // 交换 nums[i] 和 nums[j]
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
    }

    // 将基准元素放到正确的位置上
    int temp = nums[i + 1];
    nums[i + 1] = nums[high];
    nums[high] = temp;

    // 返回基准元素的索引
    return i + 1;
}
```

### 快速排序（随机选择基准元素）

```java
public static void main(String[] args) {
    quickSort(nums, 0, nums.length - 1);
}

// 快速排序方法
public static void quickSort(int[] nums, int low, int high) {
    if (low < high) {
        // 选择随机基准元素的索引
        int randomIndex = randomPartition(nums, low, high);

        // 递归地对划分的两个子数组进行排序
        quickSort(nums, low, randomIndex - 1);
        quickSort(nums, randomIndex + 1, high);
    }
}

// 划分方法，以随机选择的基准元素将数组划分为两个子数组
public static int randomPartition(int[] nums, int low, int high) {
    // 生成 low 和 high 之间的随机索引
    int randomIndex = new Random().nextInt(high - low + 1) + low;

    // 将随机索引处的元素与最后一个元素交换，使其成为基准元素
    int temp = nums[randomIndex];
    nums[randomIndex] = nums[high];
    nums[high] = temp;

    return partition(nums, low, high);
}

// 划分方法，以基准元素为准将数组划分为两个子数组
public static int partition(int[] nums, int low, int high) {
    int pivot = nums[high];
    int i = low - 1; 

    for (int j = low; j < high; j++) {
        if (nums[j] < pivot) {
            i++;
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
    }

    int temp = nums[i + 1];
    nums[i + 1] = nums[high];
    nums[high] = temp;

    return i + 1;
}
```

#### 快速选择（力扣215）

数组中的第K个最大元素，你必须设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

所以只要某次划分的 q 为倒数第 k个下标的时候，我们就已经找到了答案。 我们只关心这一点，至于 a[l⋯q−1]和 a[q+1⋯r]是否是有序的，我们不关心。

因此我们可以改进快速排序算法来解决这个问题：在分解的过程当中，我们会对子数组进行划分，如果划分得到的 q 正好就是我们需要的下标，就直接返回 a[q]；否则，如果 q 比目标下标小，就递归右子区间，否则递归左子区间。这样就可以把原来递归两个区间变成只递归一个区间，提高了时间效率。这就是「快速选择」算法。

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        // 第 k 大的数，下标是 len - k;
        int target = nums.length - k;
        int left = 0;
        int right = nums.length - 1;
        while (true){
            int pivotIndex = randomPartition(nums, left, right);
            if (pivotIndex == target)
                return nums[pivotIndex];
            else if (pivotIndex < target) {
                left = pivotIndex + 1;
            }
            else {
                right = pivotIndex - 1;
            }
        }
    }

    public static int randomPartition(int[] nums, int low, int high) {
        // 生成 low 和 high 之间的随机索引
        int randomIndex = new Random().nextInt(high - low + 1) + low;

        // 将随机索引处的元素与最后一个元素交换，使其成为基准元素
        int temp = nums[randomIndex];
        nums[randomIndex] = nums[high];
        nums[high] = temp;

        return partition(nums, low, high);
    }

    // 划分方法，以基准元素为准将数组划分为两个子数组
    public static int partition(int[] nums, int low, int high) {
        int pivot = nums[high];
        int i = low - 1;

        for (int j = low; j < high; j++) {
            if (nums[j] < pivot) {
                i++;
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
            }
        }

        int temp = nums[i + 1];
        nums[i + 1] = nums[high];
        nums[high] = temp;

        return i + 1;
    }
}
```



### 归并排序

归并排序是一种基于分治思想的排序算法，它的原理和特点如下：

**原理：**

- **分治思想**：归并排序将待排序数组分成两个子数组，分别排序，然后合并已排序的子数组，从而完成整个数组的排序过程。
- **递归实现**：归并排序通过递归地将数组划分为更小的子数组，直到子数组的长度为1，然后再通过合并操作将子数组归并成更大的有序数组，直到最终完成整个数组的排序。
- **合并操作**：合并操作是归并排序的核心步骤，它将两个已排序的子数组合并成一个有序数组。合并操作通过比较两个子数组的元素，依次选择较小的元素放入新数组中，直到两个子数组均被遍历完成。

**特点：**

- **稳定性**：归并排序是一种**稳定**的排序算法，相同元素的相对位置在排序后不会改变。
- **时间复杂度**：归并排序的时间复杂度为**O(nlogn)**，其中n为待排序数组的长度，因此归并排序适用于大规模数据的排序。
- **空间复杂度**：归并排序需要额外的空间来存储合并操作中的临时数组，其空间复杂度为**O(n)**。
- **适用性**：由于归并排序的稳定性和时间复杂度优势，它通常用于对大规模数据进行排序，尤其是外部排序。

**代码：**

```java
// 主函数，用于测试
public static void main(String[] args) {
    int[] nums = {5, 3, 7, 2, 8, 4, 1, 9, 6};
    mergeSort(nums, 0, nums.length - 1);
}

// 归并排序的方法
private static void mergeSort(int[] nums, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(nums, left, mid);
        mergeSort(nums, mid + 1, right);
        merge(nums, left, mid, right);
    }
}

// 合并两个有序数组
private static void merge(int[] nums, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // 创建临时数组存放合并结果
    int[] L = new int[n1];
    int[] R = new int[n2];

    // 将元素复制到临时数组
    for (int i = 0; i < n1; ++i)
        L[i] = nums[left + i];
    for (int j = 0; j < n2; ++j)
        R[j] = nums[mid + 1 + j];

    // 合并两个有序数组
    int i = 0, j = 0;
    int k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            nums[k] = L[i];
            i++;
        } else {
            nums[k] = R[j];
            j++;
        }
        k++;
    }

    // 处理剩余元素
    while (i < n1) {
        nums[k] = L[i];
        i++;
        k++;
    }

    while (j < n2) {
        nums[k] = R[j];
        j++;
        k++;
    }
}
```

### 希尔排序

希尔排序是插入排序的一种改进版本，也称为缩小增量排序。它是由Donald Shell于1959年提出的。

希尔排序的基本思想是将待排序的数组元素按照一定的间隔（称为增量）分组，然后对每组进行插入排序。随着增量逐渐减少，每组包含的元素越来越多，增量最终为1，此时整个数组被分成一组，完成最后一次插入排序。

**希尔排序的特点包括：**

- **改进了插入排序的性能**：希尔排序通过对插入排序的改进，在性能上有了显著提升，尤其是对于大型数组和随机顺序的数组。
- **时间复杂度**：希尔排序的时间复杂度取决于增量序列的选择，但通常在**最好情况下为O(n log n)，最坏情况下为O(n^2)**。
- **不稳定性**：希尔排序是**不稳定**的排序算法，即在排序过程中，相等元素的相对顺序可能会发生变化。

```java
// 主函数，用于测试
public static void main(String[] args) {
    int[] nums = {5, 3, 7, 2, 8, 4, 1, 9, 6};
    shellSort(nums);
}

// 希尔排序方法
public static int[] shellSort(int[] nums) {
    int n = nums.length;

    // 使用希尔增量序列，初始增量为数组长度的一半，逐步缩小增量
    for (int gap = n / 2; gap > 0; gap /= 2) {
        // 对每个子数组进行插入排序
        for (int i = gap; i < n; i++) {
            int temp = nums[i];
            int j = i;
            // 将当前元素插入到正确的位置
            while (j >= gap && nums[j - gap] > temp) {
                nums[j] = nums[j - gap];
                j -= gap;
            }
            nums[j] = temp;
        }
    }
    return nums; // 返回排序后的数组
}
```

### 堆排序

堆排序是一种基于完全二叉树（堆）的排序算法，它利用了堆的性质来进行排序。

**堆排序的基本思想是：**

1. 构建初始堆：将待排序的数组构建成一个大顶堆（或小顶堆）。
2. 交换堆顶元素和末尾元素：将堆顶元素（最大值或最小值）与末尾元素进行交换，然后对剩余元素重新构建堆，以获取剩余元素的最大值（或最小值）。
3. 重复进行第2步，直到整个数组排序完成。

**堆排序的特点包括：**

- 堆排序是一种不稳定的排序算法，因为在构建堆和交换元素的过程中，可能会改变相同元素之间的相对顺序。
- 堆排序的平均时间复杂度为 O(nlogn)，其中 n 是数组的长度。
- 堆排序不需要额外的空间来存储排序结果，因此是原地排序算法。
- 堆排序是一种选择排序，每次选取堆顶元素，因此不像快速排序那样可以利用局部性原理。

**代码：**

```java
// 主函数，用于测试
public static void main(String[] args) {
    int[] nums = {5, 3, 7, 2, 8, 4, 1, 9, 6};
    heapSort(nums);
}

// 堆排序方法
public static void heapSort(int[] nums) {
    int n = nums.length;

    // 构建最大堆
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(nums, n, i);
    }

    // 依次将堆顶元素与末尾元素交换，并重新调整堆
    for (int i = n - 1; i > 0; i--) {
        // 交换堆顶元素与末尾元素
        int temp = nums[0];
        nums[0] = nums[i];
        nums[i] = temp;

        // 调整堆，使其满足最大堆的性质
        heapify(nums, i, 0);
    }
}

// 调整堆的方法
private static void heapify(int[] nums, int n, int i) {
    int largest = i; // 初始化最大元素的索引为根节点
    int left = 2 * i + 1; // 左子节点的索引
    int right = 2 * i + 2; // 右子节点的索引

    // 如果左子节点比根节点大，则更新最大元素的索引
    if (left < n && nums[left] > nums[largest]) {
        largest = left;
    }

    // 如果右子节点比当前最大元素大，则更新最大元素的索引
    if (right < n && nums[right] > nums[largest]) {
        largest = right;
    }

    // 如果最大元素的索引不等于根节点索引，则交换根节点与最大元素
    if (largest != i) {
        int temp = nums[i];
        nums[i] = nums[largest];
        nums[largest] = temp;

        // 递归调整被影响的子树
        heapify(nums, n, largest);
    }
}
```

### 优先队列

- 优先队列采用的是堆排序（不指定Comparator时**默认为最小堆**），头是按指定排序方式的最小元素。堆排序只能保证根是最大（最小），整个堆并不是有序的。队列既可以根据元素的自然顺序来排序，也可以根据 Comparator来设置排序规则。
- 该队列是用**数组**实现，但是数组大小可以动态增加，容量无限。但在创建时可以指定初始大小。当我们向优先队列增加元素的时候，队列大小会自动增加。
- 插入方法（**offer()、poll()、remove() 、add() 方法**）时间复杂度为**O(logn)** ；**remove(Object) 和 contains(Object)** 时间复杂度为**O(n)**；检索方法**（peek、element 和 size）**时间复杂度为常量。

- 方法iterator()中提供的迭代器并不保证以有序的方式遍历优先级队列中的元素。（原因可参考PriorityQueue的内部实现）如果需要按顺序遍历，可用**Arrays.sort(pq.toArray())**。

```java
PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.length, (a, b)->(a.val - b.val));//小顶堆

add(E e) & offer(E e)
//都是向优先队列中插入元素，只是Queue接口规定二者对插入失败时的处理不同，前者在插入失败时抛出异常，后则则会返回false。
element() & peek()
//都是获取但不删除队首元素，也就是队列中权值最小的那个元素，二者唯一的区别是当方法失败时前者抛出异常，后者返回null。
//根据小顶堆的性质，堆顶那个元素就是全局最小的那个；由于堆用数组表示，根据下标关系，0下标处的那个元素既是堆顶元素。所以直接返回数组0下标处的那个元素即可。
remove() & poll()
//都是获取并删除队首元素，区别是当方法失败时前者抛出异常，后者返回null。
remove(Object o)
//方法用于删除队列中跟o相等的某一个元素（如果有多个相等，只删除一个）
//该方法不是Queue接口内的方法，而是Collection接口的方法。由于删除操作会改变队列结构，所以要进行调整；
//又由于删除元素的位置可能是任意的，所以调整过程比其它函数稍加繁琐。
//具体来说，remove(Object o)可以分为2种情况：
//1. 删除的是最后一个元素。直接删除即可，不需要调整。
//2. 删除的不是最后一个元素，从删除点开始以最后一个元素为参照调用一次siftDown()即可。此处不再赘述。
```

#### 数组中的第K个最大元素(力扣215)

优先队列的思路是很朴素的。由于找第 K 大元素，其实就是整个数组排序以后后半部分最小的那个元素。因此，我们可以维护一个有 K 个元素的最小堆：

- 如果当前堆不满，直接添加；

- 堆满的时候，如果新读到的数小于等于堆顶，肯定不是我们要找的元素，只有新遍历到的数大于堆顶的时候，才将堆顶拿出，然后放入新读到的数，进而让堆自己去调整内部结构。

说明：这里最合适的操作其实是 replace()，即直接把新读进来的元素放在堆顶，然后执行下沉siftDown()操作。Java 当中的 PriorityQueue 没有提供这个操作，只好先 poll() 再 offer()。

```java
import java.util.Comparator;
import java.util.PriorityQueue;

public class Solution {

    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        // 使用一个含有 k 个元素的最小堆，PriorityQueue 底层是动态数组，为了防止数组扩容产生消耗，可以先指定数组的长度
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(k, Comparator.comparingInt(a -> a));
        // Java 里没有 heapify ，因此我们逐个将前 k 个元素添加到 minHeap 里
        for (int i = 0; i < k; i++) {
            minHeap.offer(nums[i]);
        }

        for (int i = k; i < len; i++) {
            // 看一眼，不拿出，因为有可能没有必要替换
            Integer topElement = minHeap.peek();
            // 只要当前遍历的元素比堆顶元素大，堆顶弹出，遍历的元素进去
            if (nums[i] > topElement) {
                // Java 没有 replace()，所以得先 poll() 出来，然后再放回去
                minHeap.poll();
                minHeap.offer(nums[i]);
            }
        }
        return minHeap.peek();
    }
}
```



## 区间和问题解决方案

针对不同的题目，我们有不同的方案可以选择（假设我们有一个数组）：

1. 数组不变，求区间和：「前缀和」、「树状数组」、「线段树」

2. 多次修改某个数（单点），求区间和：「树状数组」、「线段树」
3. 多次修改某个区间，输出最终结果：「差分」
4. 多次修改某个区间，求区间和：「线段树」、「树状数组」（看修改区间范围大小）
5. 多次将某个区间变成同一个数，求区间和：「线段树」、「树状数组」（看修改区间范围大小）

这样看来，「线段树」能解决的问题是最多的，那我们是不是无论什么情况都写「线段树」呢？答案并不是，而且恰好相反，只有在我们遇到第 4 类问题，不得不写「线段树」的时候，我们才考虑线段树。因为「线段树」代码很长，而且常数很大，实际表现不算很好。我们只有在不得不用的时候才考虑「线段树」。

总结一下，我们应该按这样的优先级进行考虑：

- 简单求区间和，用「前缀和」

- 多次将某个区间变成同一个数，用「线段树」
- 其他情况，用「树状数组」

### 前缀和

前缀和技巧适用于快速、频繁地计算一个索引区间内的元素之和。

**一维**

```java
class NumArray {
    private int[] preSum;
    public NumArray(int[] nums) {
        preSum = new int[nums.length + 1];
        preSum[0] = 0;
        // 计算 nums 的累加和
        for (int i = 1; i < preSum.length; i++) {
            preSum[i] = preSum[i - 1] + nums[i - 1];
        }
    }
    public int sumRange(int left, int right) {
        return preSum[right + 1] - preSum[left];
    }
}
```

**二维**

```java
class NumMatrix {
    // 定义：preSum[i][j] 记录 matrix 中子矩阵 [0, 0, i-1, j-1] 的元素和
    private int[][] preSum;
    
    public NumMatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        if (m == 0 || n == 0) return;
        // 构造前缀和矩阵
        preSum = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                // 计算每个矩阵 [0, 0, i, j] 的元素和
                preSum[i][j] = preSum[i-1][j] + preSum[i][j-1] + matrix[i - 1][j - 1] - preSum[i-1][j-1];
            }
        }
    }
    
    // 计算子矩阵 [x1, y1, x2, y2] 的元素和
    public int sumRegion(int x1, int y1, int x2, int y2) {
        // 目标矩阵之和由四个相邻矩阵运算获得
        return preSum[x2+1][y2+1] - preSum[x1][y2+1] - preSum[x2+1][y1] + preSum[x1][y1];
    }
}
```

### 差分数组

差分数组的主要适用场景是频繁对原始数组的某个区间的元素进行增减。

```java
// 差分数组工具类
class Difference {
    // 差分数组
    private int[] diff;
    
    /* 输入一个初始数组，区间操作将在这个数组上进行 */
    public Difference(int[] nums) {
        assert nums.length > 0;
        diff = new int[nums.length];
        // 根据初始数组构造差分数组
        diff[0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            diff[i] = nums[i] - nums[i - 1];
        }
    }

    /* 给闭区间 [i, j] 增加 val（可以是负数）*/
    public void increment(int i, int j, int val) {
        diff[i] += val;
        if (j + 1 < diff.length) {
            diff[j + 1] -= val;
        }
    }

    /* 返回结果数组 */
    public int[] result() {
        int[] res = new int[diff.length];
        // 根据差分数组构造结果数组
        res[0] = diff[0];
        for (int i = 1; i < diff.length; i++) {
            res[i] = res[i - 1] + diff[i];
        }
        return res;
    }
}
```

## 哈希

### [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

> 把输入中是异位词的单词放到一个list中，这些list再放到一个list中
>
> **示例 1:**
>
> ```
> 输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
> 输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
> ```

**方法一：排序**

由于互为字母异位词的两个字符串包含的字母相同，因此对两个字符串分别进行排序之后得到的字符串一定是相同的，故可以将排序之后的字符串作为哈希表的键。

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String str : strs) {
            char[] array = str.toCharArray();
            Arrays.sort(array);
            String key = new String(array);
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
}
```

**方法二：数据编码**

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // 编码到分组的映射
        HashMap<String, List<String>> codeToGroup = new HashMap<>();
        for (String s : strs) {
            // 对字符串进行编码
            String code = encode(s);
            // 把编码相同的字符串放在一起
            codeToGroup.putIfAbsent(code, new LinkedList<>());
            codeToGroup.get(code).add(s);
        }

        // 获取结果
        List<List<String>> res = new LinkedList<>();
        for (List<String> group : codeToGroup.values()) {
            res.add(group);
        }

        return res;
    }

    // 利用每个字符的出现次数进行编码
    String encode(String s) {
        char[] count = new char[26];
        for (char c : s.toCharArray()) {
            int delta = c - 'a';
            count[delta]++;
        }
        return new String(count);
    }
}
```

### [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

> 给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。
>
> **示例 1：**
>
> ```
> 输入：nums = [100,4,200,1,3,2]
> 输出：4
> 解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [0,3,7,2,5,8,4,6,0,1]
> 输出：9
> ```

我们可以用空间换时间的思路，把数组元素放到哈希集合里面**去重**，然后去寻找连续序列的第一个元素，即可在 `O(N)` 时间找到答案。

比方说 `nums = [8,4,9,1,3,2]`，我们先找到 1，然后递增，找到了 2, 3, 4，这就是一个长度为 4 的序列。又找到 8，网上递增执照到了 9，这是一个长度为 2 的序列。虽然 for 循环嵌套 while 循环，但是每个元素只会被遍历到最多两次，所以均摊时间复杂度依然为 `O(N)`

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        // 转化成哈希集合，方便快速查找是否存在某个元素
        HashSet<Integer> set = new HashSet<Integer>();
        for (int num : nums) {
            set.add(num);
        }

        int res = 0;

        for (int num : set) {
            if (set.contains(num - 1)) {
                // num 不是连续子序列的第一个，跳过
                continue;
            }
            // num 是连续子序列的第一个，开始向上计算连续子序列的长度
            int curNum = num;
            int curLen = 1;

            while (set.contains(curNum + 1)) {
                curNum += 1;
                curLen += 1;
            }
            // 更新最长连续序列的长度
            res = Math.max(res, curLen);
        }

        return res;
    }
}
```



## 链表

### 合并两个有序链表（力扣21）

**当你需要创造一条新链表的时候，可以使用虚拟头结点简化边界情况的处理**。

```java
ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    // 虚拟头结点
    ListNode dummy = new ListNode(-1), p = dummy;
    //代码中还用到一个链表的算法题中是很常见的「虚拟头结点」技巧，也就是 dummy 节点。
    //你可以试试，如果不使用 dummy 虚拟节点，代码会复杂一些，需要额外处理指针 p 为空的情况。
    //而有了 dummy 节点这个占位符，可以避免处理空指针的情况，降低代码的复杂性。    
    
    ListNode p1 = l1, p2 = l2;
    
    while (p1 != null && p2 != null) {
        // 比较 p1 和 p2 两个指针
        // 将值较小的的节点接到 p 指针
        if (p1.val > p2.val) {
            p.next = p2;
            p2 = p2.next;
        } else {
            p.next = p1;
            p1 = p1.next;
        }
        // p 指针不断前进
        p = p.next;
    }
    
    if (p1 != null) {
        p.next = p1;
    }
    
    if (p2 != null) {
        p.next = p2;
    }
    
    return dummy.next;
}
```

### 单链表的分解（力扣86）

```java
ListNode partition(ListNode head, int x) {
    // 存放小于 x 的链表的虚拟头结点
    ListNode dummy1 = new ListNode(-1);
    // 存放大于等于 x 的链表的虚拟头结点
    ListNode dummy2 = new ListNode(-1);
    // p1, p2 指针负责生成结果链表
    ListNode p1 = dummy1, p2 = dummy2;
    // p 负责遍历原链表，类似合并两个有序链表的逻辑
    // 这里是将一个链表分解成两个链表
    ListNode p = head;
    while (p != null) {
        if (p.val >= x) {
            p2.next = new ListNode(p.val);
            p2 = p2.next;
        } else {
           p1.next = new ListNode(p.val);
            p1 = p1.next;
        }
        // 直接让 p 指针前进
        p = p.next;
    }
    // 连接两个链表
    p1.next = dummy2.next;

    return dummy1.next;
}

```

### 合并 k 个有序链表（力扣23）

```java
ListNode mergeKLists(ListNode[] lists) {
    if (lists.length == 0) return null;
    // 虚拟头结点
    ListNode dummy = new ListNode(-1);
    ListNode p = dummy;
    // 优先级队列，最小堆
    PriorityQueue<ListNode> pq = new PriorityQueue<>(
        lists.length, (a, b)->(a.val - b.val));
    // 将 k 个链表的头结点加入最小堆
    for (ListNode head : lists) {
        if (head != null)
            pq.add(head);
    }

    while (!pq.isEmpty()) {
        // 获取最小节点，接到结果链表中
        ListNode node = pq.poll();
        p.next = node;
        if (node.next != null) {
            pq.add(node.next);
        }
        // p 指针不断前进
        p = p.next;
    }
    return dummy.next;
}
```

### 单链表的倒数第 k 个节点(力扣19)

```java'
public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode p1 = head;
        ListNode p2 = head;
        ListNode p = head;
        int count = 0;

        while (p != null){
            p = p.next;
            count++;
        }
        if (count == 1 && n == 1)
            return null;
        if (count != 1 && count == n)
            return head.next;
        for (int i = 0; i < n+1; i++) {
            p2 = p2.next;
        }
        while (p2 != null){
            p2 = p2.next;
            p1 = p1.next;
        }
        if (p1.next != null){
            p1.next = p1.next.next;
        }
        else
            p1.next = null;

        return head;
    }
```

### 单链表的中点

```java
//我的方法
public ListNode middleNode(ListNode head) {
    ListNode p = head;
    ListNode q = head;
    int count = 0;
    while (p != null){
        count++;
        p = p.next;
    }
    for (int i = 0; i < count / 2 ; i++) {
        q = q.next;
    }
    return q;
}
//快慢指针
ListNode middleNode(ListNode head) {
    // 快慢指针初始化指向 head
    ListNode slow = head, fast = head;
    // 快指针走到末尾时停止
    while (fast != null && fast.next != null) {
        // 慢指针走一步，快指针走两步
        slow = slow.next;
        fast = fast.next.next;
    }
    // 慢指针指向中点
    return slow;
}
```

### 判断链表是否包含环（力扣142）

解决方案也是用快慢指针：每当慢指针 `slow` 前进一步，快指针 `fast` 就前进两步。如果 `fast` 最终遇到空指针，说明链表中没有环；如果 `fast` 最终和 `slow` 相遇，那肯定是 `fast` 超过了 `slow` 一圈，说明链表中含有环。

```java
boolean hasCycle(ListNode head) {
    // 快慢指针初始化指向 head
    ListNode slow = head, fast = head;
    // 快指针走到末尾时停止
    while (fast != null && fast.next != null) {
        // 慢指针走一步，快指针走两步
        slow = slow.next;
        fast = fast.next.next;
        // 快慢指针相遇，说明含有环
        if (slow == fast) {
            return true;
        }
    }
    // 不包含环
    return false;
}
```

**力扣142环形链表 II**

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

```java
class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast, slow;
        fast = slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) break;
        }
        // 上面的代码类似 hasCycle 函数
        if (fast == null || fast.next == null) {
            // fast 遇到空指针说明没有环
            return null;
        }

        // 重新指向头结点
        slow = head;
        // 快慢指针同步前进，相交点就是环起点
        while (slow != fast) {
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

当快慢指针相遇时，让其中任一个指针指向头节点，然后让它俩以相同速度前进，再次相遇时所在的节点位置就是环开始的位置。

我们假设快慢指针相遇时，慢指针 `slow` 走了 `k` 步，那么快指针 `fast` 一定走了 `2k` 步：

<img src = "Algorithmimages\1.jpg">

`fast` 一定比 `slow` 多走了 `k` 步，这多走的 `k` 步其实就是 `fast` 指针在环里转圈圈，所以 `k` 的值就是环长度的「整数倍」。

假设相遇点距环的起点的距离为 `m`，那么结合上图的 `slow` 指针，环的起点距头结点 `head` 的距离为 `k - m`，也就是说如果从 `head` 前进 `k - m` 步就能到达环起点。

巧的是，如果从相遇点继续前进 `k - m` 步，也恰好到达环起点。因为结合上图的 `fast` 指针，从相遇点开始走k步可以转回到相遇点，那走 `k - m` 步肯定就走到环起点了：

<img src = "Algorithmimages\2.jpg">

所以，只要我们把快慢指针中的任一个重新指向 `head`，然后两个指针同速前进，`k - m` 步后一定会相遇，相遇之处就是环的起点了。

###  两个链表是否相交（力扣160）

解决这个问题的关键是，**通过某些方式，让 `p1` 和 `p2` 能够同时到达相交节点 `c1`。**

- 我们可以让 `p1` 遍历完链表 `A` 之后开始遍历链表 `B`，让 `p2` 遍历完链表 `B` 之后开始遍历链表 `A`，这样相当于「逻辑上」两条链表接在了一起。如果这样进行拼接，就可以让 `p1` 和 `p2` 同时进入公共部分，也就是同时到达相交节点 `c1`：

<img src = "Algorithmimages\3.jpg">

按照这个思路，可以写出如下代码：

```java
ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    // p1 指向 A 链表头结点，p2 指向 B 链表头结点
    ListNode p1 = headA, p2 = headB;
    while (p1 != p2) {
        // p1 走一步，如果走到 A 链表末尾，转到 B 链表
        if (p1 == null) p1 = headB;
        else            p1 = p1.next;
        // p2 走一步，如果走到 B 链表末尾，转到 A 链表
        if (p2 == null) p2 = headA;
        else            p2 = p2.next;
    }
    return p1;
}
```

- 可以通过预先计算两条链表的长度来做到这一点

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    int lenA = 0, lenB = 0;
    // 计算两条链表的长度
    for (ListNode p1 = headA; p1 != null; p1 = p1.next) {
        lenA++;
    }
    for (ListNode p2 = headB; p2 != null; p2 = p2.next) {
        lenB++;
    }
    // 让 p1 和 p2 到达尾部的距离相同
    ListNode p1 = headA, p2 = headB;
    if (lenA > lenB) {
        for (int i = 0; i < lenA - lenB; i++) {
            p1 = p1.next;
        }
    } else {
        for (int i = 0; i < lenB - lenA; i++) {
            p2 = p2.next;
        }
    }
    // 看两个指针是否会相同，p1 == p2 时有两种情况：
    // 1、要么是两条链表不相交，他俩同时走到尾部空指针
    // 2、要么是两条链表相交，他俩走到两条链表的相交点
    while (p1 != p2) {
        p1 = p1.next;
        p2 = p2.next;
    }
    return p1;
}
```

### 删除排序链表中的重复元素（力扣 83）

```java
//快慢指针
//Java/Python 这类带有垃圾回收的语言，帮我们自动找到并回收这些「悬空」的链表节点的内存，
//C++ 这类语言没有自动垃圾回收的机制，确实需要我们编写代码时手动释放掉这些节点的内存。
public ListNode deleteDuplicates(ListNode head) {
    if (head == null)
        return head;
    ListNode slow = head, fast =head.next;
    while (fast != null){
        if (fast.val != slow.val){
            slow.next = fast;
            slow = slow.next;
        }
        fast = fast.next;
    }
    slow.next = null;
    return head;
}
```

### 迭代、递归反转链表

```java
//迭代
//反转整个链表
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null)
        return head;
    ListNode dummy = new ListNode();
    ListNode p = head;
    while (p != null){
        ListNode q = p;
        p = p.next;//这句要在下一句之前
        q.next = dummy.next;
        dummy.next = q;

    }
    return dummy.next;
}
//迭代反转部分链表
public ListNode reverseBetween(ListNode head, int left, int right) {
    if (head == null || head.next == null)
        return head;
    ListNode dummy = new ListNode(0,head);
    ListNode pre = dummy;
    for(int i=1; i<left; i++) {
        pre = pre.next;
    }
    ListNode cur = pre.next;
    for(int i=0; i<right-left; i++) {
        ListNode next = cur.next;
        cur.next = next.next;
        next.next = pre.next;
        pre.next = next;
    }
    return dummy.next;
}
```

```java
//递归
// 定义：输入一个单链表头结点，将该链表反转，返回新的头结点
ListNode reverse(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode last = reverse(head.next);
    head.next.next = head;
    head.next = null;
    return last;
}

//反转链表前 N 个节点
ListNode successor = null; // 后驱节点
// 反转以 head 为起点的 n 个节点，返回新的头结点
ListNode reverseN(ListNode head, int n) {
    if (n == 1) {
        // 记录第 n + 1 个节点
        successor = head.next;
        return head;
    }
    // 以 head.next 为起点，需要反转前 n - 1 个节点
    ListNode last = reverseN(head.next, n - 1);

    head.next.next = head;
    // 让反转之后的 head 节点和后面的节点连起来
    head.next = successor;
    return last;
}
//反转链表的一部分
ListNode reverseBetween(ListNode head, int m, int n) {
    // base case
    if (m == 1) {
        return reverseN(head, n);
    }
    // 前进到反转的起点触发 base case
    head.next = reverseBetween(head.next, m - 1, n - 1);
    return head;
}

```



<img src = "Algorithmimages\4.jpg">

<img src = "Algorithmimages\5.jpg">

### 判断回文链表

```java
//利用反转链表
class Solution {
    public static ListNode reverseList(ListNode head) {
        if (head == null || head.next == null)
            return head;
        ListNode dummy = new ListNode();
        ListNode p = head;
        while (p != null){
            ListNode q = new ListNode(p.val);
            p = p.next;//这句要在下一句之前
            q.next = dummy.next;
            dummy.next = q;

        }
        return dummy.next;
    }
    public static boolean isPalindrome(ListNode head) {
        ListNode p = head;
        ListNode dummy = head;
        ListNode reverse = reverseList(dummy);
        ListNode q = reverse;
        while (p != null){
            if (p.val != q.val)
                return false;
            p = p.next;
            q = q.next;
        }
        return true;
    }
}

//将值复制到数组中后用双指针法
class Solution {
    public boolean isPalindrome(ListNode head) {
        List<Integer> vals = new ArrayList<Integer>();

        // 将链表的值复制到数组中
        ListNode currentNode = head;
        while (currentNode != null) {
            vals.add(currentNode.val);
            currentNode = currentNode.next;
        }

        // 使用双指针判断是否回文
        int front = 0;
        int back = vals.size() - 1;
        while (front < back) {
            if (!vals.get(front).equals(vals.get(back))) {
                return false;
            }
            front++;
            back--;
        }
        return true;
    }
}
```



## 数组

###  删除有序数组中的重复项（力扣26）

```java
//高效解决这道题就要用到快慢指针技巧：
//我们让慢指针 slow 走在后面，快指针 fast 走在前面探路，
//找到一个不重复的元素就赋值给 slow 并让 slow 前进一步。
//这样，就保证了 nums[0..slow] 都是无重复的元素，
//当 fast 指针遍历完整个数组 nums 后，nums[0..slow] 就是整个数组去重之后的结果。

int removeDuplicates(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }
    int slow = 0, fast = 0;
    while (fast < nums.length) {
        if (nums[fast] != nums[slow]) {
            slow++;
            // 维护 nums[0..slow] 无重复
            nums[slow] = nums[fast];
        }
        fast++;
    }
    // 数组长度为索引 + 1
    return slow + 1;
}



//
public static int removeDuplicates(int[] nums) {
    int res = nums.length;
    int newLength = nums.length;
    for (int i = 0; i < newLength-1; i++) {
        int conut = i + 1;
        while (nums[i] == nums[conut]) {
            if(conut == newLength-1)
                break;
            else
                conut++;
        }
        int conut2 = conut;
        if(conut2 == newLength-1 && nums[newLength-1] == nums[i])
            conut2++;
        for (int j = i + 1; j < newLength - (conut2 - i - 1); j++) {
            nums[j] = nums[conut++];
        }
        newLength = newLength - (conut2 - i - 1);
        res = res - (conut2 - i - 1);
    }
    return res;
}
```

### 移除元素（力扣27）

给你一个数组 `nums` 和一个值 `val`，你需要原地移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。不要使用额外的数组空间，你必须仅使用 `O(1)` 额外空间并原地修改输入数组。

```java
public static int removeElement(int[] nums, int val) {
    int slow = 0, fast = 0;
    while (fast < nums.length){
        if(nums[fast] != val){
            nums[slow] = nums[fast];
            slow++;
        }
        fast++;
    }
    return slow;
}
```

### 移动零（力扣283）

> 给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。
>
> **请注意** ，必须在不复制数组的情况下原地对数组进行操作。
>
> **示例 1:**
>
> ```
> 输入: nums = [0,1,0,3,12]
> 输出: [1,3,12,0,0]
> ```
>
> **示例 2:**
>
> ```
> 输入: nums = [0]
> 输出: [0]
> ```

一个快指针一个慢指针，快指针遇到不为0的数则存到慢指针的位置，然后慢指针加1，快指针遍历到最后的时候，再把慢指针以后的所有元素置为0

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int slow = 0, fast = 0;
        while (fast < nums.length) {
            if (nums[fast] != 0) {
                nums[slow] = nums[fast];
                slow++;
            }
            fast++;
        }
        //Arrays.fill(Type[] array, int startIndex, int endIndex, Type value),startIndex和endIndex左闭右开
        Arrays.fill(nums,slow,nums.length,0);        
    }
}
```



### 两数之和 II 输入有序数组（力扣167）

只要**数组有序，就应该想到双指针技巧。**这道题的解法有点类似二分查找，通过调节 `left` 和 `right` 就可以调整 `sum` 的大小：

```java
int[] twoSum(int[] nums, int target) {
    // 一左一右两个指针相向而行
    int left = 0, right = nums.length - 1;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == target) {
            // 题目要求的索引是从 1 开始的
            return new int[]{left + 1, right + 1};
        } else if (sum < target) {
            left++; // 让 sum 大一点
        } else if (sum > target) {
            right--; // 让 sum 小一点
        }
    }
    return new int[]{-1, -1};
}
```

### 最长回文子串（力扣5）

给你一个字符串 `s`，找到 `s` 中最长的回文子串。如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。

```java
public String palindrome(String s, int l, int r) {
    // 防止索引越界
    while (l >= 0 && r < s.length()
           && s.charAt(l) == s.charAt(r)) {
        // 双指针，向两边展开
        l--; r++;
    }
    // 返回以 s[l] 和 s[r] 为中心的最长回文串
    return s.substring(l + 1, r);
}
public String longestPalindrome(String s) {
    String res = "";
    for (int i = 0; i < s.length(); i++) {
        // 以 s[i] 为中心的最长回文子串
        String s1 = palindrome(s, i, i);
        // 以 s[i] 和 s[i+1] 为中心的最长回文子串
        String s2 = palindrome(s, i, i + 1);
        // res = longest(res, s1, s2)
        res = res.length() > s1.length() ? res : s1;
        res = res.length() > s2.length() ? res : s2;
    }
    return res;
}
```

### 反转字符串中的单词（力扣151）

以空格为分割符完成字符串分割后，若两单词间有 x>1x > 1x>1 个空格，则在单词列表 strsstrsstrs 中，此两单词间会多出 x−1x - 1x−1 个 “空单词” （即 "" ）。解决方法：倒序遍历单词列表，并将单词逐个添加至 StringBuilder ，遇到空单词时跳过。

<img src = "Algorithmimages\9.PNG">

```java
class Solution {
    public String reverseWords(String s) {
        String[] strs = s.trim().split(" ");
        StringBuilder sb = new StringBuilder();
        for (int i = strs.length-1; i >=0 ; i--) {
            if (strs[i].equals(""))
                continue;
            sb.append(strs[i] + " ");
        }
        return sb.toString().trim();
    }
}
```

### 田忌赛马（优势洗牌力扣870）

对于任意一个 t=nums2[i] t = nums2[i] t=nums2[i] 而言，我们应当在候选集合中选择比其大的最小数，若不存在这样的数字，则选择候选集合中的最小值。同时，由于 nums1相同数会存在多个，我们还要对某个具体数字的可用次数进行记录。

也就是我们总共涉及两类操作：

1. 实时维护一个候选集合，该集合支持高效查询比某个数大的数值操作；

2. 对候选集合中每个数值的可使用次数进行记录，当使用到了候选集合中的某个数后，要对其进行计数减一操作，若计数为 000，则将该数值从候选集合中移除。

计数操作容易想到哈希表，而实时维护候选集合并高效查询可以使用基于红黑树的 TreeSet 数据结构。

```java
class Solution {
    public int[] advantageCount(int[] nums1, int[] nums2) {
        int n = nums1.length;
        TreeSet<Integer> tset = new TreeSet<>();
        Map<Integer, Integer> map = new HashMap<>();
        for (int x : nums1) {
            map.put(x, map.getOrDefault(x, 0) + 1);
            if (map.get(x) == 1) tset.add(x);
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; i++) {
            Integer cur = tset.ceiling(nums2[i] + 1);
            //ceiling()方法用于返回大于或等于给定元素的该集合中的最小元素；如果没有此类元素，则返回null。
            if (cur == null) cur = tset.ceiling(-1);
            ans[i] = cur;
            map.put(cur, map.get(cur) - 1);
            if (map.get(cur) == 0) tset.remove(cur);
        }
        return ans;
    }
}
```

### O(1)删除查找数组中的任意元素（力扣380）

对于 `getRandom` 方法，如果想「等概率」且「在 O(1) 的时间」取出元素，一定要满足：**底层用数组实现，且数组必须是紧凑的**。

这样我们就可以直接生成随机数作为索引，从数组中取出该随机索引对应的元素，作为随机元素。

**但如果用数组存储元素的话，插入，删除的时间复杂度怎么可能是 O(1) 呢**？

可以做到！对数组尾部进行插入和删除操作不会涉及数据搬移，时间复杂度是 O(1)。

**所以，如果我们想在 O(1) 的时间删除数组中的某一个元素 `val`，可以先把这个元素交换到数组的尾部，然后再 `pop` 掉**。

交换两个元素必须通过索引进行交换对吧，那么我们需要一个哈希表 `valToIndex` 来记录每个元素值对应的索引。

```java
class RandomizedSet {
    // 存储元素的值
    List<Integer> nums;
    // 记录每个元素对应在 nums 中的索引
    Map<Integer, Integer> valToIndex;

    public RandomizedSet() {
        nums = new ArrayList<>();
        valToIndex = new HashMap<>();
    }

    public boolean insert(int val) {
        // 若 val 已存在，不用再插入
        if (valToIndex.containsKey(val)) {
            return false;
        }
        // 若 val 不存在，插入到 nums 尾部，
        // 并记录 val 对应的索引值
        valToIndex.put(val, nums.size());
        nums.add(val);
        return true;
    }

    public boolean remove(int val) {
        // 若 val 不存在，不用再删除
        if (!valToIndex.containsKey(val)) {
            return false;
        }
        // 先拿到 val 的索引
        int index = valToIndex.get(val);
        // 将最后一个元素对应的索引修改为 index
        valToIndex.put(nums.get(nums.size() - 1), index);
        // 交换 val 和最后一个元素
        Collections.swap(nums, index, nums.size() - 1);
        // 在数组中删除元素 val
        nums.remove(nums.size() - 1);
        // 删除元素 val 对应的索引
        valToIndex.remove(val);
        return true;
    }

    public int getRandom() {
        // 随机获取 nums 中的一个元素
        return nums.get((int) (Math.random() * nums.size()));
    }
}
```

### [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

> 给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。
>
> 找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。
>
> 返回容器可以储存的最大水量。
>
> **示例 1：**
>
> ![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)
>
> ```
> 输入：[1,8,6,2,5,4,8,3,7]
> 输出：49 
> 解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
> ```

用 left 和 right 两个指针从两端向中心收缩，一边收缩一边计算 [left, right] 之间的矩形面积，取最大的面积值即是答案。

矩形的高度是由 min(height[left], height[right]) 即较低的一边决定的，你**如果移动较低的那一边，那条边可能会变高，使得矩形的高度变大，进而就「有可能」使得矩形的面积变大**；相反，如果你去移动较高的那一边，矩形的高度是无论如何都不会变大的，所以不可能使矩形的面积变得更大。

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0, right = height.length - 1;
        int res = 0;
        while (left < right) {
            // [left, right] 之间的矩形面积
            int cur_area = Math.min(height[left], height[right]) * (right - left);
            res = Math.max(res, cur_area);
            // 双指针技巧，移动较低的一边
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return res;
    }
}
```

### [15. 三数之和](https://leetcode.cn/problems/3sum/)

> 给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。
>
> **注意：**答案中不可以包含重复的三元组。
>
> **示例 1：**
>
> ```
> 输入：nums = [-1,0,1,2,-1,-4]
> 输出：[[-1,-1,2],[-1,0,1]]
> 解释：
> nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
> nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
> nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
> 不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
> 注意，输出的顺序和三元组的顺序并不重要。
> ```

1. 首先对数组进行排序，然后固定一个数 nums[i]
2. 如果 nums[i]大于 0，则三数之和必然无法等于 0，结束循环
3. 如果 nums[i] == nums[i−1]，则说明该数字重复，会导致结果重复，所以应该跳过
4. 使用左右指针指向 nums[i]后面的两端，分别为 nums[left] 和 nums[right]
5. 计算三个数的和 sum 判断是否满足为 0，满足则添加进结果集
6. 当 sum == 0 时，nums[left] == nums[left+1] 则会导致结果重复，应该跳过，L++
7. 当 sum == 0 时，nums[right] == nums[right-1] 则会导致结果重复，应该跳过，R--
8. 如果sum < 0, 则说明结果偏小，应该增大nums[left]，即left+1 
9. 如果sum > 0, 则说明结果偏大，应该减小nums[right]，即right-1

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        //0.判断边界情况
        if(nums == null || nums.length < 3) {
            return res;
        }
        //1.首先对数组进行排序
        Arrays.sort(nums);
        //2.固定一个数 nums[i]
        for (int i = 0; i < nums.length; i++) {
            //3.如果 nums[i]大于 0，则三数之和必然无法等于 0，结束循环
            if (nums[i] > 0) {
                break;
            }
            //4.如果 nums[i] == nums[i−1]，则说明该数字重复，会导致结果重复，所以应该跳过
            if(i > 0 && nums[i] == nums[i-1]){
                continue;
            }
            //5.使用左右指针指向 nums[i]后面的两端，分别为 nums[left] 和 nums[right]
            int left = i + 1;
            int right = nums.length - 1;
            while (left < right) {
                //6.计算三个数的和 sum 判断是否满足为 0，满足则添加进结果集
                int sum = nums[i] + nums[left] + nums[right];
                if (sum == 0) {
                    res.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    //7.当 sum == 0 时，nums[left] == nums[left+1] 则会导致结果重复，应该跳过，L++
                    //必须加上 left < right 条件，否则数组越界
                    while (left < right && nums[left] == nums[left+1]) {
                        left++;
                    }
                    //8.当 sum == 0 时，nums[right] == nums[right-1] 则会导致结果重复，应该跳过，R--
                    while (left < right && nums[right] == nums[right-1]) {
                        right--;
                    }
                    //再跳过两个结果进入下一个循环
                    left++;
                    right--;
                }
                //9.如果sum < 0, 则说明结果偏小，应该增大nums[left]，即left+1
                else if (sum < 0){
                    left++;
                }
                //9.如果sum > 0, 则说明结果偏大，应该减小nums[right]，即right-1
                else if (sum > 0) {
                    right--;
                }               
            }
        }
        return res;
    }
}
```

### [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

> 给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
>
> **示例 1：**
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)
>
> ```
> 输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
> 输出：6
> 解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
> ```

解题思路：

1. 设置左右指针分别指向数组最左边和最右边，初始化左右最高柱子高度
2. 计算left左边的最高柱子的高度leftMax计算right右边的最高柱子的高度rightMax
3. **left位置能接雨水的量取决于left左边最高柱子的高度, right位置能接雨水的量取决于right左边最高柱子的高度**，如果leftMax < rightMax, 则left 位置能接雨水的量为leftMax - height[left]，如果leftMax >= rightMax, 则right 位置能接雨水的量为rightMax - height[right]

双指针解法中，`leftMax` 和 `rightMax` 代表的是 `height[0..left]` 和 `height[right..end]` 的最高柱子高度。

<img src="D:\BaiduSyncdisk\算法\Hot100images\hfgh4gdjhg.JPG" style="zoom:50%;" />

此时的`leftMax` 是 `left` 指针左边的最高柱子，但是 `rightMax` 并不一定是 `left` 指针右边最高的柱子，这真的可以得到正确答案吗？

其实这个问题要这么思考，我们只在乎 `min(leftMax,rightMax)`。**对于上图的情况，我们已经知道 `leftMax < rightMax` 了，至于这个 `rightMax` 是不是右边最大的，不重要。重要的是 `height[i]` 能够装的水只和较低的 `leftMax` 之差有关**：

<img src="D:\BaiduSyncdisk\算法\Hot100images\56n456b8u46.JPG" style="zoom:50%;" />

```java
class Solution {
    public int trap(int[] height) {
        int res = 0;
        //1.设置左右指针分别指向数组最左边和最右边
        int left = 0, right = height.length - 1;
        //2.初始化左右最高柱子高度
        int leftMax = 0, rightMax = 0;
        //3.开始遍历
        while (left < right) {
            //4.计算left左边的最高柱子的高度leftMax
            leftMax = Math.max(leftMax, height[left]);
            //5.计算right右边的最高柱子的高度rightMax
            rightMax = Math.max(rightMax, height[right]);
            //6.left位置能接雨水的量取决于left左边最高柱子的高度,right位置能接雨水的量取决于right左边最高柱子的高度

            //7.如果leftMax < rightMax, 则left 位置能接雨水的量为leftMax - height[left],加入结果指针继续前进
            if (leftMax < rightMax) {
                res += leftMax - height[left];
                left++;
            }
            //8.如果leftMax >= rightMax, 则right 位置能接雨水的量为rightMax - height[right],加入结果指针继续前进
            else{
                res += rightMax - height[right];
                right--;
            }
        }
        return res;
    }
}
```

## 





## 二维数组

### 顺/逆时针旋转矩阵

我们可以先将 `n x n` 矩阵 `matrix` 按照左上到右下的对角线进行镜像对称：

<img src="Algorithmimages\7.jpg" style="zoom:50%;" />

然后再对矩阵的每一行进行反转：

<img src="Algorithmimages\8.jpg" style="zoom:50%;" />

顺时针旋转90度：

```java
class Solution {
    public void reverse(int[] arr) {
        int i = 0, j = arr.length - 1;
        while (j > i) {
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
            i++;
            j--;
        }
    }
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        for (int[] row : matrix) {
            reverse(row);
        }
    }
}
```

逆时针旋转90度：

```java
// 将二维矩阵原地逆时针旋转 90 度
void rotate2(int[][] matrix) {
    int n = matrix.length;
    // 沿左下到右上的对角线镜像对称二维矩阵
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n - i; j++) {
            // swap(matrix[i][j], matrix[n-j-1][n-i-1])
            int temp = matrix[i][j];
            matrix[i][j] = matrix[n - j - 1][n - i - 1];
            matrix[n - j - 1][n - i - 1] = temp;
        }
    }
    // 然后反转二维矩阵的每一行
    for (int[] row : matrix) {
        reverse(row);
    }
}

void reverse(int[] arr) { /* 见上文 */}
```

### 矩阵的螺旋遍历

核心思路是按照右、下、左、上的顺序遍历数组，并使用四个变量圈定未遍历元素的边界

<img src="Algorithmimages\10.png" style="zoom:50%;" />

随着螺旋遍历，相应的边界会收缩，直到螺旋遍历完整个数组

```java
List<Integer> spiralOrder(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int upper_bound = 0, lower_bound = m - 1;
    int left_bound = 0, right_bound = n - 1;
    List<Integer> res = new LinkedList<>();
    // res.size() == m * n 则遍历完整个数组
    while (res.size() < m * n) {
        if (upper_bound <= lower_bound) {
            // 在顶部从左向右遍历
            for (int j = left_bound; j <= right_bound; j++) {
                res.add(matrix[upper_bound][j]);
            }
            // 上边界下移
            upper_bound++;
        }
        
        if (left_bound <= right_bound) {
            // 在右侧从上向下遍历
            for (int i = upper_bound; i <= lower_bound; i++) {
                res.add(matrix[i][right_bound]);
            }
            // 右边界左移
            right_bound--;
        }
        
        if (upper_bound <= lower_bound) {
            // 在底部从右向左遍历
            for (int j = right_bound; j >= left_bound; j--) {
                res.add(matrix[lower_bound][j]);
            }
            // 下边界上移
            lower_bound--;
        }
        
        if (left_bound <= right_bound) {
            // 在左侧从下向上遍历
            for (int i = lower_bound; i >= upper_bound; i--) {
                res.add(matrix[i][left_bound]);
            }
            // 左边界右移
            left_bound++;
        }
    }
    return res;
}
```

## 栈和队列

### 扁平化嵌套列表迭代器（力扣341）

本题定义了一个类 NestedInteger ，这个类可以存储 int  或 `ListNested<Integer>` ；所以称它是一个「嵌套列表」。类似于一棵多叉树，每个节点都可以有很多子节点。它有三个方法：

isInteger() ，判断当前存储的对象是否为 int；
getInteger() , 如果当前存储的元素是 int 型的，那么返回当前的结果 int，否则调用会失败；
getList() ，如果当前存储的元素是  `ListNested<Integer>` 型的，那么返回该 List，否则调用会失败。
而「扁平化嵌套列表迭代器」说的是，我们需要设计一个迭代器，这个迭代器是把「嵌套列表」铺平（拆包）成各个 int，然后每次调用 hasNext() 来判断是否有下一个整数，通过 next() 返回下一个整数。

另外一个做法是，我们不对所有的元素进行预处理。

而是先将所有的 `NestedInteger` 逆序放到栈中，当需要展开的时候才进行展开。

```java
public class NestedIterator implements Iterator<Integer> {
    //双向队列
    Deque<NestedInteger> stack = new ArrayDeque<>();

    public NestedIterator(List<NestedInteger> list) {
        for (int i = list.size() - 1; i >= 0; i--) {
            NestedInteger item = list.get(i);
            stack.addLast(item);
        }
    }

    @Override
    public Integer next() {
        return hasNext() ? stack.pollLast().getInteger() : -1;
    }

    @Override
    public boolean hasNext() {
        if (stack.isEmpty()) {
            return false;
        } else {
            NestedInteger item = stack.peekLast();
            if (item.isInteger()) {
                return true;
            } else {
                item = stack.pollLast();
                List<NestedInteger> list = item.getList();
                for (int i = list.size() - 1; i >= 0; i--) {
                    stack.addLast(list.get(i));
                }
                return hasNext();
            }
        }
    }
}
```

### 单调队列

既能够维护队列元素「先进先出」的时间顺序，又能够正确维护队列中所有元素的最值，这就是「单调队列」结构。

[239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

> 给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。
>
> 返回 *滑动窗口中的最大值* 。
>
> **示例 1：**
>
> ```
> 输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
> 输出：[3,3,5,5,6,7]
> 解释：
> 滑动窗口的位置                最大值
> ---------------               -----
> [1  3  -1] -3  5  3  6  7       3
>  1 [3  -1  -3] 5  3  6  7       3
>  1  3 [-1  -3  5] 3  6  7       5
>  1  3  -1 [-3  5  3] 6  7       5
>  1  3  -1  -3 [5  3  6] 7       6
>  1  3  -1  -3  5 [3  6  7]      7
> ```

利用双端队列实现单调队列

既能够维护队列元素「先进先出」的时间顺序，又能够正确维护队列中所有元素的最值，这就是「单调队列」结构。

遍历数组，将 数 存放在双向队列中，并用 L,R 来标记窗口的左边界和右边界。队列中保存的并不是真的 数，而是该数值对应的数组下标位置，并且数组中的数要从大到小排序。如果当前遍历的数比队尾的值大，则需要弹出队尾值，直到队列重新满足从大到小的要求。刚开始遍历时，L 和 R 都为 0，有一个形成窗口的过程，此过程没有最大值，L 不动，R 向右移。当窗口大小形成时，L 和 R 一起向右移，每次移动时，判断队首的值的数组下标是否在 [L,R] 中，如果不在则需要弹出队首的值，当前窗口的最大值即为队首的数。

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    // 双向队列 保存当前窗口最大值的数组位置 保证队列中数组位置的数值按从大到小排序
    Deque<Integer> queue = new ArrayDeque<>();
    // 结果数组
    int[] result = new int[nums.length-k+1];
    // 遍历nums数组
    for(int i = 0;i < nums.length;i++){
        // 保证从大到小 如果前面数小则需要依次弹出，直至满足要求
        while(!queue.isEmpty() && nums[queue.peekLast()] <= nums[i]){
            queue.pollLast();
        }
        // 添加当前值对应的数组下标
        queue.addLast(i);
        // 判断当前队列中队首的值是否有效
        if(queue.peek() <= i-k){
            queue.poll();   
        } 
        // 当窗口长度为k时 保存当前窗口中最大值
        if(i+1 >= k){
            result[i+1-k] = nums[queue.peek()];
        }
    }
    return result;
}
```



## 二叉树

### 二叉树解题的两种思维模式

1. **是否可以通过遍历一遍二叉树得到答案？**如果可以，用一个 `traverse` 函数配合外部变量来实现，这叫「遍历」的思维模式。，对应着**回溯算法**。二叉树中用遍历思路解题时函数签名一般是 `void traverse(...)`，没有返回值，靠更新外部变量来计算结果。
2. **是否可以定义一个递归函数，通过子问题（子树）的答案推导出原问题的答案？**如果可以，写出这个递归函数的定义，并充分利用这个函数的返回值，这叫「分解问题」的思维模式，对应着**动态规划**。用分解问题思路解题时函数名根据该函数具体功能而定，而且一般会有返回值，返回值是子问题的计算结果。

### 二叉树的前中后序遍历

**前中后序是遍历二叉树过程中处理每一个节点的三个特殊时间点**，绝不仅仅是三个顺序不同的 List：

前序位置的代码在**刚刚进入（第一次进入）**一个二叉树节点的时候执行；

中序位置的代码在**（第二次进入）**一个二叉树节点**左子树都遍历完，即将开始遍历右子树**的时候执行。

后序位置的代码在**将要离开（第三次进入）**一个二叉树节点的时候执行；

二叉树的所有问题，就是让你在前中后序位置注入巧妙的代码逻辑，去达到自己的目的，你只需要单独思考每一个节点应该做什么，其他的不用你管，抛给二叉树遍历框架，递归会在所有节点上做相同的操作。

### 二叉树题目的通用思考过程

1. **是否可以通过遍历一遍二叉树得到答案**？如果可以，用一个 `traverse` 函数配合外部变量来实现。
2. **是否可以定义一个递归函数，通过子问题（子树）的答案推导出原问题的答案**？如果可以，写出这个递归函数的定义，并充分利用这个函数的返回值。遇到子树问题，首先想到的是给函数设置返回值，然后在后序位置做文章。
3. **无论使用哪一种思维模式，你都要明白二叉树的每一个节点需要做什么，需要在什么时候（前中后序）做**。其他的节点不用你操心，递归函数会帮你在所有节点上执行相同的操作。

### 两种方式遍历二叉树

**「遍历」的思路**

```java
List<Integer> res = new LinkedList<>();

// 返回前序遍历结果
List<Integer> preorderTraverse(TreeNode root) {
    traverse(root);
    return res;
}

// 二叉树遍历函数
void traverse(TreeNode root) {
    if (root == null) {
        return;
    }
    // 前序位置
    res.add(root.val);
    traverse(root.left);
    traverse(root.right);
}
```

**「分解问题」的思路（不建议使用）**

```java
// 定义：输入一棵二叉树的根节点，返回这棵树的前序遍历结果
List<Integer> preorderTraverse(TreeNode root) {
    List<Integer> res = new LinkedList<>();
    if (root == null) {
        return res;
    }
    // 前序遍历的结果，root.val 在第一个
    res.add(root.val);
    // 利用函数定义，后面接着左子树的前序遍历结果
    res.addAll(preorderTraverse(root.left));
    // 利用函数定义，最后接着右子树的前序遍历结果
    res.addAll(preorderTraverse(root.right));
    return res;
}
```

java 的话无论 ArrayList 还是 LinkedList，`addAll` 方法的复杂度都是 O(N)，所以总体的最坏时间复杂度会达到 O(N^2)，除非你自己实现一个复杂度为 O(1) 的 `addAll` 方法，底层用链表的话是可以做到的，因为多条链表只要简单的指针操作就能连接起来。

### 序列化二叉树

当二叉树中节点的值**不存在重复**时：

1. 如果你的序列化结果中**不包含空指针的信息**，且你只给出**一种**遍历顺序，那么你无法还原出唯一的一棵二叉树。

2. 如果你的序列化结果中**不包含空指针的信息**，且你会给出**两种**遍历顺序，那么分两种情况：

   2.1. 如果你给出的是前序和中序，或者后序和中序，那么你可以还原出唯一的一棵二叉树。

   2.2. 如果你给出前序和后序，那么你无法还原出唯一的一棵二叉树。

3. 如果你的序列化结果中**包含空指针的信息**，且你只给出**一种**遍历顺序，也要分两种情况：

   3.1. 如果你给出的是前序或者后序，那么你可以还原出唯一的一棵二叉树。

   3.2. 如果你给出的是中序，那么你无法还原出唯一的一棵二叉树。

#### 二叉树的序列化与反序列化(力扣297)

**带空指针的前序遍历解法**

```java
public class Codec {
    String SEP = ",";
    String NULL = "#";

    /* 主函数，将二叉树序列化为字符串 */
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serialize(root, sb);
        return sb.toString();
    }

    /* 辅助函数，将二叉树存入 StringBuilder */
    void serialize(TreeNode root, StringBuilder sb) {
        if (root == null) {
            sb.append(NULL).append(SEP);
            return;
        }

        /******前序遍历位置******/
        sb.append(root.val).append(SEP);
        /***********************/

        serialize(root.left, sb);
        serialize(root.right, sb);
    }

    /* 主函数，将字符串反序列化为二叉树结构 */
    public TreeNode deserialize(String data) {
        // 将字符串转化成列表
        LinkedList<String> nodes = new LinkedList<>();
        for (String s : data.split(SEP)) {
            nodes.addLast(s);
        }
        return deserialize(nodes);
    }

    /* 辅助函数，通过 nodes 列表构造二叉树 */
    TreeNode deserialize(LinkedList<String> nodes) {
        if (nodes.isEmpty()) return null;

        /******前序遍历位置******/
        // 列表最左侧就是根节点
        String first = nodes.removeFirst();
        if (first.equals(NULL)) return null;
        TreeNode root = new TreeNode(Integer.parseInt(first));
        /***********************/

        root.left = deserialize(nodes);
        root.right = deserialize(nodes);

        return root;
    }
}
```

### *二叉树的层次遍历

```java
// 输入一棵二叉树的根节点，层序遍历这棵二叉树
void levelTraverse(TreeNode root) {
    if (root == null) return;
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    // 从上到下遍历二叉树的每一层
    while (!q.isEmpty()) {
        int sz = q.size();
        // 从左到右遍历每一层的每个节点
        for (int i = 0; i < sz; i++) {
            TreeNode cur = q.poll();
            // 将下一层节点放入队列
            if (cur.left != null) {
                q.offer(cur.left);
            }
            if (cur.right != null) {
                q.offer(cur.right);
            }
        }
    }
}
```

#### 填充每个节点的下一个右侧节点指针（力扣116）

队列中保存了第 `i` 层节点的信息，我们利用这个特点，将队列中的元素都串联一遍就可以了。

```java
class Solution {
	public Node connect(Node root) {
		if(root==null) {
			return root;
		}
		LinkedList<Node> queue = new LinkedList<Node>();
		queue.add(root);
		while(queue.size()>0) {
			int size = queue.size();
			//将队列中的元素串联起来
			Node tmp = queue.get(0);
			for(int i=1;i<size;++i) {
				tmp.next = queue.get(i);
				tmp = queue.get(i);
			}
			//遍历队列中的每个元素，将每个元素的左右节点也放入队列中
			for(int i=0;i<size;++i) {
				tmp = queue.remove();
				if(tmp.left!=null) {
					queue.add(tmp.left);
				}
				if(tmp.right!=null) {
					queue.add(tmp.right);
				}
			}
		}
		return root;
	}
} 
```

### 二叉树的最大深度（力扣104）

**「遍历」的思路**

```java
// 记录最大深度
int res = 0;
// 记录遍历到的节点的深度
int depth = 0;

// 主函数
int maxDepth(TreeNode root) {
	traverse(root);
	return res;
}

// 二叉树遍历框架
void traverse(TreeNode root) {
	if (root == null) {
		return;
	}
	// 前序位置
	depth++;
    if (root.left == null && root.right == null) {
        // 到达叶子节点，更新最大深度
		res = Math.max(res, depth);
    }
	traverse(root.left);
	traverse(root.right);
	// 后序位置
	depth--;
}

```

前序位置是进入一个节点的时候，后序位置是离开一个节点的时候，`depth` 记录当前递归到的节点深度，对 `res` 的更新，你放到前中后序位置都可以，只要保证在进入节点之后，离开节点之前（即 `depth` 自增之后，自减之前）就行了。

**「分解问题」的思路**

```java
// 定义：输入根节点，返回这棵二叉树的最大深度
int maxDepth(TreeNode root) {
	if (root == null) {
		return 0;
	}
	// 利用定义，计算左右子树的最大深度
	int leftMax = maxDepth(root.left);
	int rightMax = maxDepth(root.right);
	// 整棵树的最大深度等于左右子树的最大深度取最大值，
    // 然后再加上根节点自己
	int res = Math.max(leftMax, rightMax) + 1;

	return res;
}
```

这个思路正确的核心在于，你确实可以通过子树的最大深度推导出原树的深度，所以当然要首先利用递归函数的定义算出左右子树的最大深度，然后推出原树的最大深度，主要逻辑自然放在后序位置。

### 原地将二叉树展开为链表（力扣114）

对于一个节点 `x`，可以执行以下流程：

1、先利用 `flatten(x.left)` 和 `flatten(x.right)` 将 `x` 的左右子树拉平。

2、将 `x` 的右子树接到左子树下方，然后将整个左子树作为右子树。

这样，以 `x` 为根的整棵二叉树就被拉平了，恰好完成了 `flatten(x)` 的定义。

<img src="Algorithmimages\11.jpeg" style="zoom:50%;" />

```java
// 定义：将以 root 为根的树拉平为链表
void flatten(TreeNode root) {
    // base case
    if (root == null) return;
    
    // 利用定义，把左右子树拉平
    flatten(root.left);
    flatten(root.right);

    /**** 后序遍历位置 ****/
    // 1、左右子树已经被拉平成一条链表
    TreeNode left = root.left;
    TreeNode right = root.right;
    
    // 2、将左子树作为右子树
    root.left = null;
    root.right = left;

    // 3、将原先的右子树接到当前右子树的末端
    TreeNode p = root;
    while (p.right != null) {
        p = p.right;
    } 
    p.right = right;
}
```

### 构造最大二叉树（力扣654）

设置递归函数 TreeNode build(int[] nums, int l, int r) 含义为从 nums 中的 [l,r]下标范围进行构建，返回构建后的头结点。当 l>r时，返回空节点，否则在[l,r] 中进行扫描，找到最大值对应的下标 idx 并创建对应的头结点，递归构建 [l,idx−1] 和 [idx+1,r] 作为头节点的左右子树。

```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return build(nums, 0, nums.length - 1);
    }
    TreeNode build(int[] nums, int l, int r) {
        if (l > r) return null;
        int idx = l;
        for (int i = l; i <= r; i++) {
            if (nums[i] > nums[idx]) idx = i;
        }
        TreeNode ans = new TreeNode(nums[idx]);
        ans.left = build(nums, l, idx - 1);
        ans.right = build(nums, idx + 1, r);
        return ans;
    }
}
```

### 通过前序和中序遍历结果构造二叉树(力扣105)

**二叉树的构造问题一般都是使用「分解问题」的思路：构造整棵树 = 根节点 + 构造左子树 + 构造右子树**。

对于左右子树对应的 `inorder` 数组的起始索引和终止索引比较容易确定：

<img src="Algorithmimages\12.JPG" style="zoom:50%;" />

对于 `preorder` 数组可以通过左子树的节点数推导出来，假设左子树的节点数为 `leftSize`，那么 `preorder` 数组上的索引情况是这样的：

<img src="Algorithmimages\13.JPG" style="zoom:50%;" />

```java
class Solution {
    // 存储 inorder 中值到索引的映射
    //因为题目说二叉树节点的值不存在重复，所以可以使用一个 HashMap 存储元素到索引的映射
    //这样就可以直接通过 HashMap 查到 rootVal 对应的 index
    HashMap<Integer, Integer> valToIndex = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) {
            valToIndex.put(inorder[i], i);
        }
        return build(preorder, 0, preorder.length - 1,inorder, 0, inorder.length - 1);
    }
    public TreeNode build(int[] preorder, int preStart, int preEnd, int[] inorder, int inStart, int inEnd) {
        if (preStart > preEnd) {
            return null;
        }
        // root 节点对应的值就是前序遍历数组的第一个元素
        int rootVal = preorder[preStart];
        // rootVal 在中序遍历数组中的索引
        int index = valToIndex.get(rootVal);
        int leftSize = index - inStart;
        // 先构造出当前根节点
        TreeNode root = new TreeNode(rootVal);
        // 递归构造左右子树
        root.left = build(preorder, preStart + 1, preStart + leftSize,inorder, inStart, index - 1);
        root.right = build(preorder, preStart + leftSize + 1, preEnd,inorder, index + 1, inEnd);
        return root;
    }
}
```

### 通过后序和中序遍历结果构造二叉树(力扣106)

本题与上一题相似，只要修改参数即可

<img src="Algorithmimages\14.JPG" style="zoom:50%;" />

```java
class Solution {
    HashMap<Integer, Integer> valToIndex = new HashMap<>();
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        for (int i = 0; i < inorder.length; i++) {
            valToIndex.put(inorder[i], i);
        }
        return build(postorder, 0, postorder.length - 1,inorder, 0, inorder.length - 1);
    }
    public TreeNode build(int[] postorder,int postStart,int postEnd,int[] inorder,int inStart,int inEnd){
        if (postStart > postEnd) {
            return null;
        }
        // root 节点对应的值就是后序遍历数组的最后一个元素
        int rootVal = postorder[postEnd];
        // rootVal 在中序遍历数组中的索引
        int index = valToIndex.get(rootVal);
        int leftSize = index - inStart;
        // 先构造出当前根节点
        TreeNode root = new TreeNode(rootVal);
        // 递归构造左右子树
        root.left = build(postorder, postStart, postStart + leftSize - 1,inorder, inStart, index - 1);
        root.right = build(postorder, postStart + leftSize, postEnd - 1,inorder, index + 1, inEnd);
        return root;
    }
}
```

### 通过后序和前序遍历结果构造二叉树（力扣889）

我们假设前序遍历的第二个元素是左子树的根节点，但实际上左子树有可能是空指针，那么这个元素就应该是右子树的根节点。由于这里无法确切进行判断，所以导致了最终答案的不唯一。

1、首先把前序遍历结果的第一个元素或者后序遍历结果的最后一个元素确定为根节点的值。

2、然后把前序遍历结果的第二个元素作为左子树的根节点的值。

3、在后序遍历结果中寻找左子树根节点的值，从而确定了左子树的索引边界，进而确定右子树的索引边界，递归构造左右子树即可。

<img src="https://labuladong.online/algo/images/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%B3%BB%E5%88%972/8.jpeg" alt="img" style="zoom:50%;" />

```java
class Solution {
    // 存储 postorder 中值到索引的映射
    HashMap<Integer, Integer> valToIndex = new HashMap<>();

    public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
        for (int i = 0; i < postorder.length; i++) {
            valToIndex.put(postorder[i], i);
        }
        return build(preorder, 0, preorder.length - 1,
                    postorder, 0, postorder.length - 1);
    }

    // 定义：根据 preorder[preStart..preEnd] 和 postorder[postStart..postEnd]
    // 构建二叉树，并返回根节点。
    TreeNode build(int[] preorder, int preStart, int preEnd,
                   int[] postorder, int postStart, int postEnd) {
        if (preStart > preEnd) {
            return null;
        }
        if (preStart == preEnd) {
            return new TreeNode(preorder[preStart]);
        }

        // root 节点对应的值就是前序遍历数组的第一个元素
        int rootVal = preorder[preStart];
        // root.left 的值是前序遍历第二个元素
        // 通过前序和后序遍历构造二叉树的关键在于通过左子树的根节点
        // 确定 preorder 和 postorder 中左右子树的元素区间
        int leftRootVal = preorder[preStart + 1];
        // leftRootVal 在后序遍历数组中的索引
        int index = valToIndex.get(leftRootVal);
        // 左子树的元素个数
        int leftSize = index - postStart + 1;

        // 先构造出当前根节点
        TreeNode root = new TreeNode(rootVal);
        // 递归构造左右子树
        // 根据左子树的根节点索引和元素个数推导左右子树的索引边界
        root.left = build(preorder, preStart + 1, preStart + leftSize,
                postorder, postStart, index);
        root.right = build(preorder, preStart + leftSize + 1, preEnd,
                postorder, index + 1, postEnd - 1);

        return root;
    }
}
```

### 寻找重复的子树(力扣652)(DFS)

设计递归函数 String dfs(TreeNode root)，含义为返回以传入参数 root 为根节点的子树所对应的指纹标识。

对于标识的设计只需使用 "_" 分割不同的节点值，同时对空节点进行保留（定义为空串 " "）即可。

使用哈希表记录每个标识（子树）出现次数，当出现次数为 2 首次判定为重复出现）时，将该节点加入答案。

```java
class Solution {
    Map<String,Integer> map = new HashMap<>();
    List<TreeNode> ans = new ArrayList<>();
    public List<TreeNode> findDuplicateSubtrees(TreeNode root){
        dfs(root);
        return ans;
    }
    public String dfs(TreeNode root) {
        if (root == null)
            return "";
        StringBuilder sb = new StringBuilder();
        sb.append(root.val).append("_");
        //区分左右子树节点序列相同的情况
        sb.append(dfs(root.left)).append("-").append(dfs(root.right));
        String key = sb.toString();
        map.put(key, map.getOrDefault(key,0) + 1);
        if (map.get(key) == 2)
            ans.add(root);
        return key;
    }
}
```

### 二叉树的最近公共祖先（力扣236）

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先（Lowest Common Ancestor， LCA）。最近公共祖先的定义为：对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。

递归解析：

终止条件：

- 当越过叶节点，则直接返回 null
- 当 root 等于 p,q，则直接返回 root 

递推工作：

- 开启递归左子节点，返回值记为 left
- 开启递归右子节点，返回值记为 right

返回值： 根据 left和 right，可展开为四种情况；

1. 当 left 和 right 同时为空 ：说明 root 的左 / 右子树中都不包含 p,q ，返回 null 

2. 当 left 和 right 同时不为空 ：说明 p,q分列在 root 的 异侧 （分别在 左 / 右子树），因此 root 为最近公共祖先，返回 root

3. 当 left 为空 ，right 不为空 ：p,q 都不在 root 的左子树中，直接返回 right 。具体可分为两种情况：

   + p,q 其中一个在 root 的 右子树 中，此时 righ 指向 p（假设为 p ）；

   + p,q 两节点都在 root 的 右子树 中，此时的 right 指向 最近公共祖先节点 ；
   + 当 left不为空 ， right 为空 ：与情况 3. 同理

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) {
            //只要当前根节点是p和q中的任意一个，就返回（因为不能比这个更深了，再深p和q中的一个就没了）
            return root;
        }
        //根节点不是p和q中的任意一个，那么就继续分别往左子树和右子树找p和q
        //因为是递归，使用函数后可认为左右子树已经算出结果
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        //p和q都没找到，那就没有
        if(left == null && right == null) {
            return null;
        }
        //左子树没有p也没有q，就返回右子树的结果
        if (left == null) {
            return right;
        }
        //右子树没有p也没有q就返回左子树的结果
        if (right == null) {
            return left;
        }
        //左右子树都找到p和q了，那就说明p和q分别在左右两个子树上，所以此时的最近公共祖先就是root
        return root;
    }
}
```

### 计算完全二叉树的节点数（力扣222）

如果是一个**普通**二叉树，显然只要向下面这样遍历一边即可，时间复杂度 O(N)：

```java
public int countNodes(TreeNode root) {
    if (root == null) return 0;
    return 1 + countNodes(root.left) + countNodes(root.right);
}
```

那如果是一棵**满**二叉树，节点总数就和树的高度呈指数关系：

```java
public int countNodes(TreeNode root) {
    int h = 0;
    // 计算树的高度
    while (root != null) {
        root = root.left;
        h++;
    }
    // 节点总数就是 2^h - 1
    return (int)Math.pow(2, h) - 1;
}
```

**完全**二叉树比普通二叉树特殊，但又没有满二叉树那么特殊，计算它的节点总数，可以说是普通二叉树和完全二叉树的结合版，先看代码：

```java
public int countNodes(TreeNode root) {
    TreeNode left = root, right = root;
    // 沿最左侧和最右侧分别计算高度
    int leftDepth = 0, rightDepth = 0;
    while(left != null){
        left = left.left;
        leftDepth++;
    }
    while(right != null){
        right = right.right;
        rightDepth++;
    }
    // 如果左右侧计算的高度相同，则是一棵满二叉树
    if(leftDepth == rightDepth)
        return (int)Math.pow(2,rightDepth) - 1;
    // 如果左右侧的高度不同，则按照普通二叉树的逻辑计算
    return countNodes(root.left) + countNodes(root.right) + 1;
}
```

这个算法的时间复杂度是 O(logN*logN)，这是怎么算出来的呢？

直觉感觉好像最坏情况下是 O(N*logN) 吧，因为之前的 while 需要 logN 的时间，最后要 O(N) 的时间向左右子树递归：

**关键点在于，这两个递归只有一个会真的递归下去，另一个一定会触发 `hl == hr` 而立即返回，不会递归下去**。

为什么呢？原因如下：

**一棵完全二叉树的两棵子树，至少有一棵是满二叉树**：

<img src="Algorithmimages\dsdsd.JPG" style="zoom:50%;" />

看图就明显了吧，由于完全二叉树的性质，其子树一定有一棵是满的，所以一定会触发 `hl == hr`，只消耗 O(logN) 的复杂度而不会继续递归。综上，算法的递归深度就是树的高度 O(logN)，每次递归所花费的时间就是 while 循环，需要 O(logN)，所以总体的时间复杂度是 O(logN*logN)。

## 二叉搜索树(Binary Search Tree)

### 降序遍历二叉搜索树(力扣538)

这段代码可以降序打印 BST 节点的值，如果维护一个外部累加变量 `sum`，然后把 `sum` 赋值给 BST 中的每一个节点

```java
TreeNode convertBST(TreeNode root) {
    traverse(root);
    return root;
}

// 记录累加和
int sum = 0;
void traverse(TreeNode root) {
    if (root == null) {
        return;
    }
    traverse(root.right);
    // 维护累加和
    sum += root.val;
    // 将 BST 转化成累加树
    root.val = sum;
    traverse(root.left);
}
```

### 二叉搜索树中第K小的元素（力扣230）

```java
class Solution {
    int count = 0;
    int res;
    public int kthSmallest(TreeNode root, int k) {
        travase(root, k);
        return res;
    }

    private void travase(TreeNode root,int k) {
        if (root == null)
            return;
        travase(root.left,k);
        count++;
        if (count == k) {
            res = root.val;
            return;
        }
        travase(root.right,k);
    }
}
```

### 验证二叉搜索树的合法性（力扣98）

前序遍历，用min和max记录根节点的值

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isBST(root,null,null);
    }

    private boolean isBST(TreeNode root, TreeNode min, TreeNode max) {
        if (root == null)
            return true;
        if (min != null && root.val <= min.val)
            return false;
        if (max != null && root.val >= max.val)
            return false;
        return isBST(root.left, min, root) && isBST(root.right, root ,max);
    }
}
```

中序遍历，用pre记录前一个节点的值。中序遍历时，判断当前节点是否大于中序遍历的前一个节点，如果大于，说明满足 BST，继续遍历；否则直接返回 false。

```java
class Solution {
    long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        // 访问左子树
        if (!isValidBST(root.left)) {
            return false;
        }
        // 访问当前节点：如果当前节点小于等于中序遍历的前一个节点，说明不满足BST，返回 false；否则继续遍历。
        if (root.val <= pre) {
            return false;
        }
        pre = root.val;
        // 访问右子树
        return isValidBST(root.right);
    }
}
```

### 二叉搜索树的基本操作

#### 在 BST 中搜索一个数(力扣700)

```java
public TreeNode searchBST(TreeNode root, int val) {
        if (root == null || val == root.val)
            return root;
        if (val < root.val)
            return searchBST(root.left, val);
        if (val > root.val)
            return searchBST(root.right, val);
        return null;
    }
```

#### 在 BST 中插入一个数

对数据结构的操作无非遍历 + 访问，遍历就是「找」，访问就是「改」。具体到这个问题，插入一个数，就是先找到插入位置，然后进行插入操作。一旦涉及「改」，就类似二叉树的构造问题，函数要返回 `TreeNode` 类型，并且要对递归调用的返回值进行接收。

```java
TreeNode insertIntoBST(TreeNode root, int val) {
    // 找到空位置插入新节点
    if (root == null) return new TreeNode(val);
    // if (root.val == val)
    //     BST 中一般不会插入已存在元素
    if (root.val < val) 
        root.right = insertIntoBST(root.right, val);
    if (root.val > val) 
        root.left = insertIntoBST(root.left, val);
    return root;
}
```

#### *在 BST 中删除一个数

找到目标节点了，比方说是节点 `A`，如何删除这个节点，这是难点。因为删除节点的同时不能破坏 BST 的性质。有三种情况，用图片来说明。

**情况 1**：`A` 恰好是末端节点，两个子节点都为空，那么它可以当场去世了。

<img src="Algorithmimages\bstdel1.png" style="zoom: 25%;" />

**情况 2**：`A` 只有一个非空子节点，那么它要让这个孩子接替自己的位置。

<img src="Algorithmimages\bstdel2.png" style="zoom: 50%;" />

**情况 3**：`A` 有两个子节点，麻烦了，为了不破坏 BST 的性质，`A` 必须找到左子树中最大的那个节点，或者右子树中最小的那个节点来接替自己。我们以第二种方式讲解。

![](Algorithmimages\bstdel3.png)

三种情况分析完毕，填入框架，简化一下代码：

```java
TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) return null;
    if (root.val == key) {
        // 这两个 if 把情况 1 和 2 都正确处理了
        if (root.left == null) return root.right;
        if (root.right == null) return root.left;
        // 处理情况 3
        // 获得右子树最小的节点
        TreeNode minNode = getMin(root.right);
        // 删除右子树最小的节点
        root.right = deleteNode(root.right, minNode.val);
        // 用右子树最小的节点替换 root 节点
        minNode.left = root.left;
        minNode.right = root.right;
        root = minNode;
    } else if (root.val > key) {
        root.left = deleteNode(root.left, key);
    } else if (root.val < key) {
        root.right = deleteNode(root.right, key);
    }
    return root;
}

TreeNode getMin(TreeNode node) {
    // BST 最左边的就是最小的
    while (node.left != null) node = node.left;
    return node;
}
```

替换 `root` 节点为什么这么麻烦，直接改 `val` 字段不就行了？看起来还更简洁易懂：

```java
// 处理情况 3
// 获得右子树最小的节点
TreeNode minNode = getMin(root.right);
// 删除右子树最小的节点
root.right = deleteNode(root.right, minNode.val);
// 用右子树最小的节点替换 root 节点
root.val = minNode.val;
```

仅对于这道算法题来说是可以的，但这样操作并不完美，我们一般不会通过修改节点内部的值来交换节点。因为在实际应用中，BST 节点内部的数据域是用户自定义的，可以非常复杂，而 BST 作为数据结构（一个工具人），其操作应该和内部存储的数据域解耦，所以我们更倾向于使用指针操作来交换节点，根本没必要关心内部数据。

### 不同的二叉搜索树（力扣96）

给你一个整数 `n` ，求恰由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的 **二叉搜索树** 有多少种？返回满足题意的二叉搜索树的种数。

G(n): 长度为 n 的序列能构成的不同二叉搜索树的个数。

F(i,n): 以 i为根、序列长度为 n 的不同二叉搜索树个数 (1≤i≤n)

不同的二叉搜索树的总数 G(n)，是对遍历所有 i (1≤i≤n)的 F(i,n)之和。对于边界情况，当序列长度为 1（只有根）或为 0（空树）时，只有一种情况，即：G(0)=1, G(1)=1

举例而言，创建以 3 为根、长度为 7 的不同二叉搜索树，整个序列是 [1,2,3,4,5,6,7]，我们需要从左子序列 [1,2] 构建左子树，从右子序列 [4,5,6,7] 构建右子树，然后将它们组合（即笛卡尔积）。

对于这个例子，不同二叉搜索树的个数为 F(3,7)。我们将 [1,2] 构建不同左子树的数量表示为 G(2), 从 [4,5,6,7] 构建不同右子树的数量表示为 G(4)，注意到 G(n) 和序列的内容无关，只和序列的长度有关。于是，F(3,7)=G(2)⋅G(4)。 因此，我们可以得到F(i,n)=G(i−1)⋅G(n−i)

综合两个公式可以得到 **卡特兰数** 公式
**G(n)= G(0)∗G(n−1) + G(1)∗G(n−2) + ... + G(n−1)∗G(0)**

```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        
        for(int i = 2; i < n + 1; i++)
            for(int j = 1; j < i + 1; j++) 
                dp[i] += dp[j-1] * dp[i-j];
        
        return dp[n];
    }
}
```

### 不同的二叉搜索树 II（力扣95）

我们定义 generateTrees(start, end) 函数表示当前值的集合为[start,end]，返回序列 [start,end] 生成的所有可行的二叉搜索树。按照上文的思路，我们考虑枚举 [start,end] 中的值 i 为当前二叉搜索树的根，那么序列划分为了 [start,i−1] 和 [i+1,end] 两部分。我们递归调用这两部分，即 generateTrees(start, i - 1) 和 generateTrees(i + 1, end)，获得所有可行的左子树和可行的右子树，那么最后一步我们只要从可行左子树集合中选一棵，再从可行右子树集合中选一棵拼接到根节点上，并将生成的二叉搜索树放入答案数组即可。

递归的入口即为 generateTrees(1, n)，出口为当 start>end 的时候，当前二叉搜索树为空，返回空节点即可。

```java
class Solution {
    public List<TreeNode> generateTrees(int n) {
        if (n == 0) {
            return new LinkedList<TreeNode>();
        }
        return generateTrees(1, n);
    }

    public List<TreeNode> generateTrees(int start, int end) {
        List<TreeNode> allTrees = new LinkedList<TreeNode>();
        if (start > end) {
            allTrees.add(null);
            return allTrees;
        }

        // 枚举可行根节点
        for (int i = start; i <= end; i++) {
            // 获得所有可行的左子树集合
            List<TreeNode> leftTrees = generateTrees(start, i - 1);

            // 获得所有可行的右子树集合
            List<TreeNode> rightTrees = generateTrees(i + 1, end);

            // 从左子树集合中选出一棵左子树，从右子树集合中选出一棵右子树，拼接到根节点上
            for (TreeNode left : leftTrees) {
                for (TreeNode right : rightTrees) {
                    TreeNode currTree = new TreeNode(i);
                    currTree.left = left;
                    currTree.right = right;
                    allTrees.add(currTree);
                }
            }
        }
        return allTrees;
    }
}
```

### 二叉搜索树的最近公共祖先（力扣235）

因为是二叉搜索树，所以可以分为四种情况：

1. 当 root 等于 p 或 q 其中之一，则直接返回 root
2. 当 p 和 q 全部大于 root ，则直接递归进入 root.right 寻找
3. 当 p 和 q 全部小于 root ，则直接递归进入 root.left 寻找
4. 当 p > root , q < root 或者 p < root , q > root 时，说明 root 就是 p, q 的最近公共祖先

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        //当 root 等于 p 或 q 其中之一，则直接返回 root
        if(p.val == root.val || q.val == root.val || root == null)
            return root;
        //当 p 和 q 全部大于 root ，则直接递归进入 root.right 寻找
        if(p.val > root.val && q.val > root.val)
            return lowestCommonAncestor(root.right, p, q);
        //当 p 和 q 全部小于 root ，则直接递归进入 root.left 寻找
        if(p.val < root.val && q.val < root.val)
            return lowestCommonAncestor(root.left, p, q);
        //当 p > root , q < root 或者 p < root , q > root 时，说明 root 就是 p, q 的最近公共祖先
        return root;
    }
}
```



## 深度优先遍历（DFS）（回溯算法）

**解决一个回溯问题，实际上就是遍历一棵决策树的过程，树的每个叶子节点存放着一个合法答案。你把整棵树遍历一遍，把叶子节点上的答案都收集起来，就能得到所有的合法答案**。

站在回溯树的一个节点上，你只需要思考 3 个问题：

1、路径：也就是已经做出的选择。

2、选择列表：也就是你当前可以做的选择。

3、结束条件：也就是到达决策树底层，无法再做选择的条件。

代码方面，回溯算法的框架：

```python
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

**其核心就是 for 循环里面的递归，在递归调用之前「做选择」，在递归调用之后「撤销选择」**

### 二维矩阵DFS遍历框架

```java
void dfs(int[][] grid, int i, int j, boolean[][] visited) {
    int m = grid.length, n = grid[0].length;
    if (i < 0 || j < 0 || i >= m || j >= n) {
        // 超出索引边界
        return;
    }
    if (visited[i][j]) {
        // 已遍历过 (i, j)
        return;
    }
    // 进入节点 (i, j)
    visited[i][j] = true;
    dfs(grid, i - 1, j, visited); // 上
    dfs(grid, i + 1, j, visited); // 下
    dfs(grid, i, j - 1, visited); // 左
    dfs(grid, i, j + 1, visited); // 右
}
```



### 排列组合算法框架

代码方面，回溯算法的框架：

```python
public List<List<Integer>> res = new LinkedList<>();//记录结果
public LinkedList<Integer> track = new LinkedList<>();// 记录「路径」

public List<List<Integer>> main(int[] nums) {
    boolean[] used = new boolean[nums.length];
    backtrack(nums, used);
    return res;
}

public void backtrack(int[] nums, boolean[] used) {
    if (触发结束条件) {
        res.add(new LinkedList(track));
        return;
    }
    for (int i = 0; i < nums.length; i++) {
        // 做选择
        if (used[i])
            continue;
        
        //如果有剪枝之类的操作限制在此处
        
        track.add(nums[i]);
        used[i] = true;
        // 进入下一层决策树
        backtrack(nums, used, track);
        // 取消选择
        track.removeLast();
        used[i] = false;
    }
}
```

**为什么是res.add(new LinkedList(track)) ？**

在Java中，集合（Collection）的添加是按照引用传递的方式进行的。也就是说，当你将 track 添加到 res 中时，实际上是将 track 的引用添加到了 res 中，而不是 track 的值。如果你之后修改了 track，那么在 res 中引用的 track 也会相应地改变。因此，如果你在之后修改了 track，那么 res 中添加的 track 也会受到影响。如果你想在 `res` 中保存 `track` 的当前状态，你应该在 `res.add(track)` 之后再创建一个新的 `track` 对象，例如：

```
res.add(new ArrayList<>(track));
```

这样，`res` 中的每个 `track` 都是一个独立的对象，而不是对同一个对象的引用。



#### 形式一、元素无重不可复选

**即 `nums` 中的元素都是唯一的，每个元素最多只能被使用一次**，`backtrack` 核心代码如下：

```java
/* 组合/子集问题回溯算法框架 */
void backtrack(int[] nums, int start) {
    // 回溯算法标准框架
    for (int i = start; i < nums.length; i++) {
        // 做选择
        track.addLast(nums[i]);
        // 注意参数
        backtrack(nums, i + 1);
        // 撤销选择
        track.removeLast();
    }
}

/* 排列问题回溯算法框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // 剪枝逻辑
        if (used[i]) {
            continue;
        }
        // 做选择
        used[i] = true;
        track.addLast(nums[i]);

        backtrack(nums);
        // 撤销选择
        track.removeLast();
        used[i] = false;
    }
}
```

#### 形式二、元素可重不可复选

**即 `nums` 中的元素可以存在重复，每个元素最多只能被使用一次**，其关键在于排序和剪枝，`backtrack` 核心代码如下：

```java
Arrays.sort(nums);
/* 组合/子集问题回溯算法框架 */
void backtrack(int[] nums, int start) {
    // 回溯算法标准框架
    for (int i = start; i < nums.length; i++) {
        // 剪枝逻辑，跳过值相同的相邻树枝
        if (i > start && nums[i] == nums[i - 1]) {
            continue;
        }
        // 做选择
        track.addLast(nums[i]);
        // 注意参数
        backtrack(nums, i + 1);
        // 撤销选择
        track.removeLast();
    }
}


Arrays.sort(nums);
/* 排列问题回溯算法框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // 剪枝逻辑
        if (used[i]) {
            continue;
        }
        // 剪枝逻辑，固定相同的元素在排列中的相对位置
        if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
            continue;
        }
        // 做选择
        used[i] = true;
        track.addLast(nums[i]);

        backtrack(nums);
        // 撤销选择
        track.removeLast();
        used[i] = false;
    }
}
```

#### 形式三、元素无重可复选

**即 `nums` 中的元素都是唯一的，每个元素可以被使用若干次**，只要删掉去重逻辑即可，`backtrack` 核心代码如下：

```java
/* 组合/子集问题回溯算法框架 */
void backtrack(int[] nums, int start) {
    // 回溯算法标准框架
    for (int i = start; i < nums.length; i++) {
        // 做选择
        track.addLast(nums[i]);
        // 注意参数
        backtrack(nums, i);
        // 撤销选择
        track.removeLast();
    }
}


/* 排列问题回溯算法框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // 做选择
        track.addLast(nums[i]);
        backtrack(nums);
        // 撤销选择
        track.removeLast();
    }
}
```



### 排列

元素无重不可复选（需要used数组记录是否用过）

```java
public static List<List<Integer>> res = new LinkedList<>();//记录结果
public static LinkedList<Integer> track = new LinkedList<>();// 记录「路径」

public static void backtrack(int[] nums, boolean[] used, LinkedList<Integer> track) {
    // 触发结束条件（如果有剪枝之类的操作限制在此处）
    if (track.size() == nums.length) {
        res.add(new LinkedList(track));
        return;
    }
    for (int i = 0; i < nums.length; i++) {
        // 做选择
        if (used[i])
            continue;
        //（如果有剪枝之类的操作限制在此处）
        //if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
        //        // 如果前面的相邻相等元素没有用过，则跳过
        //        continue;
        //    }
        track.add(nums[i]);
        used[i] = true;
        // 进入下一层决策树
        backtrack(nums, used, track);
        // 取消选择
        track.removeLast();
        used[i] = false;
    }
}
public static List<List<Integer>> permute(int[] nums) {
    boolean[] used = new boolean[nums.length];
    backtrack(nums, used, track);
    return res;
}
```

### 子集/组合

元素无重不可复选（需要start不走走过的选择，下次从i+1开始）

元素无重可复选（需要start不走走过的选择，下次从i开始）

```java 
public static List<List<Integer>> res = new LinkedList<>();//记录结果
public static LinkedList<Integer> track = new LinkedList<>();// 记录「路径」
public static void backtrack(int n, int k, int start, LinkedList<Integer> track) {
    // 触发结束条件
    if (track.size() == k) {
        //见下面解释
        res.add(new LinkedList(track));
        return;
    }
    for (int i = start; i <= n; i++) {
        //（如果有剪枝之类的操作限制在此处）
        //...
        //if (i > start) {
        //       if (nums[i] == nums[i - 1])
        //            continue;
        //  }
        // 做选择
        track.add(i);
        // 进入下一层决策树
        backtrack(n, k, i+1, track);
        // 取消选择
        track.removeLast();
    }
}
public static List<List<Integer>> combine(int n, int k) {
    backtrack(n, k, 1, track);
    return res;
}
```



### 图的所有可能路径（力扣797）

给你一个有 `n` 个节点的 有向无环图（DAG），请你找出所有从节点 `0` 到节点 `n-1` 的路径并输出（不要求按特定顺序）`graph[i]` 是一个从节点 `i` 可以访问的所有节点的列表（即从节点 `i` 到节点 `graph[i][j]`存在一条有向边）。

```java
List<List<Integer>> res = new LinkedList<>();
LinkedList<Integer> path = new LinkedList<>();
public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
    //始终从0开始，所以总是需要把节点0加入
    path.addLast(0);
    dfs(graph,0);
    return res;
}
//深度搜索，第一个参数是要遍历的图，第二参数是当前节点编号
public void dfs(int[][] graph, int n) {
    //如果遍历到最后一个节点，则停止遍历
    if (n == graph.length - 1){
        //达到目标节点，保存此条路径并结束搜索
        res.add(new LinkedList<>(path));
        return;
    }
    //遍历当前节点所有关联的节点
    for (int i : graph[n]) {
        //将当前节点保存在本次搜索路径中
        path.addLast(i);
        //递归遍历与当前节点关联的节点
        dfs(graph,i);
        //回溯，撤销选择
        path.removeLast();
    }
}
```

### 图的环检测算法（DFS）（力扣207）

在进入节点 `s` 的时候将 `onPath[s]` 标记为 true，离开时标记回 false，如果发现 `onPath[s]` 已经被标记，说明出现了环。

```java
class Solution {
    // 记录一次递归堆栈中的节点
    boolean[] onPath;
    // 记录遍历过的节点，防止走回头路
    boolean[] visited;
    // 记录图中是否有环
    boolean hasCycle = false;

    boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = buildGraph(numCourses, prerequisites);
        
        visited = new boolean[numCourses];
        onPath = new boolean[numCourses];
        
        for (int i = 0; i < numCourses; i++) {
            // 遍历图中的所有节点
            traverse(graph, i);
        }
        // 只要没有循环依赖可以完成所有课程
        return !hasCycle;
    }

    void traverse(List<Integer>[] graph, int s) {
        if (onPath[s]) {
            // 出现环
            hasCycle = true;
        }
        
        if (visited[s] || hasCycle) {
            // 如果已经找到了环，也不用再遍历了
            return;
        }
        // 前序代码位置
        visited[s] = true;
        onPath[s] = true;
        for (int t : graph[s]) {
            traverse(graph, t);
        }
        // 后序代码位置
        onPath[s] = false;
    }

    List<Integer>[] buildGraph(int numCourses, int[][] prerequisites) {
        // 图中共有 numCourses 个节点
        List<Integer>[] graph = new LinkedList[numCourses];
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new LinkedList<>();
        }
        for (int[] edge : prerequisites) {
            int from = edge[1], to = edge[0];
            // 添加一条从 from 指向 to 的有向边
            // 边的方向是「被依赖」关系，即修完课程 from 才能修课程 to
            graph[from].add(to);
        }
        return graph;
    }
}
```

### 判断二分图DFS（力扣785）

给你一幅「图」，请你用两种颜色将图中的所有顶点着色，且使得任意一条边的两个端点的颜色都不相同，这就是图的「双色问题」，其实这个问题就等同于二分图的判定问题，如果你能够成功地将图染色，那么这幅图就是一幅二分图，反之则不是。

判定二分图的算法很简单，就是用代码解决「双色问题」。**说白了就是遍历一遍图，一边遍历一边染色，看看能不能用两种颜色给所有节点染色，且相邻节点的颜色都不相同**。

```java
class Solution {
    // 记录图是否符合二分图性质
    private boolean ok = true;
    // 记录图中节点的颜色，false 和 true 代表两种不同颜色
    private boolean[] color;
    // 记录图中节点是否被访问过
    private boolean[] visited;

    // 主函数，输入邻接表，判断是否是二分图
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        color = new boolean[n];
        visited = new boolean[n];
        // 因为图不一定是联通的，可能存在多个子图
        // 所以要把每个节点都作为起点进行一次遍历
        // 如果发现任何一个子图不是二分图，整幅图都不算二分图
        for (int v = 0; v < n; v++) {
            if (!visited[v]) {
                traverse(graph, v);
            }
        }
        return ok;
    }

    // DFS 遍历框架
    private void traverse(int[][] graph, int v) {
        // 如果已经确定不是二分图了，就不用浪费时间再递归遍历了
        if (!ok) return;

        visited[v] = true;
        for (int w : graph[v]) {
            if (!visited[w]) {
                // 相邻节点 w 没有被访问过
                // 那么应该给节点 w 涂上和节点 v 不同的颜色
                color[w] = !color[v];
                // 继续遍历 w
                traverse(graph, w);
            } else {
                // 相邻节点 w 已经被访问过
                // 根据 v 和 w 的颜色判断是否是二分图
                if (color[w] == color[v]) {
                    // 若相同，则此图不是二分图
                    ok = false;
                    return;
                }
            }
        }
    }
}
```

### 可能的二分图DFS（力扣886）

给定一组 `n` 人（编号为 `1, 2, ..., n`）， 我们想把每个人分进**任意**大小的两组。每个人都可能不喜欢其他人，那么他们不应该属于同一组。给定整数 `n` 和数组 `dislikes` ，其中 `dislikes[i] = [ai, bi]` ，表示不允许将编号为 `ai` 和 `bi`的人归入同一组。当可以用这种方法将所有人分进两组时，返回 `true`；否则返回 `false`。

```java
class Solution {
    // 记录图是否符合二分图性质
    private boolean ok = true;
    // 记录图中节点的颜色，false 和 true 代表两种不同颜色
    private boolean[] color;
    // 记录图中节点是否被访问过
    private boolean[] visited;

    // 主函数，输入邻接表，判断是否是二分图
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        color = new boolean[n];
        visited = new boolean[n];
        // 因为图不一定是联通的，可能存在多个子图
        // 所以要把每个节点都作为起点进行一次遍历
        // 如果发现任何一个子图不是二分图，整幅图都不算二分图
        for (int v = 0; v < n; v++) {
            if (!visited[v]) {
                traverse(graph, v);
            }
        }
        return ok;
    }

    // DFS 遍历框架
    private void traverse(int[][] graph, int v) {
        // 如果已经确定不是二分图了，就不用浪费时间再递归遍历了
        if (!ok) return;

        visited[v] = true;
        for (int w : graph[v]) {
            if (!visited[w]) {
                // 相邻节点 w 没有被访问过
                // 那么应该给节点 w 涂上和节点 v 不同的颜色
                color[w] = !color[v];
                // 继续遍历 w
                traverse(graph, w);
            } else {
                // 相邻节点 w 已经被访问过
                // 根据 v 和 w 的颜色判断是否是二分图
                if (color[w] == color[v]) {
                    // 若相同，则此图不是二分图
                    ok = false;
                    return;
                }
            }
        }
    }
}
```

### 被围绕的区域DFS（力扣130）

使用 DFS 算法解决：先用 for 循环遍历棋盘的四边，用 DFS 算法把那些与边界相连的 `O` 换成一个特殊字符，比如 `#`；然后再遍历整个棋盘，把剩下的 `O` 换成 `X`，把 `#` 恢复成 `O`。这样就能完成题目的要求，时间复杂度 O(MN)。

```java
class Solution {
    public void solve(char[][] board) {
        if (board == null || board.length == 0) return;
        int m = board.length;
        int n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 从边缘o开始搜索
                boolean isEdge = i == 0 || j == 0 || i == m - 1 || j == n - 1;
                if (isEdge && board[i][j] == 'O') {
                    dfs(board, i, j);
                }
            }
        }

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                }
                if (board[i][j] == '#') {
                    board[i][j] = 'O';
                }
            }
        }
    }

    public void dfs(char[][] board, int i, int j) {
        if (i < 0 || j < 0 || i >= board.length  || j >= board[0].length || board[i][j] == 'X' || board[i][j] == '#') {
            // board[i][j] == '#' 说明已经搜索过了. 
            return;
        }
        board[i][j] = '#';
        dfs(board, i - 1, j); // 上
        dfs(board, i + 1, j); // 下
        dfs(board, i, j - 1); // 左
        dfs(board, i, j + 1); // 右
    }
}
```

### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/11/09/200px-telephone-keypad2svg.png)

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

套用回溯算法

```java
class Solution {
    // 每个数字到字母的映射
    String[] mapping = new String[] {
            "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"
    };
    List<String> res = new LinkedList<>();
    public List<String> letterCombinations(String digits) {
        if (digits.isEmpty()) {
            return res;
        }
        backtrack(digits, 0, new StringBuilder());
        return res;
    }

    public void backtrack(String digits, int start, StringBuilder sb){
        //1.递归结束条件
        if (sb.length() == digits.length()) {
            res.add(sb.toString());
            return;
        }
        //2.开始循环
        for (int i = start; i < digits.length(); i++) {
            int digit = digits.charAt(i) - '0';
            for (char c : mapping[digit].toCharArray()) {
                //3.做选择
                sb.append(c);
                //4.递归
                backtrack(digits, i+1, sb);
                //5.取消选择
                sb.deleteCharAt(sb.length() - 1);
            }
        }
    }
}
```











## 广度优先遍历（BFS）

### 基础框架

问题的本质就是让你在一幅「图」中找到从起点 `start` 到终点 `target` 的最近距离

```java
// 计算从起点 start 到终点 target 的最近距离
int BFS(Node start, Node target) {
    Queue<Node> q; // 核心数据结构
    Set<Node> visited; // 避免走回头路
    
    q.offer(start); // 将起点加入队列
    visited.add(start);
    int step = 0;

    while (q not empty) {
        int sz = q.size();
        /* 将当前队列中的所有节点向四周扩散 */
        for (int i = 0; i < sz; i++) {
            Node cur = q.poll();
            //在此处判断是否要跳过一些选择
            if (cur is 特殊情况)
                continue;
            /* 划重点：这里判断是否到达终点 */
            if (cur is target)
                return step;
            /* 将 cur 的相邻节点加入队列 */
            for (Node x : cur.adj()) {
                if (x not in visited) {
                    q.offer(x);
                    visited.add(x);
                }
            }
        }
        step++;
    }
    // 如果走到这里，说明在图中没有找到目标节点
    return 特殊值;
}
```

队列 `q` 就不说了，BFS 的核心数据结构；`cur.adj()` 泛指 `cur` 相邻的节点，比如说二维数组中，`cur` 上下左右四面的位置就是相邻节点；`visited` 的主要作用是防止走回头路，大部分时候都是必须的，但是像一般的二叉树结构，没有子节点到父节点的指针，不会走回头路就不需要 `visited`。

### 二叉树的BFS

```java
public int BFS(TreeNode root, TreeNode target) {
    if (root == null) return 0;
    Queue<TreeNode> q = new LinkedList<>(); // 核心数据结构
    //没有子节点到父节点的指针，不会走回头路就不需要 visited
    q.offer(root); // 将起点加入队列
    int step = 1;
    while (!q.isEmpty()) {
        int sz = q.size();
        /* 将当前队列中的所有节点向四周扩散 */
        for (int i = 0; i < sz; i++) {
            TreeNode cur = q.poll();
            /* 划重点：这里判断是否到达终点 */
            if (      )
                return step;
            /* 将 cur 的相邻节点加入队列 */
            if (cur.left != null)
                q.offer(cur.left);
            if (cur.right != null)
                q.offer(cur.right);
        }
        step++;
    }
    // 如果走到这里，说明在图中没有找到目标节点
    return step;
}
```

### 图的环检测算法（BFS）（力扣207）

- 统计课程安排图中每个节点的入度，生成 入度表 indegrees。
- 借助一个队列 queue，将所有入度为 0的节点入队。
- 当 queue 非空时，依次将队首节点出队，在课程安排图中删除此节点 pre：
  - 并不是真正从邻接表中删除此节点 pre，而是将此节点对应所有邻接节点 cur 的入度 −1-1−1，即 indegrees[cur] -= 1。
  - 当入度 −1-1−1后邻接节点 cur 的入度为 0，说明 cur 所有的前驱节点已经被 “删除”，此时将 cur 入队。
- 在每次 pre 出队时，执行 numCourses--；
  - 若整个课程安排图是有向无环图（即可以安排），则所有节点一定都入队并出队过，即完成拓扑排序。换个角度说，若课程安排图中存在环，一定有节点的入度始终不为 0。
  - 因此，拓扑排序出队次数等于课程个数，返回 numCourses == 0 判断课程是否可以成功安排。

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] indegrees = new int[numCourses];
        List<List<Integer>> adjacency = new ArrayList<>();
        Queue<Integer> queue = new LinkedList<>();
        for(int i = 0; i < numCourses; i++)
            adjacency.add(new ArrayList<>());
        // Get the indegree and adjacency of every course.
        for(int[] cp : prerequisites) {
            indegrees[cp[0]]++;
            adjacency.get(cp[1]).add(cp[0]);
        }
        // Get all the courses with the indegree of 0.
        for(int i = 0; i < numCourses; i++)
            if(indegrees[i] == 0) queue.add(i);
        // BFS TopSort.
        while(!queue.isEmpty()) {
            int pre = queue.poll();
            numCourses--;
            for(int cur : adjacency.get(pre))
                if(--indegrees[cur] == 0) queue.add(cur);
        }
        return numCourses == 0;
    }
}
```

### 拓扑排序（力扣210）

根据环检测方法修改

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        int[] indegrees = new int[numCourses];
        List<List<Integer>> adjacency = new ArrayList<>();
        Queue<Integer> queue = new LinkedList<>();
        for(int i = 0; i < numCourses; i++)
            adjacency.add(new ArrayList<>());
        // Get the indegree and adjacency of every course.
        for(int[] cp : prerequisites) {
            indegrees[cp[0]]++;
            adjacency.get(cp[1]).add(cp[0]);
        }
        // Get all the courses with the indegree of 0.
        for(int i = 0; i < numCourses; i++)
            if(indegrees[i] == 0) queue.add(i);
        //储存拓扑排序一个排序结果的数组
        int[] res = new int[numCourses];
        //记录进入结果的节点数量
        int count = 0;
        // BFS TopSort.
        while(!queue.isEmpty()) {
            int pre = queue.poll();
            //// 弹出节点的顺序即为拓扑排序结果
            res[count] = pre;
            count++;
            for(int cur : adjacency.get(pre))
                if(--indegrees[cur] == 0) queue.add(cur);
        }
        if(count == numCourses)
            return  res;
        else
            return new int[]{};
    }
}
```

### 判断二分图BFS（力扣785）

给你一幅「图」，请你用两种颜色将图中的所有顶点着色，且使得任意一条边的两个端点的颜色都不相同，这就是图的「双色问题」，其实这个问题就等同于二分图的判定问题，如果你能够成功地将图染色，那么这幅图就是一幅二分图，反之则不是。

判定二分图的算法很简单，就是用代码解决「双色问题」。**说白了就是遍历一遍图，一边遍历一边染色，看看能不能用两种颜色给所有节点染色，且相邻节点的颜色都不相同**。

```java
class Solution {
    // 记录图是否符合二分图性质
    private boolean ok = true;
    // 记录图中节点的颜色，false 和 true 代表两种不同颜色
    private boolean[] color;
    // 记录图中节点是否被访问过
    private boolean[] visited;

    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        color =  new boolean[n];
        visited =  new boolean[n];
        
        for (int v = 0; v < n; v++) {
            if (!visited[v]) {
                // 改为使用 BFS 函数
                bfs(graph, v);
            }
        }
        
        return ok;
    }

    // 从 start 节点开始进行 BFS 遍历
    private void bfs(int[][] graph, int start) {
        Queue<Integer> q = new LinkedList<>();
        visited[start] = true;
        q.offer(start);
        
        while (!q.isEmpty() && ok) {
            int v = q.poll();
            // 从节点 v 向所有相邻节点扩散
            for (int w : graph[v]) {
                if (!visited[w]) {
                    // 相邻节点 w 没有被访问过
                    // 那么应该给节点 w 涂上和节点 v 不同的颜色
                    color[w] = !color[v];
                    // 标记 w 节点，并放入队列
                    visited[w] = true;
                    q.offer(w);
                } else {
                    // 相邻节点 w 已经被访问过
                    // 根据 v 和 w 的颜色判断是否是二分图
                    if (color[w] == color[v]) {
                        // 若相同，则此图不是二分图
                        ok = false;
                        return;
                    }
                }
            }
        }
    }
}
```

### 可能的二分图BFS（力扣886）

给定一组 `n` 人（编号为 `1, 2, ..., n`）， 我们想把每个人分进**任意**大小的两组。每个人都可能不喜欢其他人，那么他们不应该属于同一组。给定整数 `n` 和数组 `dislikes` ，其中 `dislikes[i] = [ai, bi]` ，表示不允许将编号为 `ai` 和 `bi`的人归入同一组。当可以用这种方法将所有人分进两组时，返回 `true`；否则返回 `false`。

```java 
class Solution {
    // 记录图是否符合二分图性质
    private boolean ok = true;
    // 记录图中节点的颜色，false 和 true 代表两种不同颜色
    private boolean[] color;
    // 记录图中节点是否被访问过
    private boolean[] visited;
    public boolean possibleBipartition(int n, int[][] dislikes) {
        // 图节点编号从 1 开始
        color = new boolean[n + 1];
        visited = new boolean[n + 1];
        // 转化成邻接表表示图结构
        List<Integer>[] graph = buildGraph(n, dislikes);
        for (int v = 1; v <= n; v++) {
            if (!visited[v]) {
                bfs(graph, v);
            }
        }

        return ok;
    }

    private List<Integer>[] buildGraph(int n, int[][] dislikes) {
        // 图节点编号为 1...n
        List<Integer>[] graph = new LinkedList[n + 1];
        for (int i = 1; i <= n; i++) {
            graph[i] = new LinkedList<>();
        }
        for (int[] edge : dislikes) {
            int v = edge[1];
            int w = edge[0];
            // 「无向图」相当于「双向图」
            // v -> w
            graph[v].add(w);
            // w -> v
            graph[w].add(v);
        }
        return graph;
    }
    // 从 start 节点开始进行 BFS 遍历
    private void bfs(List<Integer>[] graph, int start) {
        Queue<Integer> q = new LinkedList<>();
        visited[start] = true;
        q.offer(start);

        while (!q.isEmpty() && ok) {
            int v = q.poll();
            // 从节点 v 向所有相邻节点扩散
            for (int w : graph[v]) {
                if (!visited[w]) {
                    // 相邻节点 w 没有被访问过
                    // 那么应该给节点 w 涂上和节点 v 不同的颜色
                    color[w] = !color[v];
                    // 标记 w 节点，并放入队列
                    visited[w] = true;
                    q.offer(w);
                } else {
                    // 相邻节点 w 已经被访问过
                    // 根据 v 和 w 的颜色判断是否是二分图
                    if (color[w] == color[v]) {
                        // 若相同，则此图不是二分图
                        ok = false;
                        return;
                    }
                }
            }
        }
    }
}
```

## 最小生成树

### Kruskal算法

Kruskal 算法也是一种贪心算法，它通过不断选择图中权重最小的边，并且不会形成环来构建最小生成树。

Kruskal 算法的基本思想是：

1. 将图中的所有边按权重从小到大进行排序。
2. 依次考虑排序后的边，如果这条边连接的两个顶点不在同一个连通分量中，则选择这条边并将它们所连接的两个连通分量合并。
3. 重复步骤 2，直到最小生成树中有 n-1 条边为止（n 为顶点数量）。

Kruskal 算法通常使用并查集（Union-Find）来维护各个连通分量，以快速判断两个顶点是否在同一连通分量中。

```java
class Solution {
    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        List<int[]> edges = new ArrayList<>();
        
        // 通过循环生成所有边及权重
        //===================================
        //for (int i = 0; i < n; i++) {
        //    for (int j = i + 1; j < n; j++) {
        //        int xi = points[i][0], yi = points[i][1];
        //        int xj = points[j][0], yj = points[j][1];
        //        // 用坐标点在 points 中的索引表示坐标点
        //        edges.add(new int[]{
        //                i, j, Math.abs(xi - xj) + Math.abs(yi - yj)
        //        });
        //    }
        //}
        //===========================================
        
        // 将边按照权重从小到大排序
        Collections.sort(edges, (a, b) -> {
            return a[2] - b[2];
        });
        // 执行 Kruskal 算法
        int mst = 0;
        UF uf = new UF(n);
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int weight = edge[2];
            // 若这条边会产生环，则不能加入 mst
            if (uf.connected(u, v)) {
                continue;
            }
            // 若这条边不会产生环，则属于最小生成树
            mst += weight;
            uf.union(u, v);
        }
        return mst;
    }

    class UF {
        // 连通分量个数
        private int count;
        // 存储一棵树
        private int[] parent;
        // 记录树的「重量」
        private int[] size;

        // n 为图中节点的个数
        public UF(int n) {
            this.count = n;
            parent = new int[n];
            size = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                size[i] = 1;
            }
        }

        // 将节点 p 和节点 q 连通
        public void union(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            if (rootP == rootQ)
                return;

            // 小树接到大树下面，较平衡
            if (size[rootP] > size[rootQ]) {
                parent[rootQ] = rootP;
                size[rootP] += size[rootQ];
            } else {
                parent[rootP] = rootQ;
                size[rootQ] += size[rootP];
            }
            // 两个连通分量合并成一个连通分量
            count--;
        }

        // 判断节点 p 和节点 q 是否连通
        public boolean connected(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            return rootP == rootQ;
        }

        // 返回节点 x 的连通分量根节点
        private int find(int x) {
            while (parent[x] != x) {
                // 进行路径压缩
                parent[x] = parent[parent[x]];
                x = parent[x];
            }
            return x;
        }

        // 返回图中的连通分量个数
        public int count() {
            return count;
        }
    }
}
```

### Prim算法

Prim 算法是一种贪心算法，它从一个单点开始，逐步将树扩展为最小生成树。时间复杂度是 `O(ElogE)`，E为一幅图边的条数。

Prim 算法的基本思想是：

1. 选择任意一个顶点作为起始点。
2. 从已选择的顶点集合出发，每次选择一条边连接已经选择的顶点和未选择的顶点集合，且这条边的权重最小。
3. 将新选择的顶点加入已选择的顶点集合，并继续重复步骤 2，直到所有顶点都被选择。

Prim 算法通常使用优先队列来存储候选边，以便快速找到权重最小的边。

```java
class Solution {
    public int PrimAlgorithm(int[][] points) {
        // 创建邻接表来表示图
        List<List<int[]>> graph = new ArrayList<>();
        // 图中顶点的数量
        int n = points.length;
        for (int i = 0; i <= n; i++) {
            graph.add(new ArrayList<>());
        }
        // 填充邻接表
        // 通过循环生成所有边及权重
        =======================================================
        //for (int i = 0; i < n; i++) {
        //    for (int j = i + 1; j < n; j++) {
        //        int xi = points[i][0], yi = points[i][1];
        //        int xj = points[j][0], yj = points[j][1];
        //        int weight = Math.abs(xi - xj) + Math.abs(yi - yj);
        //        // 用 points 中的索引表示坐标点
        //        graph.get(i).add(new int[]{j, weight});
        //        graph.get(j).add(new int[]{i, weight});
        //    }
        //}
        // 
        //for (int[] connection : connections) {
        //    int city1 = connection[0];
        //    int city2 = connection[1];
        //    int cost = connection[2];
        //    // 由于连接是双向的，因此需要在两个城市的邻接表中添加边
        //    graph.get(city1).add(new int[]{city2, cost});
        //    graph.get(city2).add(new int[]{city1, cost});
        //}
		============================================================
        // 使用 Prim 算法求解最小成本
        boolean[] visited = new boolean[n + 1]; // 记录顶点是否被访问过
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> a[1] - b[1]); // 最小堆，按照边的权重排序
        minHeap.offer(new int[]{0, 0}); // 从顶点 0 开始
        int minCost = 0; // 最小成本
        int count = 0; // 已连接顶点的数量

        while (!minHeap.isEmpty()) {
            int[] current = minHeap.poll();
            int city = current[0];
            int cost = current[1];

            if (visited[city]) continue; // 如果顶点已经被访问过，则跳过

            visited[city] = true; // 标记顶点为已访问
            minCost += cost; // 更新最小成本
            count++; // 更新已连接顶点的数量

            if (count == n) {
                return minCost; // 如果已连接所有顶点，直接返回最小成本
            }

            // 遍历当前point的邻接边
            for (int[] neighbor : graph.get(city)) {
                if (!visited[neighbor[0]]) {
                    minHeap.offer(new int[]{neighbor[0], neighbor[1]}); // 将未访问的邻接边加入最小堆
                }
            }
        }

        return -1; // 无法连接所有顶点，返回 -1
    }
}
```

### 最低成本联通所有城市（力扣1135）

**Kruskal**

每座城市相当于图中的节点，连通城市的成本相当于边的权重，连通所有城市的最小成本即是最小生成树的权重之和。将所有边按照权重从小到大排序，从权重最小的边开始遍历，如果这条边和 `mst` 中的其它边不会形成环，则这条边是最小生成树的一部分，将它加入 `mst` 集合；否则，这条边不是最小生成树的一部分，不要把它加入 `mst` 集合。

```java
class Solution {
    public int minimumCost(int n, int[][] connections) {
        // 城市编号为 1...n，所以初始化大小为 n + 1
        UnionFind uf = new UnionFind(n + 1);
        // 对所有边按照权重从小到大排序
        Arrays.sort(connections, (a, b) -> (a[2] - b[2]));
        // 记录最小生成树的权重之和
        int mst = 0;
        for (int[] edge : connections) {
            int u = edge[0];
            int v = edge[1];
            int weight = edge[2];
            // 若这条边会产生环，则不能加入 mst
            if (uf.connected(u, v)) {
                continue;
            }
            // 若这条边不会产生环，则属于最小生成树
            mst += weight;
            uf.union(u, v);
        }
        // 保证所有节点都被连通
        // 按理说 uf.count() == 1 说明所有节点被连通
        // 但因为节点 0 没有被使用，所以 0 会额外占用一个连通分量
        return uf.count() == 2 ? mst : -1;
    }

    class UnionFind {
        // 连通分量个数
        private int count;
        // 存储每个节点的父节点
        private int[] parent;

        // n 为图中节点的个数
        public UnionFind(int n) {
            this.count = n;
            parent = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        // 将节点 p 和节点 q 连通
        public void union(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);

            if (rootP == rootQ)
                return;

            parent[rootQ] = rootP;
            // 两个连通分量合并成一个连通分量
            count--;
        }

        // 判断节点 p 和节点 q 是否连通
        public boolean connected(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            return rootP == rootQ;
        }

        public int find(int x) {
            if (parent[x] != x) {
                //路径压缩
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }

        // 返回图中的连通分量个数
        public int count() {
            return count;
        }
    }
}
```

**Prim**

我们先把题目输入的 `connections` 转化成邻接表形式，然后输入给之前实现的 `Prim` 算法类即可。

关于 `buildGraph` 函数需要注意两点：

一是题目给的节点编号是从 1 开始的，所以我们做一下索引偏移，转化成从 0 开始以便 `Prim` 类使用；

二是如何用邻接表表示无向加权图，「无向图」其实就可以理解为「双向图」。

这样，我们转化出来的 `graph` 形式就和之前的 `Prim` 算法类对应了，可以直接施展 Prim 算法计算最小生成树。

```java
// 使用 Prim 算法找到连接所有城市的最低成本
public int minimumCost(int n, int[][] connections) {
    // 创建邻接表来表示图
    List<List<int[]>> graph = new ArrayList<>();
    for (int i = 0; i <= n; i++) {
        graph.add(new ArrayList<>());
    }

    // 填充邻接表
    for (int[] connection : connections) {
        int city1 = connection[0];
        int city2 = connection[1];
        int cost = connection[2];
        // 由于连接是双向的，因此需要在两个城市的邻接表中添加边
        graph.get(city1).add(new int[]{city2, cost});
        graph.get(city2).add(new int[]{city1, cost});
    }

    // 使用 Prim 算法求解最小成本
    boolean[] visited = new boolean[n + 1]; // 记录城市是否被访问过
    PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> a[1] - b[1]); // 最小堆，按照边的权重排序
    minHeap.offer(new int[]{1, 0}); // 从城市 1 开始
    int minCost = 0; // 最小成本
    int count = 0; // 已连接城市的数量

    while (!minHeap.isEmpty()) {
        int[] current = minHeap.poll();
        int city = current[0];
        int cost = current[1];

        if (visited[city]) continue; // 如果城市已经被访问过，则跳过

        visited[city] = true; // 标记城市为已访问
        minCost += cost; // 更新最小成本
        count++; // 更新已连接城市的数量

        if (count == n) {
            return minCost; // 如果已连接所有城市，直接返回最小成本
        }

        // 遍历当前城市的邻接边
        for (int[] neighbor : graph.get(city)) {
            if (!visited[neighbor[0]]) {
                minHeap.offer(new int[]{neighbor[0], neighbor[1]}); // 将未访问的邻接边加入最小堆
            }
        }
    }

    return -1; // 无法连接所有城市，返回 -1
}
```

### 连接所有点的最小费用（力扣1584）

**Kruskal**

每个坐标点是一个二元组，那么按理说应该用五元组表示一条带权重的边，但这样的话不便执行 Union-Find 算法；所以我们用 `points` 数组中的索引代表每个坐标点，这样就可以直接复用之前的 Kruskal 算法逻辑了。

```java
class Solution {
    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        // 生成所有边及权重
        List<int[]> edges = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int xi = points[i][0], yi = points[i][1];
                int xj = points[j][0], yj = points[j][1];
                // 用坐标点在 points 中的索引表示坐标点
                edges.add(new int[]{
                        i, j, Math.abs(xi - xj) + Math.abs(yi - yj)
                });
            }
        }
        // 将边按照权重从小到大排序
        Collections.sort(edges, (a, b) -> {
            return a[2] - b[2];
        });
        // 执行 Kruskal 算法
        int mst = 0;
        UF uf = new UF(n);
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int weight = edge[2];
            // 若这条边会产生环，则不能加入 mst
            if (uf.connected(u, v)) {
                continue;
            }
            // 若这条边不会产生环，则属于最小生成树
            mst += weight;
            uf.union(u, v);
        }
        return mst;
    }

    class UF {
        // 连通分量个数
        private int count;
        // 存储一棵树
        private int[] parent;
        // 记录树的「重量」
        private int[] size;

        // n 为图中节点的个数
        public UF(int n) {
            this.count = n;
            parent = new int[n];
            size = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                size[i] = 1;
            }
        }

        // 将节点 p 和节点 q 连通
        public void union(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            if (rootP == rootQ)
                return;

            // 小树接到大树下面，较平衡
            if (size[rootP] > size[rootQ]) {
                parent[rootQ] = rootP;
                size[rootP] += size[rootQ];
            } else {
                parent[rootP] = rootQ;
                size[rootQ] += size[rootP];
            }
            // 两个连通分量合并成一个连通分量
            count--;
        }

        // 判断节点 p 和节点 q 是否连通
        public boolean connected(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            return rootP == rootQ;
        }

        // 返回节点 x 的连通分量根节点
        private int find(int x) {
            while (parent[x] != x) {
                // 进行路径压缩
                parent[x] = parent[parent[x]];
                x = parent[x];
            }
            return x;
        }

        // 返回图中的连通分量个数
        public int count() {
            return count;
        }
    }
}
```

**Prim**

```java
class Solution {
    public int minCostConnectPoints(int[][] points) {
        // 创建邻接表来表示图
        List<List<int[]>> graph = new ArrayList<>();
        // 图中顶点的数量
        int n = points.length;
        for (int i = 0; i <= n; i++) {
            graph.add(new ArrayList<>());
        }
        // 填充邻接表
        // 生成所有边及权重
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int xi = points[i][0], yi = points[i][1];
                int xj = points[j][0], yj = points[j][1];
                int weight = Math.abs(xi - xj) + Math.abs(yi - yj);
                // 用 points 中的索引表示坐标点
                graph.get(i).add(new int[]{j, weight});
                graph.get(j).add(new int[]{i, weight});
            }
        }

        // 使用 Prim 算法求解最小成本
        boolean[] visited = new boolean[n + 1]; // 记录point是否被访问过
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> a[1] - b[1]); // 最小堆，按照边的权重排序
        minHeap.offer(new int[]{0, 0}); // 从point 0 开始
        int minCost = 0; // 最小成本
        int count = 0; // 已连接point的数量

        while (!minHeap.isEmpty()) {
            int[] current = minHeap.poll();
            int city = current[0];
            int cost = current[1];

            if (visited[city]) continue; // 如果point已经被访问过，则跳过

            visited[city] = true; // 标记point为已访问
            minCost += cost; // 更新最小成本
            count++; // 更新已连接point的数量

            if (count == n) {
                return minCost; // 如果已连接所有point，直接返回最小成本
            }

            // 遍历当前point的邻接边
            for (int[] neighbor : graph.get(city)) {
                if (!visited[neighbor[0]]) {
                    minHeap.offer(new int[]{neighbor[0], neighbor[1]}); // 将未访问的邻接边加入最小堆
                }
            }
        }

        return -1; // 无法连接所有point，返回 -1
    }
}
```

## 	滑动窗口

### 滑动窗口算法框架

```java
/* 滑动窗口算法框架 */
void slidingWindow(String s) {
    // 用合适的数据结构记录窗口中的数据
    HashMap<Character, Integer> window = new HashMap<>();

    int left = 0, right = 0;
    while (right < s.length()) {
        // c 是将移入窗口的字符
        char c = s.charAt(right);
        // 增大窗口
        right++;
        // -----------进行窗口内数据的一系列更新-------------------
        
        if(满足扩大窗口的条件)//可有可无
            //window中若没有c则把c的value设置为0, 若有c则把c的value设置为原有值上+1
            window.put(c, window.getOrDefault(c, 0) + 1);
            ...
            /*** debug 输出的位置 ***/
            System.out.printf("window: [%d, %d)\n", left, right);
        
		// ------------------------------------------------------
        
        // 判断左侧窗口是否要收缩
        while ( window needs shrink) {
            // d 是将移出窗口的字符
            char d = s.charAt(left);
            left++;
            // -----------进行窗口内数据的一系列更新-------------------
            
            if(满足缩小窗口的条件)//可有可无
                window.put(d, window.get(d) - 1);
                // 缩小窗口
            	...
            // ------------------------------------------------------
        }
    }
}
```

其中两处 `...` 表示的更新窗口数据的地方，到时候你直接往里面填就行了。而且，这两个 `...` 处的操作分别是扩大和缩小窗口的更新操作，它们操作是完全对称的。

### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

> 给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。
>
> **示例 1:**
>
> ```
> 输入: s = "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
> ```

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character,Integer> window = new HashMap<>();
        int res = 0;
        //初始化滑动窗口
        int left = 0, right = 0;
        //开始向右滑动
        while (right < s.length()) {
            char r = s.charAt(right);
            //扩大窗口右边界，将元素放到窗口，并更新map[right] + 1
            window.put(r, window.getOrDefault(r, 0) + 1);
            right++;
            //出现不符合条件的元素，减小窗口
            while (window.get(r) > 1) {
                //减小窗口左边界,更新map[left] - 1, 直到满足条件
                char l = s.charAt(left);
                window.put(l, window.get(l) - 1);
                left++;
            }
            //更新结果
            res = Math.max(res, right -left);
        }
        return res;
    }
}
```



## 动态规划

### 股票买卖问题

给你一个整数数组 `prices` 和一个整数 `k` ，其中 `prices[i]` 是某支给定的股票在第 `i` 天的价格。设计一个算法来计算你所能获取的最大利润。你最多可以完成 `k` 笔交易。也就是说，你最多可以买 `k` 次，卖 `k` 次。注意：不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

这个问题的「状态」有三个，第一个是天数，第二个是允许交易的最大次数，第三个是当前的持有状态（即之前说的 `rest` 的状态，我们不妨用 1 表示持有，0 表示没有持有）。然后我们用一个三维数组就可以装下这几种状态的全部组合：

```python
dp[i][k][0 or 1]
0 <= i <= n - 1, 1 <= k <= K
n 为天数，大 K 为交易数的上限，0 和 1 代表是否持有股票。
此问题共 n × K × 2 种状态，全部穷举就能搞定。

for 0 <= i < n:
    for 1 <= k <= K:
        for s in {0, 1}:
            dp[i][k][s] = max(buy, sell, rest)
```

我们想求的最终答案是 `dp[n - 1][K][0]`，即最后一天，最多允许 `K` 次交易，最多获得多少利润。

<img src = "Algorithmimages\645645453.png">

通过这个图可以很清楚地看到，每种状态（0 和 1）是如何转移而来的。根据这个图，我们来写一下状态转移方程：

```python
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
              max( 今天选择 rest,        今天选择 sell       )
```

解释：今天我没有持有股票，有两种可能，我从这两种可能中求最大利润：

1、我昨天就没有持有，且截至昨天最大交易次数限制为 `k`；然后我今天选择 `rest`，所以我今天还是没有持有，最大交易次数限制依然为 `k`。

2、我昨天持有股票，且截至昨天最大交易次数限制为 `k`；但是今天我 `sell` 了，所以我今天没有持有股票了，最大交易次数限制依然为 `k`。

```python
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
              max( 今天选择 rest,         今天选择 buy         )
```

解释：今天我持有着股票，最大交易次数限制为 `k`，那么对于昨天来说，有两种可能，我从这两种可能中求最大利润：

1、我昨天就持有着股票，且截至昨天最大交易次数限制为 `k`；然后今天选择 `rest`，所以我今天还持有着股票，最大交易次数限制依然为 `k`。

2、我昨天本没有持有，且截至昨天最大交易次数限制为 `k - 1`；但今天我选择 `buy`，所以今天我就持有股票了，最大交易次数限制为 `k`。

这里着重提醒一下，时刻牢记「状态」的定义，**状态 `k` 的定义并不是「已进行的交易次数」，而是「最大交易次数的上限限制」**。如果确定今天进行一次交易，且要保证截至今天最大交易次数上限为 `k`，那么昨天的最大交易次数上限必须是 `k - 1`。

**每次选择`buy`则表示一次交易开始，选择`sell`则表示一次交易结束**

定义 base case，即最简单的情况。

```java
dp[-1][...][0] = 0
解释：因为 i 是从 0 开始的，所以 i = -1 意味着还没有开始，这时候的利润当然是 0。

dp[-1][...][1] = -infinity
解释：还没开始的时候，是不可能持有股票的。
因为我们的算法要求一个最大值，所以初始值设为一个最小值，方便取最大值。

dp[...][0][0] = 0
解释：因为 k 是从 1 开始的，所以 k = 0 意味着根本不允许交易，这时候利润当然是 0。

dp[...][0][1] = -infinity
解释：不允许交易的情况下，是不可能持有股票的。
因为我们的算法要求一个最大值，所以初始值设为一个最小值，方便取最大值。
```

#### base case 以及状态转移方程

```java
base case：
dp[-1][...][0] = dp[...][0][0] = 0
dp[-1][...][1] = dp[...][0][1] = -infinity
状态转移方程：
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
           // max( 今天选择 rest,        今天选择 sell       )
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
           // max( 今天选择 rest,         今天选择 buy         )
```

#### 交易次数k=1

```java
public int maxProfit(int[] prices) {
    int n = prices.length;
    int[][] dp = new int[n][2];
    for (int i = 0; i < n; i++) {
        if (i = 0) {
            // base case
            dp[i][0] = 0;
            dp[i][1] = -prices[i];
            continue;
        }
        //状态转移方程
        dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
        dp[i][1] = Math.max(dp[i-1][1], -prices[i]);
    }
    return dp[n - 1][0];
}
```

#### 不限制交易次数k=+∞

```java
public int maxProfit(int[] prices) {
    int n = prices.length;
    int[][] dp = new int[n][2];
    for (int i = 0; i < n; i++) {
        if (i = 0) {
            // base case
            dp[i][0] = 0;
            dp[i][1] = -prices[i];
            continue;
        }
        //状态转移方程
        dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
        dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0] - prices[i]);
    }
    return dp[n - 1][0];
}
```

#### 不限制交易次数k=+∞，且有冷静期cooldown

```java
public int cooldown(int[] prices, int cooldown) {
    int n = prices.length;
    int[][] dp = new int[n][2];
    for (int i = 0; i < n; i++) {
        if (i == 0) {
            // base case 1
            dp[i][0] = 0;
            dp[i][1] = -prices[i];
            continue;
        }
        if (i - cooldown - 1 < 0) {
            // base case 2
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i-1][1], -prices[i]);
            continue;
        }
        //状态转移方程
        dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
        dp[i][1] = Math.max(dp[i - 1][1], dp[i - cooldown - 1][0] - prices[i]);
    }
    return dp[n - 1][0];
}
```

#### 不限制交易次数k=+∞，交易包含手续费fee

```java
public int maxProfit(int[] prices, int fee) {
    int n = prices.length;
    int[][] dp = new int[n][2];
    for (int i = 0; i < n; i++) {
        if (i == 0) {
            // base case
            dp[i][0] = 0;
            dp[i][1] = -prices[i];
            continue;
        }
        //状态转移方程
        dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i] - fee);
        //每当选择sell卖出股票时，才代表一次交易结束，扣除手续费
        dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0] - prices[i]);
    }
    return dp[n - 1][0];
}
```

#### 交易次数k=任意整数时

```java
public int maxProfit(int[] prices) {
    int n = prices.length;
    int[][][] dp = new int[n][k+1][2];
    for (int i = 0; i < n; i++) {
        for (int j = 1; j < k+1; j++) {
            if (i == 0) {
                // base case
                dp[i][j][0] = 0;
                dp[i][j][1] = -prices[i];
                continue;
            }
            //状态转移方程
            dp[i][j][0] = Math.max(dp[i-1][j][0], dp[i-1][j][1] + prices[i]);
            dp[i][j][1] = Math.max(dp[i-1][j][1], dp[i-1][j-1][0] - prices[i]);
        }
    }
    return dp[n - 1][k][0];
}
```

#### 交易次数k=max_k，冷静期cooldown，手续费fee

输入股票价格数组 `prices`，你最多进行 `max_k` 次交易，每次交易需要额外消耗 `fee` 的手续费，而且每次交易之后需要经过 `cooldown` 天的冷冻期才能进行下一次交易，请你计算并返回可以获得的最大利润。

```java
// 同时考虑交易次数的限制、冷冻期和手续费
int maxProfit_all_in_one(int max_k, int[] prices, int cooldown, int fee) {
    int n = prices.length;
    if (n <= 0) {
        return 0;
    }
    if (max_k > n / 2) {
        // 交易次数 k 没有限制的情况
        return maxProfit_k_inf(prices, cooldown, fee);
    }

    int[][][] dp = new int[n][max_k + 1][2];
    // k = 0 时的 base case
    for (int i = 0; i < n; i++) {
        dp[i][0][1] = Integer.MIN_VALUE;
        dp[i][0][0] = 0;
    }

    for (int i = 0; i < n; i++) 
        for (int k = max_k; k >= 1; k--) {
            if (i - 1 == -1) {
                // base case 1
                dp[i][k][0] = 0;
                dp[i][k][1] = -prices[i] - fee;
                continue;
            }

            // 包含 cooldown 的 base case
            if (i - cooldown - 1 < 0) {
                // base case 2
                dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
                // 别忘了减 fee
                dp[i][k][1] = Math.max(dp[i-1][k][1], -prices[i] - fee);
                continue;
            }
            dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
            // 同时考虑 cooldown 和 fee
            dp[i][k][1] = Math.max(dp[i-1][k][1], dp[i-cooldown-1][k-1][0] - prices[i] - fee);     
        }
    return dp[n - 1][max_k][0];
}

// k 无限制，包含手续费和冷冻期
int maxProfit_k_inf(int[] prices, int cooldown, int fee) {
    int n = prices.length;
    int[][] dp = new int[n][2];
    for (int i = 0; i < n; i++) {
        if (i - 1 == -1) {
            // base case 1
            dp[i][0] = 0;
            dp[i][1] = -prices[i] - fee;
            continue;
        }

        // 包含 cooldown 的 base case
        if (i - cooldown - 1 < 0) {
            // base case 2
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
            // 别忘了减 fee
            dp[i][1] = Math.max(dp[i-1][1], -prices[i] - fee);
            continue;
        }
        dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
        // 同时考虑 cooldown 和 fee
        dp[i][1] = Math.max(dp[i - 1][1], dp[i - cooldown - 1][0] - prices[i] - fee);
    }
    return dp[n - 1][0];
}
```

### 多维动态规划

#### [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)

> 给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列** ，返回 `0` 。
>
> 一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
>
> - 例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。
>
> 两个字符串的 **公共子序列** 是这两个字符串所共同拥有的子序列。
>
> **示例 1：**
>
> ```
> 输入：text1 = "abcde", text2 = "ace" 
> 输出：3  
> 解释：最长公共子序列是 "ace" ，它的长度为 3 。
> ```



```java
class Solution {
    //备忘录
    int[][] memo;
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        //1.初始化备忘录 -1代表未计算过
        memo = new int[m + 1][n + 1];
        for (int i = 0; i < memo.length; i++) {
            Arrays.fill(memo[i], -1);
        }
        //2.计算text1[0...]和text2[0...]的最长公共子序列长度
        return dp(text1, 0, text2, 0);
    }
    //方法功能：计算text1[i...]和text2[j...]的最长公共子序列长度
    public int dp(String text1, int i, String text2, int j)
    {
        //1.base case
        if (i == text1.length() || j == text2.length()) {
            return 0;
        }
        //2.如果备忘录中存在则直接返回
        if (memo[i][j] != -1) {
            return memo[i][j];
        }
        //3.text1[i]和text2[j]一定在最长公共子序列中
        if (text1.charAt(i) == text2.charAt(j)) {
            memo[i][j] = 1 + dp(text1, i + 1, text2, j + 1); 
        }
        //4.text1[i]和text2[j]至少有一个在最长公共子序列中
        else{
            memo[i][j] = Math.max(dp(text1, i, text2, j + 1), dp(text1, i + 1, text2, j));
            //text1[i]和text2[j]都不在最长公共子序列中 不必考虑 一定比上述情况小
            //dp(text1, i + 1, text2, j + 1)          
        }
        return memo[i][j];
    }
}
```

# SQL语句

### 连接

| 连接类型                                 | 说明                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| INNER JOIN 内连接                        | （默认连接方式）只有当两个表都存在满足条件的记录时才会返回行。 |
| LEFT JOIN / LEFT OUTER JOIN 左(外)连接   | 返回左表中的所有行，即使右表中没有满足条件的行也是如此。     |
| RIGHT JOIN / RIGHT OUTER JOIN 右(外)连接 | 返回右表中的所有行，即使左表中没有满足条件的行也是如此。     |
| FULL JOIN / FULL OUTER JOIN 全(外)连接   | 只要其中有一个表存在满足条件的记录，就返回行。               |
| SELF JOIN                                | 将一个表连接到自身，就像该表是两个表一样。为了区分两个表，在 SQL 语句中需要至少重命名一个表。 |
| CROSS JOIN                               | 交叉连接，从两个或者多个连接表中返回记录集的笛卡尔积。       |

下图展示了 LEFT JOIN、RIGHT JOIN、INNER JOIN、OUTER JOIN 相关的 7 种用法。

![img](https://oss.javaguide.cn/p3-juejin/701670942f0f45d3a3a2187cd04a12ad~tplv-k3u1fbpfcp-zoom-1.png)

如果不加任何修饰词，只写 `JOIN`，那么默认为 `INNER JOIN`

对于 `INNER JOIN` 来说，还有一种隐式的写法，称为 “**隐式内连接**”，也就是没有 `INNER JOIN` 关键字，使用 `WHERE` 语句实现内连接的功能







### DATEDIFF 函数

认识一下 DATEDIFF 函数，可以计算两者的日期差

DATEDIFF('2007-12-31','2007-12-30');   # 1
DATEDIFF('2010-12-30','2010-12-31');   # -1

```java
SELECT b.Id
FROM Weather as a,Weather as b
WHERE a.Temperature < b.Temperature and DATEDIFF(a.RecordDate,b.RecordDate) = -1;
```

### DATE_FORMAT(date, format) 

用于以不同的格式显示日期/时间数据。date 参数是合法的日期，format 规定日期/时间的输出格式。

数据表中的 trans_date 是精确到日，我们可以使用 DATE_FORMAT() 函数将日期按照年月 %Y-%m 输出。比如将 2019-01-02 转换成 2019-01 。

```mysql
DATE_FORMAT(trans_date, '%Y-%m')
```





### round()函数

round函数用于数据的四舍五入，它有两种形式：

1、round(x,d) ，x指要处理的数，d是指保留几位小数

这里有个值得注意的地方是，d可以是负数，这时是指定小数点左边的d位整数位为0,同时小数位均为0；

2、round(x) ,其实就是round(x,0),也就是默认d为0；

### IFNULL 函数

在mysql中IFNULL() 函数用于判断第一个表达式是否为 NULL，如果第一个值不为NULL就执行第一个值。第一个值为 NULL 则返回第二个参数的值。

### avg()函数

avg（条件）相当于sum（if（条件，1，0））/count(全体)

### count函数

count( if (条件,1,**null**)

count(distinct subject_id)









