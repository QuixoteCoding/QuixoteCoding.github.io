---
title: first article
date: 2021-03-13 20:43:43
tags:
---

# 排序Series

### 快速排序

**原理：**取数组中随机一个数作为标准数，分别将小于、等于、大于它的数放在数组的左中右，然后再分别将大于区和小于区做同样的操作，直到大于小于区的大小都等于1。

```java
代码细节：
// 1.主要方法quickSort的参数是arr[]数组，l左索引，r右索引
quickSort(int[] arr, int l, int r)
// 2.递归出口条件是左索引l小于右索引r
if(l < r){}
// 3.获得排序区间中的一个随机位置的数，将它和r索引的交换
swap(arr, r, l + (int) Math.random() * (r - l + 1))
// 4.partition方法得到的数组保存了相等区的开始和结束索引，所以递归函数的参数要注意
quickSort(arr, l, p[0] - 1)
quickSort(arr, p[1] + 1, r)
// 5.partition方法中less和more的定义注意
int less = l - 1
int more = r
// 6.partition方法while循环条件：l是一直变化的遍历索引，碰到more区就停下
while (l < more) {}
// 7.less区和more区的扩张规则：arr[r]保存的就是标准数，所以要用arr[l]和它比较
 if (arr[l] < arr[r]) {
     // 7.1.less区扩张，注意前++
     swap(arr, ++less, l++);
 } else if (arr[l] > arr[r]) {
     // 7.2.more区扩张，注意前--，l不++
     swap(arr, --more, l);
 } else {
     // 7.3.相等，l直接++
      l++;
 }
// 8.标准数归位
swap(arr, r, more)
// 9.创建返回的p数组，more已经是交换好的标准数，less是小于区结束索引需要+1
return new int[]{less + 1, more}
```

### 归并排序

**原理：**将数组不断地中间分开，直到分出只剩两个，然后使用一个辅助数组，从最小的小数组依次排序。

```java
代码细节：
// 1.确定递归出口
if (l == r) return;
// 2.二分数组并调用递归函数
int mid = (l + r) / 2;
mergeSort(arr, l, mid);
mergeSort(arr, mid + 1, r);
// 3.主要逻辑，借助辅助数组比较并再填回原数组
merge(arr, l, mid, r);
// 4.merger方法中创建辅助数组help[]
int[] help = new int[r - l + 1];
// 5.定义三个索引下标
// 5.1 辅助数组索引i
// 5.2 左部分数组索引p1，从l开始
// 5.3 右部分数组索引p2，从mid + 1开始
int i = 0, p1 = l, p2 = mid + 1;
// 6.循环条件：左右部分索引都没有用完
while (p1 <= mid && p2 <= r) {
// 7.将较小的先加入辅助数组
    help[i++] = arr[p1] < arr[p2] ? arr[p1++] : arr[p2++];
}
// 8.将没有用完的一部分剩下的都加入辅助数组
while (p1 <= mid) {
    help[i++] = arr[p1++];
}
while (p2 <= r) {
    help[i++] = arr[p2++];
}
// 9.将辅助数组中的数据重新填入原数组
for (int k = 0; k < help.length; k++) {
    //加入新数组时，索引从l开始
    arr[l] = help[k];
    l++;
}
```

### 堆排序

**原理：**先将数组一个数一个数地调整为大根堆，然后将堆顶元素和最后一个值交换，分离出最后一个值，然后再调整大根堆。

```java
代码细节：
// 1.一个一个的将数组调整为大根堆
for (int i = 0; i < arr.length; i++) {
    heapInsert(arr, i);
}
// 2.heapInsert方法调整为大根堆，具体操作就是调整当前节点和父节点的位置关系，然后向上调整
// 向上调整这一点很重要
private static void heapInsert(int[] arr, int index) {
    // 循环条件：当前值大于父节点
    while (arr[index] > arr[(index - 1) / 2]) {
        // 交换当前位置和父节点
        swap(arr, index, (index - 1) / 2);
        //  index变成它的父节点
        index = (index - 1) / 2;
    }
}
// 3.size表示每次取出的堆顶元素的保存位置
int size = arr.length;
// 4.将堆顶元素保存到数组尾部(重点--size)
swap(arr, 0, --size);
// 5.在size减完之前，一直重复调整->取出堆顶操作
while (size > 0) {
    heapify(arr, 0, size);
    // 5.1不要忘了在循环中取出
    swap(arr, 0, --size);
}
// 6.由上到下的调整
private static void heapify(int[] arr, int index, int size) {
    int left = index * 2 + 1;
    while (left < size) {
        // 找到左右子树中较大的一个的索引
        int largest =
            left + 1 < size && arr[left + 1] > arr[left] ? left + 1 : left;
        // 找到父左右三者中最大的索引
        largest = arr[largest] > arr[index] ? largest : index;
        // 如果当前index就是最大的，就不需要执行下面的操作
        if (index == largest) break;
        // 把最大的调整到index的位置
        swap(arr, index, largest);
        // 开始处理被调换的位置
        index = largest;
        // 不要忘记更新left
        left = index * 2 + 1;
    }
}
```

# Didn't Code

### LRU缓存

- #### 功能：

  1. set(key, value) 插入记录
  2. get(key) 返回key对应的value

- #### 要求：

  1. set和get方法的复杂度均为O(1)
  2. 当某个key的set或get操作一旦发生，便认为这个key是最近使用的
  3. 当缓存的大小超过构造方法中确定的size时，移除离上次使用时间最长的键值对

- #### 实现思路

  - 使用哈希表+双向链表实现

  - 双向链表中的节点类型为自定义的Node类型，链表中**尾部是最近使用过的节点**，大小超过size是要删除头节点。
  - Node节点本身就要维护last和next两个指针
  - 缓存结构中包含了两个Map，分别是key->Node和Node->key
  - key查Node哈希表用于get操作
  - Node查key哈希表用于remove操作。双向链表中的remove方法会返回被删除的节点，根据这个节点拿到他的key，然后在两个哈希表中把这个节点删除。

### LFU缓存

- #### 功能：

  1. set(key, value) 插入记录
  2. get(key) 返回key对应的value

- #### 要求：

  1. set和get方法的复杂度均为O(1)
  2. 容量不足时淘汰访问次数最少的，次数相同时则淘汰最不常访问的

- #### 实现思路

  - 使用双向链表，每个节点下挂一个双向链表实现
  - 大链表中的节点表示操作次数，每个次数节点下挂的一个由旧到新排序节点的双向链表

  - Node中包含了key, value, times, Node up, Node down
  - 待补充

# 迭代遍历二叉树

### 先序遍历

- 为什么使用栈来辅助？二叉树本身只包含了向下的指针，但是我们在遍历过程中需要有返回父节点的操作，栈就可以帮助我们保存我们需要的数据，它的先进后出的特点，可以实现返回父节点。

 ```java
代码细节：
public static void preOrder(Node head) {
    if (head != null) {
        // 1.先将head加入stack中
        Stack<Node> stack = new Stack<>();
        stack.add(head);
        // 2.遍历stack
        while (!stack.isEmpty()) {
            // 3.将head弹出并输出
            head = stack.pop();
            System.out.println(head.value + " ");
            // 4.按顺序将右节点，左节点加入stack
            // 因为先序遍历是先左后右
            if (head.right != null) {
                stack.push(head.right);
            }
            if (head.left != null) {
                stack.push(head.left);
            }
        }
    }
}
 ```

### 中序遍历

 ```java
代码细节：
public static void inOrder(Node head) {
    if (head != null) {
        Stack<Node> stack = new Stack<>();
        // 1.注意循环条件有两个
        while (!stack.isEmpty() || head != null) {
            // 2.先将所有的左节点加入栈
            if (head != null) {
                stack.push(head);
                head = head.left;
            } else {
                // 3.弹出并打印最左的节点
                head = stack.pop();
                System.out.println(head.value);
                // 到最底层的left时，他的right也是null，所以会再次进入else，弹出上一个根节点
                // 这句很重要！
                head = head.right;
            }
        }
    }
}
 ```

### 后序遍历

 ```java
代码细节：
public static void pastOrder(Node head) {
    if (head != null) {
        // 1.准备两个stack
        // s1用来中左右遍历
        Stack<Node> s1 = new Stack<>();
        // s2用来逆序s1
        Stack<Node> s2 = new Stack<>();
        // 2.相比先序遍历将左右子节点的顺序反过来
        s1.push(head);
        while (!s1.isEmpty()) {
            head = s1.pop();
            // 3.中加入s2
            s2.push(head);
            // 4.左加入s2
            if (head.left != null) {
                s1.push(head.left);
            }
            // 5.右加入s2
            if (head.right != null) {
                s1.push(head.right);
            }
        }
        // 6.逆序输出s2
        while (!s2.isEmpty()) {
            System.out.println(s2.pop().value + " ");
        }
    }
}
 ```

# 回溯算法

### 全排列#46

```java
public class Permute {
    public static List<List<Integer>> permute(int[] nums) {
        int len = nums.length;
        // 1.res结果数组
        List<List<Integer>> res = new ArrayList<>();
        if (len == 0) return res;
        // 2.使用stack保存每一种结果
        Stack<Integer> path = new Stack<Integer>();
        // 3.使用一个和原数组等长的布尔数组，保存每一个元素是否使用过
        boolean[] used = new boolean[len];
        dfs(nums, len, 0, path, used, res);
        return res;
    }
	/**
	* params：
	* 原数组nums, 数组长度len, 布尔数组used, 结果集合res
	* 递归深度depth: 用来控制递归层次以及出口，重要！
	* 单次结果：path
	*/
    private static void dfs(int[] nums, int len, int depth, Stack<Integer> path, boolean[] used, List<List<Integer>> res) {
        // 4.用深度判断递归出口
        if (depth == len) {
            // 5.要添加新创建的ArrayList
            res.add(new ArrayList<>(path));
        } else {
            // 6.遍历数组元素
            for (int i = 0; i < len; i++) {
                // 7.先判断是否用过
                if (used[i]) continue;
                // 8.加入单次结果
                path.push(nums[i]);
                // 9.当前元素被使用过
                used[i] = true;
                // 10.在当前元素被使用情况下开始下一个元素
                dfs(nums, len, depth + 1, path, used, res);
                // 11.回溯当前的选择
                used[i] = false;
                path.pop();
            }
        }

    }
}
```