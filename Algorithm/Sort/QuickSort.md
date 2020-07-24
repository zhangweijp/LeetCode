# 快速排序
## 算法思想
我们从数组Array中选出一个元素作为轴pivot，我们使用双指针，左指针left从数组的左端向右遍历找到大于pivot的元素，同时右指针right从数组的右端开始想做遍历找到小于pivot的元素，将left、right指向的元素进行交换，继续遍历、交换，直到left指针和right指针相遇，相遇点的左边都是小于等于pivot的元素，而相遇点的边都是大于pivot的元素，我们将相遇点右边的第一个大于pivot的元素与处在数组末端的pivot进行交换，就完成了一次排序。我们再使用相同的流程对pivot左边的区间和pivot右边的区间进行排序，直到区间长度为1，我们就对整个数组完成了排序。
## Java代码
```java
public class QuickSort {
    public void quickSort(int[] nums, int l, int r) {
        if (r <= l)
            return;
        //这里默认选用 每次快排的右端点作为pivot
        int left = l;
        int right = r-1;
        int pivot = nums[r];
        //通过一次快排 我们要将数组中小于等于pivot的元素 都移动到pivot的左边
        //将数组中大于pivot的元素都移动到pivot的右边
        //nums[left] <= pivot
        //nums[right] > pivot
        while (left <= right) {
            //left <= right
            //当left和right相遇后，即left==right时，left还会向右移动一次，
            //那么此时left所处的位置就是pivot的位置
            while (left <= right && nums[left] <= pivot)
                left++;
            while (left <= right && nums[right] > pivot)
                right--;
            if (left < right)
                swap(nums,left,right);
        }

        swap(nums,left,r);
        quickSort(nums,l,left-1);
        quickSort(nums,left+1,r);
    }

    public void swap(int[] nums, int a, int b) {
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }

    public static void main(String[] args) {
        int[] nums = {6,5,3,2,1,7};
        new QuickSort().quickSort(nums,0,nums.length-1);
        for (int i = 0; i < nums.length; i++) {
            System.out.print(nums[i]+" ");
        }

    }
```