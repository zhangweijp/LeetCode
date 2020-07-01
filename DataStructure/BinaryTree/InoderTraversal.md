# 二叉树的中序遍历（中根遍历） 左中右
## 中序遍历的过程
1. 当前节点有左子树就优先访问左子树
2. 向左走到尽头之后，从栈中弹出分支
3. 有右子树则向右子树前进，没有则从栈中继续弹出分支
## 如何实现
中序遍历也是依靠DFS来完成便利，所以同样使用stack作为容器存储节点，访问到当前节点不为时将其入栈，然后向左子树前进，当前节点为空时则从栈中弹出分支，并将其保存的元素存入链表，而栈中的节点都已经访问过左子树，所以我们不用重复访问其左子树，而应该去访问其右子树，当当前节点为空且栈也为空时，则说明遍历已经完毕。
## java代码
```java
public List ineorder(TreeNode root){
    Stack stack = new Stack();
    List list = new LinkedList();
    while(!stack.isEmpty || root!=null){
        if(root!=null){
            stack.push(root);
            root = root.left;
        } else{
            root = stack.pop();
            list.add(root.val);
            root = root.right;
        }
    }
}
```