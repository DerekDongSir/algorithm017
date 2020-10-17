#### 22.括号生成
```go
func generateParenthesis(n int) []string {
    var ans = []string{}
    var dfs func(path []byte, left, right int)

    dfs = func(path []byte, left, right int){
        if left == n && right == n{
            ans = append(ans, string(path))
            return
        }

        if left < n{
            cp := make([]byte, len(path))
            copy(cp, path)
            dfs(append(cp, '('), left + 1, right)
        }

        if right < left{
            dfs(append(path, ')'), left, right + 1)
        }
    }

    dfs([]byte{}, 0, 0)

    return ans
}
```

#### 236.二叉树的最近公共祖先

```go
/**
 * Definition for TreeNode.
 * type TreeNode struct {
 *     Val int
 *     Left *ListNode
 *     Right *ListNode
 * }
 */
 func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
     if root == nil{ return nil }

     if p == root || q == root{ return root }

     l := lowestCommonAncestor(root.Left, p, q)
     r := lowestCommonAncestor(root.Right, p, q)

     if l != nil && r != nil{ return root }
     if l != nil{ return l }
     return r
}
```


#### 105.从前序与中序遍历序列构造二叉树
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func buildTree(preorder []int, inorder []int) *TreeNode {
    if len(preorder) == 0{ return nil }

    if len(preorder) == 1{ return &TreeNode{Val: preorder[0]}}

    x := preorder[0]
    root := &TreeNode{ Val : x }

    for i, v := range inorder{
        if v == x{
            root.Left = buildTree(preorder[1: i + 1], inorder[:i])
            root.Right = buildTree(preorder[i + 1:], inorder[i + 1:])
            break            
        }
    }
    return root
}
```

#### 77.组合
```py
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        ans = []
        
        def helper(path, start):
            if len(path) == k:
                ans.append(path[:])
                return
            if n - start + 1 < k - len(path):
                return # 剪枝

            for i in range(start, n + 1):
                path.append(i)
                helper(path, i + 1)
                path.pop()
        
        helper([], 1)

        return ans
```
```go
func combine(n int, k int) [][]int {
    var (
        ans = make([][]int, 0)
        helper func(path []int, start int)
    )

    helper = func(path []int, start int){
        if n - start + 1 < k - len(path){
            return
        }
        if len(path) == k{
            cp := make([]int, k)
            copy(cp, path)
            ans = append(ans, cp)
            return
        }

        for i := start; i <= n; i++{
            path = append(path, i)
            helper(path, i + 1)
            path = path[: len(path) - 1]
        } 
    }
    
    helper(make([]int, 0, k), 1)

    return ans
}
```


#### 46.全排列
**python permutations**
```py
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        return list(itertools.permutations(nums))
```

**backtracking**
```py
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        ans = []

        def dfs(path, rest):
            if not rest:
                ans.append(path)
                return
                
            for i in range(len(rest)):
                dfs(path + [rest[i]], rest[:i] + rest[i+1:])

        dfs([], nums)

        return ans
```

```go
func permute(nums []int) [][]int {
    var (
        ans = [][]int{}
        dfs func(path, rest []int)
    )

    dfs = func(path, rest []int){
        if len(rest) == 0{
            ans = append(ans, path)
            return
        }

        for i:= 0; i < len(rest); i++{
            cp := make([]int, len(path), len(path) + 1)
            copy(cp, path)
            
            t := make([]int, len(rest) - 1)
            copy(t[0:i], rest[0:i])
            copy(t[i:], rest[i + 1:])

            dfs(append(cp, rest[i]), t)
        }
    }
    dfs([]int{}, nums)

    return ans
}
```

#### 47.全排列 II

```py
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        ans = []

        def dfs(path, l):
            if not l:
                ans.append(path)
                return

            d = set()
            for k, v in enumerate(l):
                if v not in d:
                    dfs(path + [v], l[:k] + l[k + 1:])
                    d.add(v)

        dfs([], nums)
        return ans
```

```go
func permuteUnique(nums []int) [][]int {
    var(
        ans = make([][]int, 0)
        dfs func(path, rest []int)
    )

    dfs = func(path, rest []int){
        if len(rest) == 0{
            ans = append(ans, path)
            return
        }

        d := make(map[int]struct{})

        for k, v := range rest{
            if _,ok := d[v]; !ok{
                cp := make([]int, len(path) + 1)
                copy(cp[0: len(path)], path)
                cp[len(path)] = v


                t := make([]int, len(rest) -1)
                copy(t[0:k], rest[:k])
                copy(t[k:], rest[k + 1:])

                d[v] = struct{}{}
                dfs(cp, t)
            }
        }
    }
    dfs([]int{}, nums)

    return ans
}
```
