---

title: 排序算法

layout: post

---

##**1，冒泡排序**

重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。

```c
BUBBLE_SORT(A)
/* 每次冒泡都将最小的元素放到最前 */
for i = 1 to A.length-1
  for j = A.length down to i
    if A[j] < A[j-1]
      swap(A[j], A[j-1])
```

##**2，选择排序**

每一趟从待排序的数据元素中选出最小/大的一个元素，顺序放在已排好序的数列的最后，直到全部待排序的数据元素排完。

```c
SELECTION_SORT(A)
for i = 1 to A.length - 1
  min = +++MAX
  for j = i to A.length
    if A[j] < min
      min = A[j]
      minlabel = j
swap(A[i], A[minlabel])
```

##**3，插入排序**

通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。

```c
INSERT_SORT(A)
/* 对最前面的2/3/.../A.length个元素进行排序 */
for i = 2 to A.length
  key = A[i]
  i = i - 1
  while i > 0 and A[i] > key
    A[i + 1] = A[i]
    i = i - 1
  A[i + 1] = key
```

##**4，归并排序**

将两个（或两个以上）有序表合并成一个新的有序表，即把待排序序列分为若干个有序的子序列，再把有序的子序列合并为整体有序序列。

```c
MERGE_SORT(A, p, r)
/* 分治：左子数组，右子数组，整个数组 */
if p < r
  q = (p + r) / 2
  MERGE_SORT(A, p, q)
  MERGE_SORT(A, q + 1, r)
  MERGE(A, p, q, r)

MERGE(A, p, q, r)
/* 构造两个子数组分别进行存放 */
n1 = q - p + 1
n2 = r - q
let L[1...n1] and R[1...n2] be new array
for i = 1 to n1
  L[i] = A[p + i]
for j = 1 to n2
  R[j] = A[q + j]
i = 1
j = 1
/* 总共有r-p+1个元素，每次取L和R第一个小的元素放到数组中 */
for k = p to r
  if L[i] <= R[j]
    A[k] = L[i]
    i = i + 1
  else A[k] = R[j]
    J = j + 1
```

##**5，堆排序**

```c
HEAP_SORT(A)
/* 构造最大堆 */
BUILD_MAX_HEAP(A)
/* 每次将根节点和末节点交换，后堆大小减一，再对根节点进行维护 */
for i = A.length down to 2
  swap(A[i], A[1])
  A.heapsize = A.heapsize - 1
  HEAPIFY(A, 1)

BUILD_MAX_HEAP(A)
/* 只需对一半的非叶节点进行维护 */
A.heapsize = A.length
for i = A.length / 2 downto 1
  HEAPIFY(A, i)

HEAPIFY(A, i)
/* 与左右孩子进行比较，把大的放到父节点中 */
l = LEFT(i)
r = RIGHT(i)
largest = i
if l < A.heapsize and A[l] > A[i]
  largest = l
if r < A.heapsize and A[r] > A[i]
  largest = r
if largest != i
  swap(A[i], A[largest])
  HEAPIFY(A, largest)
```

##**6，快速排序**

通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

```c
QUICK_SORT(A, p, r)
/* 以某个元素为基准分为小于/等于/大于三部分，分别对两个子数组递归 */
if p < r
  q = PARTITION(A, p ,r)
  QUICK_SORT(A, p, q)
  QUICK_SORT(A, q + 1, r)

PARTITION(A, p, r)
/* 以最后一个元素为基准 */
key = A[r]
i = p - 1
for j = p to r - 1
  if A[j] < key
    i = i + 1
    swap(A[i], A[j])
swap A[i + 1] and A[r]
return i + 1
```

##**7，计数排序**

使用一个额外的数组C，其中第i个元素是待排序数组A中值等于i的元素的个数。然后根据数组C来将A中的元素排到正确的位置。

```c
COUNT_SORT(A, B, k)
/* k大于数组中的最大元素，构造B数组存放结果，C数组存放变量 */
for i = 1 to k
  C[i] = 0
for i = 1 to A.length
  C[A[i]] = C[A[i]] + 1
for i = 2 to C.length
  C[i] = C[i] + C[i - 1]
/* 将第几大的元素放在第几位，同时对其减一防止相等元素覆盖 */
for i = 1 to A.length
  B[C[A[i]]] = A[i]
  C[A[i]] = C[A[i]] - 1
```

##**8，基数排序**

将所有待比较数值（正整数）统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后, 数列就变成一个有序序列。

```c
RADIX_SORT(A, d)
for i = 1 to d
  use a stable sort o sort array A on digit i
```

##**9，桶排序**

将数组分到有限数量的桶子里。每个桶子再个别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序）。桶排序是鸽巢排序的一种归纳结果。

```c
BUCKET_SORT(A)
n = A.length
let B[0...n-1] be a new array
for i = 0 to n-1
  let B[i] be empty
for i = 1 to n
  insert A[i] into B[nA[i]]
for i = 0 to n-1
  sort list B[i] with insertion sort
concatenate lists B[0],B[1]...B[n-1] together
```

##**算法比较**

|| 算法名称 || 平均情况 || 最好情况 || 最坏情况 || 辅助存储 || 稳定性 ||

|| 冒泡排序 || O(n2) || O(n) || O(n2) || O(1) || 稳定 ||

|| 选择排序 || O(n2) || O(n2) || O(n2) || O(1) || 不稳定 ||

|| 插入排序 || O(n2) || O(n) || O(n2) || O(1) || 稳定 ||

|| 归并排序 || O(nlogn) || O(nlogn) || O(nlogn) || O(n) || 稳定 ||

|| 堆排序 || O(nlogn) || O(nlogn) || O(nlogn) || O(1) || 不稳定 ||

|| 快速排序 || O(nlogn) || O(nlogn) || O(n2) || O(logn) || 不稳定 ||

|| 计数排序 || O(x) || O(x) || O(x) ||  ||

|| 基数排序 || O(n) || O(n) || O(n) || O(n) || 稳定 ||

|| 桶排序 || O(n) || O(n) || O(n) || xxx || 不稳定 ||

注：O(1) < O(logn) < O(n) < O(nlogn) < O(n^2) < O(n^3) < O(2^n) < O(n!) < O(n^n)
