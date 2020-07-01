# Sorting

## 冒泡排序（bubble sort）

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200525163723531.png" alt="image-20200525163723531" style="zoom: 67%;" />

动态演示图：[bubble sort](https://github.com/siberia123/Gif/blob/master/冒泡排序动图.gif)

冒泡排序代码如下：

```c
#include <stdio.h>

/*
 * 思想：每经过一次排序，都会在未排序好的元素中挑选一个最大的元素，放置在已排序好元素的前端。
 * 划分：未排序好|已排序好
 * 步骤：每次都在未排序好的元素中，挑选一个最大的元素，并将其放到已排序好的元素中。
 */

void bubble_sort(int arr[],int length){
    //每次都需要把一个最大的元素移动到后面，一共只需要比较n-1个元素（还剩最后一个元素是不用比较的，因为它肯定是最小的）
    for (int i = 0;i < length - 1;i++)
        //需要比较的次数，只需要在未排序好的元素中进行比较（未排序元素的个数为：length-1-i）
        for (int j = 0;j < length - 1 - i;j++){ //这个length - 1 - i 就充当着分界线的作用
            if (arr[j] > arr[j + 1]){
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
}
--------------------------------------------------------------------------------------------------------
int main() {
    int arr[] = {3,2,5,56,7,3,6,7,23,36};
    bubble_sort(arr,10);
    for (int i = 0; i < 10; ++i) {
        printf("%d ",arr[i]);
    }
    return 0;
}
```





## 选择排序（selection sort）

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200526143122515.png" alt="image-20200526143122515" style="zoom:80%;" />

选择排序动图：[selection sort](https://github.com/siberia123/Gif/blob/master/选择排序动图.gif)

选择排序代码：

```c
#include <stdio.h>

/*
 * 思想：在未排序好的元素中选取最小的，放置在已排序好的末尾。
 * 划分：已排序好 | 未排序好
 */


void selection_sort(int arr[],int size){
    //总共需要排序 size - 1 次，因为最后一个剩下的元素肯定是最小的（每次都会在未排序的元素中挑选最小的，放在已排序好元素的后端。）
    for (int i = 0; i < size - 1; ++i) { //这个 i 充当着分界线的作用
        //用来记录最小元素
        int minIndex = i;
        //在未排序好元素中遍历，寻找最小元素
        for (int j = i + 1; j < size; ++j) {
            if (arr[j] < arr[minIndex])
                //找到了最小，就用minIndex来标记
                minIndex = j;
        }
        //将最小元素交换到已排序好元素的末端
        int temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
}
-----------------------------------------------------------------------------------------------------
int main(){
    int arr[] = {3,2,5,4,64,23,45,34,46,423,34,4,3,343,53};
    selection_sort(arr,15);
    for (int i = 0; i < 15; ++i) {
        printf("%d ",arr[i]);
    }
    return 0;
}

```





## 插入排序（insertion sort）

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200526125402763.png" alt="image-20200526125402763" style="zoom:80%;" />

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200526125444376.png" alt="image-20200526125444376" style="zoom:80%;" />

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200526125553976.png" alt="image-20200526125553976" style="zoom:80%;" />

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200526132002086.png" alt="image-20200526132002086" style="zoom:80%;" />

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200526132122415.png" alt="image-20200526132122415" style="zoom:80%;" />

动态演示图：[insertion sort](https://github.com/siberia123/Gif/blob/master/插入排序动图.gif)

插入排序代码如下：

```c
#include <stdio.h>
/*
 * 思想：从未排序的始端取一个元素，插入到已排序中。
 * 划分：已排序 | 未排序
 */

void insertion_sort(int arr[],int size){
    //从第二个元素开始取（第一个元素只有一个肯定是排序好的）
    for (int i = 1; i < size; ++i) {
        //j：代表已排序好元素的最后一个元素
        int j = i - 1;
        //先把未排序的始端保存（因为下面会发生移动覆盖）
        int key = arr[i];
        //对已排序的元素进行依次比较
        while (j >= 0 && key < arr[j]){
            //移动覆盖
            arr[j+1] = arr[j];
            j -= 1;
        }
        //元素插入到正确位置
        arr[j+1] = key;
    }
}
-----------------------------------------------------------------------------------------------
int main(){
    int arr[] = {1,4,3,54,23,5,34,23,3,4,42};
    insertion_sort(arr,11);
    for (int i = 0; i < 11; ++i) {
        printf("%d ",arr[i]);
    }
    return 0;
}
```





## 快速排序（quick sort）

