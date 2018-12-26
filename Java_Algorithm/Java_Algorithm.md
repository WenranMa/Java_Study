# Java数据结构算法 

## Part 1. 排序

### Selection Sort

- 每次选择最小值，分有序区和无序区，从无序区遍历找出最小的元素移动到有序区。O(N^2) time.
```Java
public static void selectionSort(int[] a) {
	for (int i = 0; i < a.length; i++) {
		int min = i;   //有序区
		for (int j = i + 1; j < a.length; j++) {
			if (a[j] < a[min]) {
				min = j;
			}
		}
		int temp = a[min];
		a[min] = a[i];
		a[i] = temp;
	}
}
```

### Insertion Sort

- 前面为排序好的，后面为待排序的，每次读入一个新数据(a[j])，从后往前依次比较。O(N^2) time.
```java
public static void insertionSort(int[] a) {
	for (int i = 0; i < a.length; i++) {
		for (int j = i; j > 0; j--) {
			if (a[j] < a[j - 1]) {
				int temp = a[j];
				a[j] = a[j - 1];
				a[j - 1] = temp;
			}
		}
	}
}
```

### Shell Sort

- 属于插入类排序,是将整个无序列分割成若干小的子序列分别进行插入排序，排序过程：先取一个正整数h1 < n，把所有序号相隔h的数组元素放一组，组内进行直接插入排序；然后取h2 < h1，重复上述分组和排序操作；直至hi=1，即所有记录放进一个组中排序为止。这里h的关系是3h+1. O(N^3/2)
```java
public static void shellSort(int[] a) {
	int len = a.length;
	int h = 1;
	while (h < len / 3) {
		h = h * 3 + 1;
	}
	while (h >= 1) {
		for (int i = h; i < len; i++) {
			for (int j = i; j >= h; j -= h) {
				if (a[j] < a[j - h]) {
					int temp = a[j];
					a[j] = a[j - h];
					a[j - h] = temp;
				}
			}		
		}
		h = h / 3;
	}
}
```

### Merge Sort

- O(nlogn) time, O(nlogn) space.
```java
public static void mergeSort(int[] a) {
	mergeSort(a, 0, a.length - 1);
}
private static void mergeSort(int a[], int l, int r) {
	if (l >= r)
		return;
	int mid = l + (r - l) / 2;
	mergeSort(a, l, mid);
	mergeSort(a, mid + 1, r);
	merge(a, l, r, mid);
}
private static void merge(int a[], int l, int r, int mid) {
	int[] left = new int[mid - l + 1]; // 注意下标大小
	int[] right = new int[r - mid];
	left = Arrays.copyOfRange(a, l, mid + 1); //right exclusive.
	right = Arrays.copyOfRange(a, mid + 1, r + 1);
	int i = 0, j = 0, k = l; // 注意K的下标 不一定是零
	while (i < left.length && j < right.length) {
		if (left[i] < right[j])
			a[k++] = left[i++];
		else
			a[k++] = right[j++];
	}
	while (j < right.length) // 剩余数据的处理
		a[k++] = right[j++];
	while (i < left.length)
		a[k++] = left[i++];
}
```

### Heap Sort

- 堆，二叉堆，一棵完全二叉树。树中每个节点与数组中存放该节点值的元素相对应。树的每一层都是填满的，除了最后一层。节点在堆中的高度定义为从本节点到叶子节点的最长简单下降路径上边的数目，而堆的高度就是树根的高度。因此，在高度为h的堆中，共有h+1层，最多有2^(h+1)-1个元素，最少有2^h个元素。而n个元素的堆的高度为log2n的下取整。
整个堆如果有n个元素，则叶子是n/2的下取整+1, +2, +3,...n. 比如n=9，9个元素，则叶子的下标是5 6 7 8 9。注意此时堆的下标是以1开始的。实际编程中下标为0开始，所以如果是n个元素，叶子应该是n/2的下取整, +1, +2, +3,...n-1.剩下的就是非叶子节点。
最后整个heapsort的时间为O(nlogn). 因为n-1次调用max-heapify. O(1)Space.
```java
public static void heapSort(int[] a) { // 先建堆，然后从root中取最大数
	int len = a.length;
	buildHeap(a, len);
	for (int i = len - 1; i > 0; i--) {
		swap(a, 0, i); // 最大值在root，所以和数组最后的值做交换
		len--; // 然后去掉最大值
		max_heapify(a, len, 0); // 新的堆root是个小值，所以要继续沉到底
	}
}
public static void buildHeap(int[] a, int len) { // 长度是n，起始为0，从(n/2下取整)开始全是叶子
	for (int i = len / 2 - 1; i >= 0; i--) {
		max_heapify(a, len, i); // len/2-1 就是对所有非叶子的节点下沉
	}
}
public static void max_heapify(int[] a, int heapSize, int i) {
	int l = 2 * i + 1; // i的起始值是0，所以比书上的公式多了1
	int r = 2 * i + 2;
	int target = i; // target就是要对其做递归的参数
	if (l < heapSize && a[l] > a[target]) {
		target = l;
	}
	if (r < heapSize && a[r] > a[target]) {
		target = r;
	}
	if (target != i) {
		swap(a, i, target);
		max_heapify(a, heapSize, target);
	}
}
public static void swap(int[] a, int i, int j) {
	int temp = a[i];
	a[i] = a[j];
	a[j] = temp;
}
```
