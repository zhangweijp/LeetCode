# 二叉树的先序遍历（先根遍历） 中左右
## 先序遍历的过程：
1. 先访问根节点
2. 有左子树就访问左子树
3. 最后访问右子树
## 如何实现
无论是先序遍历、中序遍历还是后序遍历，都是基于DFS的，也就是说我们可以使用栈来保存二叉树的右分支，这样每当左子树走到尽头时，直接从栈中弹出之前保存的右分支即可，若栈也为空则说明遍历已经完毕。
## 详细步骤
```
while(栈stack不为空 || 当前节点cur不为空){
    if(cur不为空){
        if(cur的右子树不为空)
            cur入栈;
        cur向左子树前进;
    }else{
        从栈中弹出分支赋值给cur;
    }
}
```
## java代码
```java
public List preorder(TreeNode root){
    Stack stack = new Stack();
    List list = new LinkedList();
    while(!stack.isEmpty() || root!=null){
        if(root!=null){
            List.add(root.val);
            if(root.right!=null)
                stack.push(root.right);
            root = root.left;
        }else {
            root = stack.pop();
        }
    }
}
```