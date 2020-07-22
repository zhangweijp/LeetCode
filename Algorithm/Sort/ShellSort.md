# 希尔排序
希尔排序是对插入排序的一种优化，希尔排序效率比普通的插入排序要高，但是**不具备稳定性**。  
## 算法思想
按照一定的间隔，将数组切割成几个子数组，对其进行插入排序，使得子数组内部先有序，整体上基本有序，缩小间隔再次插入排序，直到间隔缩小为1时排序完成。  
**特点：**
* 间隔大的时候 交换的次数少
* 间隔小的时候 移动的距离短
## Java代码
```java
public class ShellSort {

    public void shellSort(int[] nums) {
        int n = 1;
        //n要超过数组长度的1/3
        while (n <= nums.length/3) {
            n = 3*n +1;
        }
        for (int gap = n; gap > 0; gap = (gap-1)/3) {
            for (int i = gap; i < nums.length; i++) {
                int p = i;
                while (p >= gap && nums[p] < nums[p-gap]) {
                    int temp = nums[p];
                    nums[p] = nums[p-gap];
                    nums[p-gap] = temp;
                    p -= gap;
                }
            }
        }
    }

    //对交换方式进行了优化
    public void shellSort2(int[] nums) {
        int n = 1;
        //n要超过数组长度的1/3
        while (n <= nums.length/3) {
            n = 3*n +1;
        }
        for (int gap = n; gap > 0; gap = (gap-1)/3) {
            for (int i = gap; i < nums.length; i++) {
               int p = i-gap;
               int temp = nums[i];
               while (p>=0 && nums[p] > nums[p+gap]) {
                   nums[p+gap] = nums[p];
                   p -= gap;
               }
               nums[p+gap] = temp;
            }
        }
    }

    public static void main(String[] args) {
        int[] nums = {23,44,32,28,52,24,56,23,51,63,33,52,77,46};
        new ShellSort().shellSort2(nums);
        for (int i = 0; i < nums.length; i++) {
            System.out.print(nums[i]+" ");
        }
    }
}
```