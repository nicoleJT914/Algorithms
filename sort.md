## 选择排序
```java
public class Selection {
    public static void sort(Comparable[] a) {
        int N = a.length;
        for (int i = 0;i<N; i++) {
            int min = i;
            for (int j = i+1; j<N; j++) {
                if (a[min].compareTo(a[j]) > 0) {
                    min = j;
                }
            }
            exch(a, i, min);
        }
    }
    private static void exch(Comparable[] a, int i, int j) {
        Comparable temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
}
```
长度为N的数组，选择排序需要N次交换和N^2/2次比较

特点：
1. 运行时间与输入序列本身顺序无关
2. 数据移动是**最少**的

## 插入排序
```java
public class Insertion {
    public static void sort(Comparable[] a) {
        int N = a.length;
        for (int i=1; i<N; i++) {
            for (int j=i; j>0; j--) { // 注意边界条件是>0，不是>=0!
                if (a[j].compareTo(a[j-1]) < 0) {
                    exch(a, j, j-1);
                }
            }
        }
    }
}
```
最好情况 ==> 有序 ==> N-1次比较和0次交换

最坏情况 ==> 逆序 ==> N^2/2次比较和N^2/2次交换

插入排序对部分有序的数组十分高效，也很适合小规模数组

插入排序
