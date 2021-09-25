# go len 和cap区别

切片拥有 长度 和 容量。

切片的长度就是它所包含的元素个数。

切片的容量是从它的第一个元素开始数，到其底层数组元素末尾的个数。

切片 s 的长度和容量可通过表达式 len(s) 和 cap(s) 来获取。

你可以通过重新切片来扩展一个切片，给它提供足够的容量。试着修改示例程序中的切片操作，向外扩展它的容量，看看会发生什么。



```
package main

import "fmt"

func main()  {
   var nums = [6]int{1, 2, 3, 4, 6, 6}
   PrintNums(nums[:])

   PrintNums(nums[2:])

   PrintNums(nums[2:4])

}
func PrintNums(nums []int)  {
   fmt.Printf("len = %d,cap =  %d, nums=%v\n", len(nums), cap(nums), nums)
```

结果为

```
len = 6,cap =  6, nums=[1 2 3 4 6 6]
len = 4,cap =  4, nums=[3 4 6 6]
len = 2,cap =  4, nums=[3 4]
```



原文链接：

https://blog.csdn.net/weixin_43902449/article/details/106446474	