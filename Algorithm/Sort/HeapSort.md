# 堆排序
- 堆是一棵完全二叉树，且其中的每个父节点都大于其子节点；
- 如何构建一个大根堆呢，我们先从下至上，从右至左遍历整颗树，对每个节点都进行heapify操作，而heapify操作时从上至下进行，知道我们遍历到根节点，我们就完整的建好了一个堆，这里我们选择用数组模拟一棵完全二叉树；
- 如何排序呢？我们交换堆的根节点和最右的叶子节点，再将根节点删除，直到整个堆的元素均被移除，我们就能得到一个排好序的数组。
## Heapify
    我们找到当前节点和其孩子节点中最大的节点，将其与根节点进行交换，然后在对交换的节点进行相同的操作，直到遍历到叶子节点结束。
### Heapify java代码
```java
public void heapify(int[] tree, int i, int n) {
    //记录最大节点的坐标
    int max = i;
    //孩子节点
    int c1 = 2*i + 1;
    int c2 = 2*i + 2;
    //先判断孩子节点是否存在 在判断其值是否大于tree[max]
    if (c1 < n && tree[c1] > tree[max]) 
        max = c1;
    
    //同上
    if (c2 < n && tree[c2] > tree[max]) 
        max = c2;
    
    if (i != max) {
        swap(tree, i, max);
        heapify(tree, max, n);
    }
}
```
## BuildHeap
我们从下向上，从右至左，依次对每个父节点进行heapify，这样就能构建一个堆。
### BuildTree java代码
```java
public void buildHeap(int[] tree, int n) {
    int lastNode = n-1;
    int lastParent = (lastNode - 1)/2;
    for (int i = lastParent; i >= 0; i--) {
        heapify(tree, i, n);
    }
}
```
## HeapSort
    我们交换堆的根节点和最右的叶子节点，再将根节点删除，直到整个堆的元素均被移除，我们就能得到一个排好序的数组。
### HeapSort java代码
```java
public void heapSort(int[] tree, int n) {
    buildHeap(tree, n);
    for (int i = n-1; i >= 0; i--) {
        swap(tree, 0, i);
        //堆的结构被破坏了， 所以我们要重新建堆
        heapify(tree, 0, i);
    }
}
```