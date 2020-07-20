# 计数排序
与基数排序类似，基数排序也是一种特殊的桶排序，用空间复杂度来换时间复杂度。计数排序的应用场景：主要是针对数据量较大而数据的范围较小的数据集合。
## 算法思想
已知数组Arr的值域为\[0,N],我们新建一个数组count\[],用来存放数组元素出现的频率，遍历Arr，count\[Arr\[n]]++,count\[i]就记录了Arr中元素i出现的频率。  
**例子：** 
```
    Arr = {0,1,4,2,4,2,5,2,1,3} 取值范围[0-5]
    那么count数组就为:
    count = {1,2,3,1,2,1}
    count[0] 存放的就是Arr中0出现的频率：1
    ...
    count[5] 存放的就是Arr中5出现的频率：1
```
**我们拿到这个count数组能干什么呢？**  
count\[i]存放的是元素出现的频率，那么我们可以通过遍历count数组还原出一个排好序的ArrSorted数组。
```
    count = {1,2,3,1,2,1}

    ArrSorted = {0,1,1,2,2,2,3,4,4,5}
```
**Java代码**
```java
    for(int i = 0, j = 0; i < count.length; i++) {
        while(count[i]-- > 0) {
            ArrSorted[j] = i;
        }
    }
```
## 改进
* 我们限定了数据范围的模式是\[0,N]，这显然没有普适性，如果数据的范围改为\[Start,End]，如何改进呢？
* 排序过程之中并不能保证稳定性，即相同的元素之间的内部有序性不能得以保证。  
1. 对于第一点，我们只需要对数据进行简单的偏移操作即可将\[Start,End]重新构造成我们熟悉的\[0,N]模式，即\[(Start - Start), (End - Start)] --> \[0,N]，我们新建一个大小为Q-M+1的count数组，对于Arr中的元素Arr\[i]，以 count\[Arr\[i]-M]++ 的方式来存储其频率。
2. 对于第二点，已经知道count\[i]存储的是i的个数，那么通过count数组们就能知道——元素i在排序数组中处于什么位置，有多少个：
```
    i_start = count\[0] + count\[1] .... + count[i-1] + 1

    i_end = count\[0] + count\[1] .... + count[i-1] + count[i]
```
我们对count数组进行改造
```java
    for(int i = 1; i < count.length; i++) {
        count[i] += count[i-1];
    }
```
那么此时count\[i]存放的就是最后一个i在ArrSorted中出现的位置，我们倒叙遍历Arr，再借助此时的count数组就能保证排序算的稳定性；
```java
    for(int i = arr.length-1; i >= 0; i--) {
        ArrSorted[--count[Arr[i]]] = Arr[i];
    }
```
结合第一点的改进
```java
    for(int i = arr.length-1; i >= 0; i--) {
        ArrSorted[--count[Arr[i] - Start]] = Arr[i];
    }
```
### 完整的Java代码
```java
public class CountSort {
    public static int[] countSort(int[] nums, int start, int end) {
        end = end - start;
        //返回排好序的数组
        int[] result = new int[nums.length];
        //排序数组
        int[] count = new int[end+1];
        for (int i = 0; i < nums.length; i++) {
            count[nums[i]-start]++;
        }
        //不稳的排序版本
//        for (int i = 0, j = 0; i < count.length; i++) {
//            while (count[i]-- > 0) {
//                result[j++] = i;
//            }
//        }

        //稳定的版本
        for (int i = 1; i < count.length; i++) {
            count[i] += count[i-1];
        }

        for (int i = nums.length-1; i >= 0; i--) {
            result[--count[nums[i] - start]] = nums[i];
        }

        return result;
    }

    public static void main(String[] args) {
        int[] nums = {1,0,4,2,5,2,3,2,4,2,1,0,4,5};
        System.out.println(nums.length);
        nums =  CountSort.countSort(nums,0,5);
        for (int i = 0; i < nums.length; i++) {
            System.out.print(nums[i]+" ");
        }
    }
}
```