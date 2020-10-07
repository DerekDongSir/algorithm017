#### 242.有效的字母异位词
```go
func isAnagram(s string, t string) bool {
    if len(s) != len(t){return false}
    var arr = [26]int{}

    for _, v := range s{
        arr[int(v) - 97] ++
    }
    for _, v := range t{
        arr[int(v) - 97] --
    }

    for _, v := range arr{
        if v != 0{
            return false
        }
    }
    
    return true
}
```
```py
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t) : return False

        arr = [0 for _ in range(26)]
            
        for a, b in zip(s, t):
            arr[ord(a) - 97] += 1
            arr[ord(b) - 97] -= 1
        
        return not any(arr) # arr中所有元素应该都为0
```

#### 49.字母异位词分组
```py
from collections import defaultdict

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:    
        ans = defaultdict(list)

        for s in strs:
            ans[''.join(sorted(s))].append(s)
        
        return list(ans.values())
```
```go
func groupAnagrams(strs []string) [][]string {
    var(
        ans = make([][]string, 0)
        hashmap = make(map[[26]int][]string)
        generateKey = func(str string)[26]int{
            var key [26]int
            for _, v := range str{
                key[v - 'a'] ++
            }
            return key
        }
    )

    for _, str := range strs{
        key := generateKey(str)
        ls, ok := hashmap[key]
        
        if !ok{
            hashmap[key] = []string{str}
        }else{
            hashmap[key] = append(ls, str)
        }
    }

    for _, ls := range hashmap{
        ans = append(ans, ls)
    }

    return ans
}
```

#### 二叉树的中序遍历
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func inorderTraversal(root *TreeNode) []int {
    var ans = []int{}
    var inorder func (node *TreeNode)

    inorder = func (node *TreeNode){
        if node == nil{ return }

        inorder(node.Left)
        ans = append(ans, node.Val)
        inorder(node.Right)

    }

    inorder(root)

    return ans
}
```
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func inorderTraversal(root *TreeNode) []int {
    var ans = []int{}
    var cur = root
    var stack = []*TreeNode{}

    for cur != nil || len(stack) > 0{
        for cur != nil{
            stack = append(stack, cur)
            cur = cur.Left
        }

        cur = stack[len(stack) - 1]
        stack = stack[:len(stack) - 1]
        ans = append(ans, cur.Val)
        cur = cur.Right
    }

    return ans
}
```
#### 二叉树的前序遍历
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func preorderTraversal(root *TreeNode) []int {
    var (
        ans = make([]int, 0)
        preorder func(node *TreeNode)
    )
    
    preorder = func(node *TreeNode){
        if node != nil{
            ans = append(ans, node.Val)
            preorder(node.Left)
            preorder(node.Right)
        }
    }

    preorder(root)
    
    return ans
}
```

```go
func preorderTraversal(root *TreeNode) []int {
    var(
        ans = make([]int, 0)
        stack []*TreeNode
    )
    if root != nil{ stack = []*TreeNode{root}}
    
    for len(stack) > 0{
        end := len(stack) -1 
        node := stack[end]
        stack = stack[:end]

        ans = append(ans, node.Val)
        if node.Right != nil{ stack = append(stack, node.Right) }
        if node.Left != nil{ stack = append(stack, node.Left) }
    }
    return ans
}
```

#### N叉树的后序遍历
```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func postorder(root *Node) []int {
    var(
        ans = make([]int, 0)
        pOrder func(node *Node)
    )

    pOrder = func(node *Node){
        if node != nil{
            for _, n := range node.Children{
                pOrder(n)
            }
            ans = append(ans, node.Val)
        }
    }
    pOrder(root)

    return ans
}
```
```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func postorder(root *Node) []int {
    var(
        ans = make([]int, 0)
        stack []*Node
    )
    if root != nil{
        stack = []*Node{root}
    }

    for len(stack) > 0{
        end := len(stack) - 1
        n := stack[end]

        ans = append(ans, n.Val)
        
        cp := make([]*Node, end + len(n.Children))
        copy(cp[:end], stack)
        copy(cp[end:], n.Children)
        stack = cp
    }

    for i, j := 0, len(ans)-1; i < j; i, j = i+1, j-1 {
        ans[i], ans[j] = ans[j], ans[i]
    }
    
    return ans
}
```


#### N叉树的前序遍历
```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func preorder(root *Node) []int {
    var ans = []int{}
    var pre func(node *Node)
    
    pre = func(node *Node){
        if node != nil{
            ans = append(ans, node.Val)

            for _, child := range node.Children{
                pre(child)
            }
        }
    }
    
    pre(root)

    return ans
}
```
```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func preorder(root *Node) []int {
    var(
        ans = make([]int, 0)
        stack []*Node
    )
    if root != nil{
        stack = []*Node{ root }
    }

    for len(stack) > 0{
        end := len(stack) - 1
        node := stack[end]

        ans = append(ans, node.Val)

        stack = stack[:end]
        children := node.Children
        for i := len(children) - 1; i >=0; i--{
            stack = append(stack, children[i])
        }
    }
    return ans
}
```


#### N叉树的层序遍历
```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func levelOrder(root *Node) [][]int {
    var (
        ans = make([][]int, 0)
        queue []*Node
    )
    if root != nil{
        queue = []*Node{ root }
    }

    for len(queue) > 0{
        len_ := len(queue)
        t := make([]int, len_)
        for i := 0; i < len_; i++{
            t[i] = queue[i].Val
            queue = append(queue, queue[i].Children...)
        }
        queue = queue[len_:]
        ans = append(ans, t)
    }

    return ans
}
```

#### 40.最小的k个数
**桶排序**
```go
func getLeastNumbers(arr []int, k int) []int {
    var(
        ans = make([]int, 0, k * 2)
        ls = make([]int, 10000)
    )
    for _, n := range arr{
        ls[n] ++
    }

    for i, v := range ls{
        if v > 0{
            for t := 0; t < v; t ++{
                ans = append(ans, i)
            }
            if len(ans) >= k{
                break
            }
        }
    }
    return ans[:k]
}
```
**heap**
```py
class Solution:
    def getLeastNumbers(self, arr: List[int], k: int) -> List[int]:
        ans  = []
        heapq.heapify(arr)

        for _ in range(k):
            ans.append(heapq.heappop(arr))
        
        return ans
```

#### 347.前K个高频元素
```py
from collections import Counter

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        dic = collections.Counter(nums)

        heap, ans = [], []

        for i in dic:
            heapq.heappush(heap, (-dic[i], i))

        for _ in range(k):
            ans.append(heapq.heappop(heap)[1])
            
        return ans
```
```py
from collections import Counter

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        dic = collections.Counter(nums)

        heap = []
        
        for i in dic:
            heapq.heappush(heap, (dic[i], i))

            if len(heap) > k:
                heapq.heappop(heap)

        return [i for _, i in heap]
```

#### 239.滑动窗口最大值
```py
from collections import deque

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        ans = []

        q = deque()

        for i, v in enumerate(nums):
            while q and v >= nums[q[-1]]:
                q.pop()
            
            q.append(i)

            if q[0] <= i - k:
                q.popleft()
            
            if i >= k - 1:
                ans.append(nums[q[0]])
        
        return ans
```

#### 263.丑数
```go
func isUgly(num int) bool {
    var NUMS = [3]int{5, 3, 2}

    if num == 0{ return false }
    if num == 1{ return true }

    for _, n := range NUMS{
        if num % n == 0{
            return isUgly(num / n)
        }
    }
    
    return false 
}
```

#### 264.丑数II
```go
func nthUglyNumber(n int) int {
    var (
        p2,p3,p5 = 0, 0, 0
        ls = []int{1}
        Min = func (a ...int) int {
            min := int(^uint(0) >> 1) // largest int
            for _, i := range a {
                if i < min {
                    min = i
                }
            }
            return min
        }
    )

    for i := 1; i < n; i++{
        value := Min(ls[p2] * 2, ls[p3] * 3, ls[p5] * 5)
        if value % 2 == 0{ p2 ++ }
        if value % 3 == 0{ p3 ++ }
        if value % 5 == 0{ p5 ++ }
        ls = append(ls, value)
    }
    return ls[n - 1]
}
```


