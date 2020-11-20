# 236. �������������������
### ԭ��
����һ��������, �ҵ�����������ָ���ڵ������������ȡ�

�ٶȰٿ�������������ȵĶ���Ϊ���������и��� T ��������� p��q������������ȱ�ʾΪһ����� x������ x �� p��q �������� x ����Ⱦ����ܴ�һ���ڵ�Ҳ���������Լ������ȣ�����

���磬�������¶�����:  root = [3,5,1,6,2,0,8,null,null,7,4] 

ʾ�� 1:
����: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
���: 3
����: �ڵ� 5 �ͽڵ� 1 ��������������ǽڵ� 3��

ʾ�� 2:
����: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
���: 5
����: �ڵ� 5 �ͽڵ� 4 ��������������ǽڵ� 5����Ϊ���ݶ�������������Ƚڵ����Ϊ�ڵ㱾��

˵��:
���нڵ��ֵ����Ψһ�ġ�
p��q Ϊ��ͬ�ڵ��Ҿ������ڸ����Ķ������С�

��Դ�����ۣ�LeetCode��
[����](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree)��https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree

### 2�ֽⷨ
##### 1.�ݹ�
```java
private TreeNode res;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        res = null;
        contains(root, p, q);
        return res;
    }

    private boolean contains(TreeNode x, TreeNode p, TreeNode q){
        if(x == null) return false;
        boolean leftCon = contains(x.left, p, q);
        boolean rightCon = contains(x.right, p, q);
        if((x == p && rightCon) || (x == q && leftCon) || (x == p && leftCon)|| (x == q && rightCon) ||(leftCon && rightCon)) {
            res = x;
            return true;
        }
        return x == p || x == q || leftCon || rightCon;
    }
```
˼·������
* �������`p,q`�����ڽ��`x`��xΪ`p,q`������������Ƚ�㣬���ҽ����������������
    * xΪq��x���������ְ���p
    * xΪq��x���������ְ���p
    * xΪp��x���������ְ���q
    * xΪp��x���������ְ���q
    * x�����������������ֱ����p��q��
* Ϊʲôֻ������������������������룬���x����p��q������������Ƚ�㣬���������壺
    * x���ǹ������Ƚڵ㣬��xΪ��������ͬʱ����p��q��
    * x�ǹ������Ƚڵ㣬����������ģ���ôx����������ֻ����һ������p��q��
    * �����������������ʣ��������˵�����������
* ����������Ҫ�ж���ĳ�����Ϊ�������Ƿ���p��q֮һ�����庯��`boolean contains(TreeNode x, TreeNode p, TreeNode q)`�����ں����ڲ����ǻ���Ҫ�жϽ��x�Ƿ�������������Ƚ�㡣
    * �ݹ����������`x == null`����Ȼ��������p��q������`false`
    * �ݹ����`boolean leftCon = contains(x.left, p, q); `�õ��������Ƿ���p����q��`boolean rightCon = contains(x.right, p, q);`�õ��������Ƿ���p����q��
    * �������һ��ʼ˵���������`(x == p && rightCon) || (x == q && leftCon) || (x == p && leftCon)|| (x == q && rightCon) ||(leftCon && rightCon)`�����ҵ�����������ӽڵ�`res = x;`��������xΪ�������ܰ���p��q������`true`��
    * �������xΪq����p��������������ֻ����һ����p��q������true�����򷵻�false��` return x == p || x == q || leftCon || rightCon;`��
    * ע�����`res`����ֵ��һ�Σ�Ȼ�󷵻ؽ����p��q�����ڰ���������������У�������ĵݹ������`leftCon`��`rightCon`ֻ����һ��Ϊtrue�����������㲻�����q����q�����Ա���ÿ�����Ĺ����У�`res`�ĸ�ֵֻ����һ�Σ�����Ҫ�ҵ�����������Ƚ�㡣
* ����������ֻ��Ҫ��`res`��ֵΪ`null`��Ȼ���ٵ���`contains(root, p, q)`��Ȼ�󷵻�`res`���ɡ�
* ʱ�临�Ӷ�Ϊ$O(n)$���ռ临�Ӷ������߳����ȡ�

���н����
* ִ����ʱ :8 ms, ������ Java �ύ�л�����99.70%���û�
* �ڴ����� :42 MB, ������ Java �ύ�л�����5.02%���û�

##### 2.���仯����
��PS�����Ǹ��˺ܾ�������ʱû�������һ�ַ�����������ģ����Ž��ǵ�һ��������

```java
Map<TreeNode, Integer> map;

    public TreeNode lowestCommonAncestor2(TreeNode root, TreeNode p, TreeNode q) {
        map = new HashMap<>();
        return helper(root, p, q);
    }

    private TreeNode helper(TreeNode root, TreeNode p, TreeNode q){
        if (contains(root.left, p.val, q.val) == 2)
            return helper(root.left, p, q);
        else if (contains(root.right, p.val, q.val) == 2)
            return helper(root.right, p, q);
        return root;
    }

    // -1��ʾ��û�ҵ�, 0��ʾ�ҵ�p��1��ʾ�ҵ�q��2��ʾ���ҵ�
    private int contains(TreeNode x, int p, int q) {
        if (x == null) return -1;
        if (map.containsKey(x))
            return map.get(x);
        int temp;
        if (x.val == p) {
            if (contains(x.left, p, q) == 1 || contains(x.right, p, q) == 1)
                temp = 2;
            else temp = 0;
        } else if (x.val == q) {
            if (contains(x.left, p, q) == 0 || contains(x.right, p, q) == 0)
                temp = 2;
            else temp = 1;
        } else {
            int left = contains(x.left, p, q), right = contains(x.right, p, q);
            if (left == 2 || right == 2 || (left == 0 && right == 1) || (left == 1 && right == 0))
                temp = 2;
            else if (left == 0 || right == 0)
                temp = 0;
            else if (left == 1 || right == 1)
                temp = 1;
            else temp = -1;
        }
        map.put(x, temp);
        return temp;
    }
```
˼·������
* ��xΪ�������У�����p��q�����ֻ�����֣����庯��`int contains(TreeNode x, int p, int q)`�ж����������
    * p,q���ڸ����У�����-2
    * p,q�����ڸ����У�����-1
    * p�ڸ����У���q���ڸ����У�����0
    * q�ڸ����У���p���ڸ����У�����1
* `contains`���ж��߼��е㸴�ӣ�
    * ���x��ֵΪp����ȥ�������������������Ƿ����ҵ�q��Ҳ����`contains(x.left, p, q) == 1 || contains(x.right, p, q) == 1`�������ҵ���x������p��q������2������ֻ����q������0
    * ���x��ֵΪq���ԳƵ��߼�
    * ���x��ֵ��ΪqҲ��Ϊp����ȥ�鿴�������������`int left = contains(x.left, p, q), right = contains(x.right, p, q);`
        * �����������������������һ������p��q�������������ֱ����p��q������2
        * ����������������һ������p������0
        * ����������������һ������q������1
        * ���򷵻�-1
* ����`TreeNode helper(TreeNode root, TreeNode p, TreeNode q)`������ֵΪp��q������������Ƚ�㡣
    * ���x��������ͬʱ����p��q��˵���������в���p��q������������Ƚ�㡣
    * ��������������ͬ��
    * ����������������p��q�е�һ������ǰ������Ҫ�ҵĴ𰸣���Ϊ��Ŀ�涨һ������p��q������������Ƚ�㣬���Բ������Ҳ����������
* �������ֱ�Ӱ��ղŵ�˼·�������кܶ���ظ����㣬���ʱ��Ϳ����ڵݹ��м�����仯������ʹ��ɢ�б����Ϊֵ����ʾ����p��q���������Ϊֵ����`contains`�ĵݹ�������ȥ�鿴�Ƿ��Ѿ�������ý�����������о�ֱ�ӷ��أ����û�оͰ��߼����㣬��󷵻�ǰ�����������д洢��

���н����
* ִ����ʱ :11 ms, ������ Java �ύ�л�����35.86%���û�
* �ڴ����� :42.3 MB, ������ Java �ύ�л�����100.00%���û�

----
* ����LeetCode����뿴[���ֿ�](https://github.com/ustcyyw/yyw_algorithm)
* �������С�����Զ��������ο�[������Ŀ](https://github.com/ustcyyw/markdown_tool)
* [�ҵ�github](https://github.com/ustcyyw)���б��С��ĿҲ�ܺ��档��΢���~С����зз