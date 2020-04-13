在了解这类问题解法之前，需要先了解heap相关的知识。

## 堆定义
堆是具有下列性质的完全二叉树：
- 每个结点的值都大于或等于其左右孩子结点的值，成为大顶堆
- 每个结点的值都小于或等于其左右孩子结点的值，成为小顶堆

如果按照层序遍历的方式给结点从1开始编号，则：
- 结点下标为i的结点，left(i) = 2i;right(i) = 2i + 1
- 结点下标为i的结点，parent(i) = Math.floor(i / 2)

## 堆排序算法
```js
function swap(arr, a, b) {
    [arr[a], arr[b]] = [arr[b], arr[a]]
}

function maxHeap(arr, i, size) {
    let left = 2 * i + 1
    let right = 2 * i + 2
    let largest = i

    if(left < size && arr[left] > arr[largest]) largest = left
    if(right < size && arr[right] > arr[largest]) largest = right
    if(largest !== i) {
        swap(arr, i, largest)
        maxHeap(arr, largest, size)
    }
}

function heapSort(arr) {
    let len = arr.length;
    if(len <= 1) return arr

    for(let i = Math.floor(len / 2); i >= 0; i--) {
        maxHeap(arr, i, len)
    }

    for(let j = 0; j < len; j++) {
        swap(arr, 0, len - 1 - j)
        maxHeap(arr, 0, len - 1 - j)
    }

    return arr
}
```
最好、最坏、平均时间复杂度均为O(nlogn)

### Top K 问题
掌握了堆排序之后，我们再来看这道题目。

这道题是一个经典的 `Top K` 问题，是面试中的常客。Top K 问题有三种不同的解法，均需要掌握。
- 直接排序
- 类似快速排序的分治法
- 使用堆（优先队列）

前置知识：需要先掌握`堆排序`和`快速排序`的思路

#### 1. 直接排序
```js
var getLeastNumbers = function(arr, k) {
    let newArr = arr((a, b) => a - b)
    return newArr.slice(0, k)
}
```

#### 2. 改进的快速排序
```js
var getLeastNumbers = function(arr, k) {
    let len = arr.length
    if(!len || !k) return []

    let start = 0
    let end = len - 1
    let pivot = quickSort(arr, start, end)
    
    while(pivot !== (k-1)) {
         if(pivot > k - 1) {
            pivot = quickSort(arr, start, pivot - 1)
        } else {
            pivot = quickSort(arr, pivot + 1, end)
        }
    }
    return arr.slice(0, k)
};

var quickSort = function(arr, start, end) {
    let pivot = arr[start]

    while(start < end) {
        while(start < end && arr[end] >= pivot) end--
        arr[start] = arr[end]

        while(start < end && arr[start] < pivot) start++
        arr[end] = arr[start]
    }

    arr[start] = pivot
    return start
}
```
时间复杂度 O(n) ， 空间复杂度O(n)

缺点：虽然时间复杂度是 O(n)，但是缺点也很明显，最主要的就是内存问题，在海量数据的情况下，我们很有可能没办法一次性将数据全部加载入内存，这个时候这个方法就无法完成使命了。还有一点就是这种思路需要我们修改输入的数组，这也是值得考虑的一点

#### 3. 堆
求前K个最小数，维护大顶堆；求前K个最大数，维护小顶堆。
```js
var getLeastNumbers = function(arr, k) {
    let len = arr.length
    if(!k) return []
    if(len <= 1) return arr

    // 把前k个数构建成大堆
    for(let i = Math.floor(k / 2); i >= 0; i--) {
        maxHeap(arr, i, k)
    }

    // 依次把k个数后面的数跟大堆根对比
    for(let j = k; j<len; j++) {
        if(arr[j] < arr[0]) {
            swap(arr, 0, j)
            // 维护大根堆性质
            maxHeap(arr, 0, k)
        }
    }

    let res = []
    while(k--) {
        res.push(arr.shift())
    }
    return res
};

function swap(arr, a, b) {
    [arr[a], arr[b]] = [arr[b], arr[a]]
}

function maxHeap(arr, i, size) {
    if(i >= size) return

    let left = 2 * i + 1
    let right = 2 * i + 2
    let largest = i

    if(left < size && arr[left] > arr[largest]) largest = left
    if(right < size && arr[right] > arr[largest]) largest = right
    if(largest !== i) {
        swap(arr, i, largest)
        maxHeap(arr, largest, size)
    }
}
```
时间复杂度 O(nlogk) ， 空间复杂度O(k)

对于海量数据，我们不需要一次性将全部数据取出来，可以一次只取一部分，因为我们只需要将数据一个个拿来与堆顶比较。

另外还有一个优势就是对于动态数组，我们可以一直都维护一个K大小的大顶堆，当有数据被添加到集合中时，我们就直接拿它与堆顶的元素对比。这样，无论任何时候需要查询当前的前 K 小数据，我们都可以里立刻返回给他。

整个操作中，遍历数组需要 O(n) 的时间复杂度，一次堆化操作需要 O(logK)，加起来就是 O(nlogK) 的复杂度，换个角度来看，如果 K 远小于 n 的话， O(nlogK) 其实就接近于 O(n) 了，甚至会更快，因此也是十分高效的。
