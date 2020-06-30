# 二叉树的中序遍历（中根遍历） 左中右
## java代码
```java
public List ineorder(TreeNode root){
    TreeNode node = new TreeNode();
    Stack stack = new Stack();
    List list = new LinkedList();
    while(!stack.isEmpty() || root!=null){
        if(root==null){
            root = stack.pop();
            list.add(root.val);
            root = root.right;
        } else{
            stack.push(root);
            root = root.left;
        }
    }
}
```