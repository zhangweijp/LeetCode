# 归并排序
## Java代码
```java
public class MergeSort {
    public void merge(int[] nums ,int left, int mid, int right) {
        int[] temp = new int[right - left +1];
        for (int k = left; k <= right; k++) {
            temp[k-left] = nums[k];
        }

        mid -= left;
        right -= left;

        int count = left;
        int i = 0,j = mid;

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
        mergeSort(nums,left,mid);
        mergeSort(nums,mid+1,right);
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