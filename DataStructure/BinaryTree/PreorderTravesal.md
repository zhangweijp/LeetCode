# 二叉树的先序遍历 中左右
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