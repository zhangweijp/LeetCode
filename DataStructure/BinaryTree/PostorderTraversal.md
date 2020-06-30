# 二叉树的后序遍历（后根遍历） 左右中
## java代码
```java
public List postorder(TreeNode root){
    TreeNode node = new TreeNode();
    Stack stack = new Stack();
    List list = new LinkedList();
    while(!stack.isEmpty() || root!=null){
        if(root!=null){
            List.add(root.val);
            if(root.left!=null)
                stack.push(root.left);
            root = root.right;
        }else {
            root = stack.pop();
        }
    }
}
```