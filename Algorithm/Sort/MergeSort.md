# 归并排序
## 算法思想
首先需要了解merge操作，即对两个有序的数组进行合并，使其合并为一个有序数组。我们不断对数组进行平分，对细分后的小数组进行排序，然后将两部分进行合并，再返回到上一层，在进行合并，直到完成对整个数组的排序。
## Java代码
```java
public class MergeSort {
    //自底向上的归并排序
    public void merge(int[] nums ,int left, int mid, int right) {
        //temp数组存放 left->right区间内的所有元素
        int[] temp = new int[right - left +1];
        for (int k = left; k <= right; k++) {
            temp[k-left] = nums[k];
        }

        mid -= left;
        right -= left;

        int count = left;
        int i = 0,j = mid;
        //对nums数组左半区间和右半区间进行合并
        while (i < mid && j <= right) {
            if (temp[i] > temp[j]) {
                nums[count++] = temp[j++];
            }else {
                nums[count++] = temp[i++];
            }
        }
        while (i < mid)
            nums[count++] = temp[i++];
        while (j <= right)
            nums[count++] = temp[j++];
    }

    public void mergeSort(int[] nums, int left, int right) {
        if (right <= left)
            return;
        int mid = left+(right - left)/2;
        //对nums的左半区间进行排序
        mergeSort(nums,left,mid);
        //对nums的右半区间进行排序
        mergeSort(nums,mid+1,right);
        //对排序好的左半区间和右半区间进行合并
        merge(nums,left,mid+1,right);
    }
    
    public static void main(String[] args) {
        int[] nums = {4,5,6,3,9,7,2,1,2,3,8,3,5,2,5,2,5,3,6,6,3,9,7,0,8,6,4};
        new MergeSort().mergeSort(nums,0,nums.length-1);
        for (int i = 0; i < nums.length; i++) {
            System.out.print(nums[i]+" ");
        }
    }
}
```