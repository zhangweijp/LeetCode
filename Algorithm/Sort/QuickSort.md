# 快速排序
## Java代码
```java
public class QuickSort {
    public void quickSort(int[] nums, int l, int r) {
        if (r <= l)
            return;

        int left = l;
        int right = r-1;
        int pivot = nums[r];
        while (left <= right) {
            //nums[left] <= pivot
            //nums[right] > pivot
            //这两行判断代码的作用
            //可以将left定位到pivot区间右边后一个位置上
            //同样的操作 在二分法里也有
            
            //left <= right
            //保证 left和right在相遇后可以停止循环
            while (left <= right && nums[left] <= pivot)
                left++;
            while (left <= right && nums[right] > pivot)
                right--;
            if (left < right){
                swap(nums,left,right);
            }
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