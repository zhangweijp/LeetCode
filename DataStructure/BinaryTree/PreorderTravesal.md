# 二叉树的先序遍历（先根遍历） 中左右
## java代码
```java
public List preorder(TreeNode root){
    TreeNode node = new TreeNode();
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