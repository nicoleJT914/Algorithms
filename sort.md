## 选择排序
每次选择剩余元素中最小的放到数组左边
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
将当前元素与其左边所有元素进行比较，如果比左边元素小，则交换位置。插入排序保证当前索引左边的元素是有序的。
```java
public class Insertion {
    public static void sort(Comparable[] a) {
        int N = a.length;
        for (int i=1; i<N; i++) {
            for (int j=i; j>0 && (a[j].compareTo(a[j-1]) < 0); j--) {
                exch(a, j, j-1);
            }
        }
    }
}
```
最好情况 ==> 有序 ==> N-1次比较和0次交换

最坏情况 ==> 逆序 ==> N^2/2次比较和N^2/2次交换

插入排序对部分有序的数组十分高效，也很适合小规模数组

插入排序先找出最小元素置于数组的最左边，可以省略内循环j>0的条件

## 希尔排序
基于插入排序的快速的排序算法，思想是使数组中任意间隔为h的元素都是有序的

交换不相邻的元素以对数组的局部进行排序，并最终用插入排序将局部有序的数组排序
```java
public class Shell {
    public static void sort(Comparable[] a) {
        int N = a.length;
        int h = 1;
        while (h < N/3) {
            h = 3*h + 1;
        }
        while (h >= 1) {
            for (int i=h; i<N; i++) {
                for (int j=i; j>=h && (a[j].compareTo(a[j-h]) < 0); j -= h) {
                    exch(a, j, j-h);
                }
            }
            h /= 3;
        }
    }
}

```
数组规模大时，shell比插入排序快的多

## 归并排序
将两个有序的数组归并为一个更大的有序数组，用归并操作进行递归

- 自顶向下的归并排序

每次归并前将待归并元素复制到aux数组中，需要额外空间
```java
public class Merge {
    private static Comparable[] aux;
    public static void main(String[] args) {
        String[] a = {"M", "E", "R", "G", "E", "S", "O", "R", "T", "E", "X", "A", "M", "P", "L", "E"};
        sort(a);
        show(a);
    }
    public static void sort(Comparable[] a) {
        aux = new Comparable[a.length];
        sort(a, 0, a.length-1);
    }
    private static void sort(Comparable[] a, int lo, int hi) {
        if (hi <= lo)
            return;
        int mid = lo+(hi-lo)/2;
        sort(a, lo, mid);
        sort(a, mid+1, hi);
        merge(a, lo, mid, hi);
    }
    public static void merge(Comparable[] a, int lo, int mid, int hi) {
        int i = lo, j = mid+1;
        for (int k=lo; k<=hi; k++) {
            aux[k] = a[k];
        }
        for (int k=lo; k<=hi; k++) {
            if (i > mid)
                a[k] = aux[j++];
            else if (j > hi)
                a[k] = aux[i++];
            else if (less(aux[j], aux[i]))
                a[k] = aux[j++];
            else
                a[k] = aux[i++];
        }
    }
    private static boolean less(Comparable v, Comparable w) {
        return v.compareTo(w) < 0;
    }
    private static void show (Comparable[] a) {
        for (int i=0;i< a.length;i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println();
    }
}
```
归并排序所需的时间和NlgN成正比

缺点是辅助数组所使用的额外空间和N的大小成正比

改进：
1. 对小规模数组使用插入排序：递归对于小规模问题方法调用过于频繁
2. 测试数组是否已经有序：归并前，判断a[mid]<=a[mid+1]。此操作可将有序数组的运行时间变为线性
3. 不将元素复制到辅助数组：空间不可以！每次递归调用前交换数组和辅助数组的角色

- 自底向上的归并排序

从小粒度开始归并：1&1归并 ==> 2&2归并 ==> 4&4归并 ==> ...
```java
public class MergeBU {
    private static Comparable[] aux;
    public static void main(String[] args) {
        String[] a = {"M", "E", "R", "G", "E", "S", "O", "R", "T", "E", "X", "A", "M", "P", "L", "E"};
        sort(a);
        show(a);
    }
    public static void sort(Comparable[] a) {
        int N = a.length;
        aux = new Comparable[a.length];
        for (int sz=1; sz<N; sz *= 2) {
            for (int lo=0; lo<N-sz; lo += sz*2) {
                merge(a, lo, lo+sz-1, Math.min(lo+sz*2-1, N-1));
            }
        }
    }
    public static void merge(Comparable[] a, int lo, int mid, int hi) {
        int i = lo, j = mid+1;
        for (int k=lo; k<=hi; k++) {
            aux[k] = a[k];
        }
        for (int k=lo; k<=hi; k++) {
            if (i > mid)
                a[k] = aux[j++];
            else if (j > hi)
                a[k] = aux[i++];
            else if (less(aux[j], aux[i]))
                a[k] = aux[j++];
            else
                a[k] = aux[i++];
        }
    }
    private static boolean less(Comparable v, Comparable w) {
        return v.compareTo(w) < 0;
    }
    private static void show (Comparable[] a) {
        for (int i=0;i< a.length;i++) {
            System.out.print(a[i] + " ");
        }
        System.out.println();
    }
}
```

归并排序是一种渐进最优的基于比较排序的算法。

## 快速排序

切分的关键：
1. 对于切分j, a[j]已经排定
2. a[lo]到a[j-1]中的所有元素都不大于a[j]
3. a[j+1]到a[hi]中的所有元素都不小于a[j]

```java
public class Quick {
    public static void sort(Comparable[] a) {
        // StdRnadom.shuffle(a);
        sort(a, 0 ,a.length-1);
    }
    private static void sort(Comparable[] a, int lo, int hi) {
        if (hi <= lo)
            return;
        int j = partition(a, lo, hi);
        sort(a, lo, j-1);
        sort(a, j+1, hi);
    }
    private static int partition(Comparable[] a, int lo, int hi) {
        int i = lo, j = hi+1;
        Comparable v = a[lo];
        while (true) {
            while (less(a[++i], v)) {
                if (i == hi)
                    break;
            }
            while (less(v, a[--j])) {
                if (j == lo)
                    break;
            }
            if (i >= j)
                break;
            exch(a, i,  j);
        }
        exch(a, lo, j);
        return j;
    }
}

```

快速排序的特点：
1. 原地排序（只需要很小的辅助栈）
2. 对长度为N的数组排序所需时间和NlgN成正比

和归并排序的区别：
1. 归并排序递归调用发生在处理整个数组之前
2. 快速排序递归调用发生在处理整个数组之后

需要注意的：
1. j==lo这个条件是冗余的，因为a[lo]不可能比它自己小
2. 最好情况：每次都能正好将数组对半分
3. 最差情况：切分不平衡，解决方法就是排序前随机排序
