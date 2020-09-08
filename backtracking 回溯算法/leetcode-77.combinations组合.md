#题目

给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

输入: n = 4, k = 2
输出:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]


#要点&&思路

- 如果解决一个问题有多个步骤，每一个步骤有多种方法，题目又要我们找出所有的方法，可以使用回溯算法；
- 回溯算法是在一棵树上的 深度优先遍历（因为要找所有的解，所以需要遍历）；
- 组合问题，相对于排列问题而言，不计较一个组合内元素的顺序性（即 [1, 2, 3] 与 [1, 3, 2] 认为是同一个组合），因此很多时候需要按某种顺序展开搜索，这样才能做到不重不漏。

这里的剪枝要尤其注意一下，每一次不一定要遍历从当前position到最大的n，可以发现最大的是到上限为 n-(k-path.size)+1),第一次写不仔细想下可能会漏掉剪枝，剪枝之后时间还是会节省不少的。


#code

class Solution {
    public List<List<Integer>> combine(int n, int k) {
        
        List<List<Integer>> ans =new ArrayList<>();
        Deque<Integer> path = new ArrayDeque<>();
        if(n<k||k<=0){
            return ans;
        }
        dfs(n,k,1,path,ans);
        
        return ans;
    }
    
    public void dfs(int n,int k,int position,Deque<Integer> path,List<List<Integer>> ans){
        if(path.size()==k){
            ans.add(new ArrayList(path));
            return;
        }
        for(int x=position;x<=n-(k-path.size())+1;x++){ //←注意一下这里的剪枝
            path.addLast(x);
            dfs(n,k,x+1,path,ans);
            path.removeLast();
        }
    }
}

