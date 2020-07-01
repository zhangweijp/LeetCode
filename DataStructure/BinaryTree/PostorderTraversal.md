# 二叉树的后序遍历（后根遍历） 左右中
## 后序遍历的过程
1. 优先访问其左子树
2. 当左子树访问完毕，在访问右子树
3. 最后回到根节点
## 通过先序遍历 得到后序遍历
我们已经知道先序遍历(root->left->right)，如果我们将中序遍历中，链表的插入节点的方式从头插法改为尾插法，那么整个先序遍历结果就会反转：(right->left->root)，可以发现此时的反先序遍历(right->left->root)已经和后序遍历(left->right->root)十分相似了，那么我们只需要再对反先序遍历优先访问的子树从左子树改为右子树，那么我们就可以得到后序遍历(left->right->root)。
## java代码
```java
public List postorder(TreeNode root){
    TreeNode node = new TreeNode();
    Stack stack = new Stack();
    List list = new LinkedList();
    while(!stack.isEmpty() || root!=null){
        if(root!=null){
            //头插法
            List.addFirst(root.val);
            if(root.left!=null)
                stack.push(root.left);
            //优先访问右子树
            root = root.right;
        }else {
            root = stack.pop();
        }
    }
}
```