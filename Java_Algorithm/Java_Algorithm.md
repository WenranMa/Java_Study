#Java Algorithm and Data Structure (Java数据结构与算法)

##Part 1. 排序

###Selection Sort

- 特点：每次选择最小值，分有序区和无序区，从无序区遍历找出最小的元素移动到有序区。O(N^2) time.
```Java
public static void selectSort(int[] a) {
	for (int i = 0; i < a.length; i++) {
		int min = i;   //有序区
		for (int j = i + 1; j < a.length; j++) {
			if (a[j] < a[min])
				min = j;
		}
		int temp = a[min];
		a[min] = a[i];
		a[i] = temp;
	}
}
```




