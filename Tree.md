## 树

### 定义
树是n（n>=0）个结点的有限集。
- n > 0 时只有一个根结点。
- m > 0 时，子树的个数没有限制，但一定是互不相交的。

### 树的相关概念

#### 结点的度/树的度 （Degree）
结点的度：结点拥有的子树的个数  
树的度：树内各结点的度的最大值

#### 结点的层次（Level）/树的深度 （Depth）
结点的层次：从根开始定义，根为第一层，根的孩子为第二层  
树的深度：树中结点的最大层次

## 二叉树
n个结点的有限集合，由一个根结点和两颗互不相交的、分别称为根结点的左子树和右子树的二叉树组成。  

### 二叉树的性质
5条

### 二叉树的存储
- 顺序存储（一般只用于完全二叉树）
- 链式存储

### 二叉树的遍历
- 前序遍历
- 中序遍历
- 后序遍历
- 层序遍历

```js
// 层级遍历-递归
var levelRecursive = function(root) {
    if(!root) return []
    let result = []
    let stack = [root]
    let count = 0
    
    function bfs() {
        if(count >= stack.length) return
        
        let node = stack[count]
        result.push(node.val)
        node.left && stack.push(node.left)
        node.right && stack.push(node.right)
        count++
        bfs()
    }
    
    bfs()
    return result
}
```
```js
// 层级遍历-非递归-方法一
var levelUnRecursive = function(root) {
    if(!root) return []
    let result = []
    let queue = [root]
    let pointer = 0
    while(pointer<queue.length) {
        let node = queue[pointer++]
        result.push(node.val)
        node.left && queue.push(node.left)
        node.right && queue.push(node.right)
    }
    return result
}
// 层级遍历-非递归-方法二
var levelUnRecursive = function(root) {
    if(!root) return []
    let result = []
    let queue = [root]
    while(queue.length) {
        let node = queue.shift()
        result.push(node.val)
        node.left && queue.push(node.left)
        node.right && queue.push(node.right)
    }
    return result
}
```
```js
// 前序遍历-递归
var preorderRecursive = function(root) {
    let result = []
    
    function dfs(node) {
        if(!node) return
        result.push(node.val)
        dfs(node.left)
        dfs(node.right)
    }
    
    dfs(root)
    return result
}
```
```js
// 前序遍历-非递归
var preorderUnRecursive = function(root) {
    let result = []
    if(!root) return result
    let stack = [root]
    while(stack.length) {
        let node = stack.pop()
        result.push(node.val)
        node.right && stack.push(node.right)
        node.left && stack.push(node.left)
    }
    return result
}
```
```js
// 中序遍历-递归
var inorderRecursive = function(root) {
    let result = []
    
    function dfs(node) {
        if(!node) return
        dfs(node.left)
        result.push(node.val)
        dfs(node.right)
    }
    
    dfs(root)
    return result
}
```
```js
// 中序遍历-非递归
var inorderUnRecursive = function(root) {
    let result = []
    if(!root) return result
    let stack = []
    let node = root
    while(node) {
        stack.push(node)
        node = node.left
    }
    while(stack.length) {
        let node = stack.pop()
        result.push(node.val)
        let r = node.right
        while(r) {
            stack.push(r)
            r = r.left
        }
    }
    return result
}
```
```js
// 后序遍历-递归
var postorderRecursive = function(root) {
    let result = []
    
    function dfs(node) {
        if(!node) return
        dfs(node.left)
        dfs(node.right)
        result.push(node.val)
    }
    
    dfs(root)
    return result
}
```
```js
// 后序遍历-非递归
var postorderUnRecursive = function(root) {
    let result = []
    if(!root) return result
    let stack = [root]
    while(stack.length) {
        let node = stack.pop()
        result.unshift(node.val)
        node.left && stack.push(node.left)
        node.right && stack.push(node.right)
    }
}
```

## 线索二叉树

```js
typedef enum {Link, Thread} // 0 表示指向左右孩子指针；1 表示指向前驱/后继结点
typedef ADT BiThrNode {
    data
    lTag: Link|Thread
    BiThrNode lChild
    rTag: Link|Thread
    BiThrNode rChild
}
```
线索二叉树相当于是一个双向链表，大大节省了存储空间和查找时间。

### 线索二叉树的遍历
```js
// 伪代码
// T 为伪头节点，左孩子是根结点，后继是中序遍历的最后一个结点
function inOrderTraverseThr(T) {
    let p = T.lChild
    while(p) {
        while(p.lTag === 0) {
            p = p.lChild
        }
        while(p.rTag === 1 && p.rChild) {
            p = p.rChild
        }
        p = p.rChild
    }
}
```

## 常用的二叉树的分类

### 满二叉树  
如果所有分支结点都存在左子树和右子树，并且所有叶子都在同一层上。

### 完全二叉树
对一棵具有n个结点的二叉树按层序编号，如果编号的结点与同样深度的满二叉树中编号为i的结点在二叉树中位置完全相同，则称为完全二叉树。
- 结点顺序必须是连续的

### 赫夫曼树

#### 相关概念
- 路径长度：路径上的分支数量
- 树的路径长度：从树根到每一结点的路径长度之和
- 带权路径长度：路径长度与结点上权的乘积
- 树的带权路径长度：树中所有叶子结点的带权路径长度之和
- 赫夫曼树：树的带权路径长度 WPL 最小的二叉树，又叫“最优二叉树”

### 二叉排序树（Binary sort Tree）/ 二叉搜索树
- 若它的左子树不为空，则左子树上所有结点的值均小于它的根结点的值。
- 若它的右子树不为空，则右子树上所有结点的值均大于它的根结点的值。
- 它的左右子树分别为二叉排序树。

🌰 **二叉排序树的查找**
```js
// 递归
function searchBST(root, val) {
    if(!root) return null
    if(val === root.val) return root
    if(val < root.val) {
        return searchBST(root.left, val)
    } else {
        return searchBST(root.right, val)
    }
}
```
```js
// 非递归
function searchBSTUnRecursive(root, val) {
    if(!root) return null
    let node = root
    while(node) {
        if(val === node.val) return node
        if(val < node.val) node = node.left
        else node = node.right
    }
    return null
}
```

🌰 **二叉排序树的插入**
```js
// 递归
function insertBSTRecursive(root, val) {
    if(!root) {
        root.val = val
        return true
    }
    
    if(val === root.val) return false
    
    if(val < root.val) {
        if(root.left) {
            insertBSTRecursive(root.left, val)
        } else {
            root.left = new TreeNode(val)
        }
    } else {
        if(root.right) {
            insertBSTRecursive(root.right, val)
        } else {
            root.right = new TreeNode(val)
        }
    }
    
    return true
}
```
```js
// 非递归
function insertBSTUnRecursive(root, val) {
    if(!root) {
        root.val = val
        return true
    }

    let node = root
    while(node) {
        if(val === node.val) return false
        else if(val < node.val) {
            if(node.left) {
                node = node.left
            } else {
                node.left = new Treeroot(val)
                return true
            }
        } else {
            if(node.right) {
                node = node.right
            } else {
                node.right = new Treeroot(val)
                return true
            }
        }
    }

    return true
}
```

🌰 **二叉搜索树的删除**
```js
// 递归
function deleteBSTRecursive(root, val) {
    if(!root) return

    if(val === root.val) deleteNode(root)
    else if(val < root.val) {
        deleteBSTRecursive(root.left, val)
    }else {
        deleteBSTRecursive(root.right, val)
    }
}

function deleteNode(root) {
    if(!root.left) {
        // 无左结点
        root = root.right
    } else if(!root.right) {
        // 无右结点
        root = root.left
    } else {
        // 即有左结点又有右结点，找删除结点root的直接前驱
        let p = root
        let s = root.left
        while(s.right) {
            p = s
            s = s.right
        }

        root.val = s.val
        if(p !== root) {
            p.right = s.left
        } else {
            p.left = s.left
        }
    }
    return true
}
```

🌰 **二叉搜索树的判断**
```js
// 方法一
var isValidBST = function(root) {
    if(!root) return true
    
    if(root.left && maxFn(root.left) >= root.val) return false
    if(root.right && minFn(root.right) <= root.val) return false

    return isValidBST(root.left) && isValidBST(root.right)
};

var minFn = function(root) {
    let min = root.val
    if(root.left) {
        let minVal = minFn(root.left)
        min = minVal > min ? min : minVal
    }
    if(root.right) {
        let minVal = minFn(root.right)
        min = minVal > min ? min : minVal
    }
    return min
}

var maxFn = function(root) {
    let max = root.val
    if(root.left) {
        let maxVal = maxFn(root.left)
        max = maxVal > max ? maxVal : max
    }
    if(root.right) {
        let maxVal = maxFn(root.right)
        max = maxVal > max ? maxVal : max
    }
    return max
}
效率较慢，需要比较重复的结点
```

```js
// 方法二
var isValidBST = function(root) {
    return helper(root, Number.MIN_SAFE_INTEGER, Number.MAX_SAFE_INTEGER)
};

var helper = function(root, min, max) {
    if(!root) return true
    if(min >= root.val) return false
    if(max <= root.val) return false

    return helper(root.left, min, root.val) && helper(root.right, root.val, max)
}
```

```js
// 方法三
// 中序遍历，判断数组是否升序
var isValidBST = function(root) {
    if(!root) return true
    let stack = []
    let node = root

    while(node) {
        stack.push(node)
        node = node.left
    }

    let prev = Number.MIN_SAFE_INTEGER
    while(stack.length) {
        let node = stack.pop()
        if(node.val <= prev) return false
        prev = node.val
        if(node.right) {
            let r = node.right
            while(r) {
                stack.push(r)
                r = r.left
            }
        }
    }
    return true
};
```

```js
// 方法四
// 递归，改进方法三，减少空间复杂度
var prev = null
var isValidBST = function(root) {
    return isBST(root)
};
var isBST = function(root) {
    if(root) {
        if(!isBST(root.left)) return false
        if(prev && root.val <= prev.val) return false
        prev = root
        if(!isBST(root.right)) return false
    }
    return true
}
```

### 平衡二叉树（AVL树）

平衡二叉树是一种二叉排序树，其中每个节点的左子树和右子树的高度差至多等于1。
- 是一棵二叉排序树
- 左子树深度减右子树深度只能为1/0/-1


🌰 **判断二叉树是否是平衡二叉树**
```js
// 方法1: 递归
var isBalanced = function(root) {
    if(!root) return false
    if(isBalanced(root.left) && isBalanced(root.right)) {
        let left = getDepth(root.left)
        let right = getDepth(root.right)
        return Math.abs(left - right) <= 1
    }
    return false
}
var getDepth = function(root) {
    if(!root) return 0
    let left = getDepth(root.left)
    let right = getDepth(root.right)
    return Math.max(left, right) + 1
}
```
```js
// 方法2: 改进递归
var isBalanced = function(root) {
    let flag = true
    
    var balance = function(root) {
        if(!flag) return
        if(!root) return 0
        let left = balance(root.left)
        let right = balance(root.right)
        if(Math.abs(left - right) > 1) flag = false
        return Math.max(left, right) + 1
    }
    
    balance(root)
    return flag
}
```
```js
// 方法3: 改进递归
var isBalanced = function(root) {
    var balance = function(root) {
        if(!root) return 0
        let left = balance(root.left)
        let right = balance(root.right)
        if(left >= 0 && right >= 0 && Math.abs(left - right) <= 1) {
            return Math.max(left, right) + 1
        }
        return -1
    }
    
    return balance(root) >= 0
}
```
### 镜像二叉树

🌰 判断一棵二叉树是否镜像对称
```js
// 递归
var isSymmetric = function(root) {
    if(!root) return true
    
    var isMirror = function(left, right) {
        if(!left && !right) return true
        if(!left || !right) return false
        if(left.val === right.val) {
            return isMirror(left.left, right.right) && isMirror(left.right, right.left)
        }
        return false
    }
    
    return isMirror(root)
}
```
```js
// 迭代
var isSymmetric = function(root) {
    if(!root) return true
    let stack = [root.left, root.right]
    while(stack.length) {
        let node1 = stack.pop()
        let node2 = stack.pop()
        if(!node1 && !node2) continue
        if(!node1 || !node2) return false
        if(node1.val !== node2.val) return false
        stack.push(node1.left)
        stack.push(node2.right)
        stack.push(node1.right)
        stack.push(node2.left)
    } 
    return true
}
```

## Leetcode 刷题顺序
- [x] 面试题07-重建二叉树
- [x] 面试题26-树的子结构
- [x] 面试题27-二叉树的镜像
- [x] 面试题32-1 -从上往下打印二叉树
- [x] 面试题32-2 -从上往下打印二叉树 2
- [x] 面试题32-3 -从上往下打印二叉树 3
- [x] 面试题33-二叉搜索树的后序遍历序列
- [x] 面试题34-二叉树中和为某一值的路径
- [ ] 面试题36-二叉搜索树与双向链表
- [x] 面试题55-1-二叉树的深度
- [x] 面试题55-2-平衡二叉树
- [x] 面试题28-对称的二叉树
- [ ] 面试题37-序列化二叉树
- [x] 面试题54-二叉搜索树的第K大节点

