# 插入排序
具备稳定性，处理基本有序的数据时有着较好的效率。
## 算法思想
从前向后遍历数组，再以当前元素的位置为起点，向前选择合适的位置进行插入。
## Java代码
```
public class InsertionSort {
    public void insertionSort(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            int p = i;
            while (p > 0 && nums[p] < nums[p-1]) {
                int temp = nums[p];
                nums[p] = nums[p-1];
                nums[p-1] = temp;
                p--;
            }
        }
    }
    public static void main(String[] args) {
        int[] nums = {1,0,4,2,5,2,3,2,4,2,1,0,4,5};
        new InsertionSort().insertionSort(nums);
        for (int i = 0; i < nums.length; i++) {
            System.out.print(nums[i]+" ");
        }
    }
}
```