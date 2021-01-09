 public boolean remove(K key) throws IllegalNullKeyException {
    if (key == null)
      throw new IllegalNullKeyException();
    TreeNode<K, V> removeNode = this.search(key, this.root);
    if (removeNode == null)
      return false;
    // node is present and must be removed
    delete(removeNode);
    return true;
  }

  private TreeNode<K, V> getPredecessor(TreeNode<K, V> startNode) {
    // assume start node not null as called only when node has two children
    if (startNode.right == null)
      return startNode;
    else
      return getPredecessor(startNode.right);
  }

  private void delete(TreeNode<K, V> removeNode) {
    TreeNode<K, V> parent = removeNode.parent;
    if (parent == null) { // root removal
      if (removeNode.right == null && removeNode.left == null) {
        this.root = null;
      } else if (removeNode.right == null) {// no right, so left becomes root, which should now be
                                            // black
        this.root = removeNode.left;
        if (this.root.color == RED)
          this.root.color = BLACK;
      } else if (removeNode.left == null) {// no left, so right becomes root, which should now be
                                           // black
        this.root = removeNode.right;
        if (this.root.color == RED)
          this.root.color = BLACK;
      } else {
        // find predecessor, replace as root, delete predecessor
        TreeNode<K, V> deleteLeaf = getPredecessor(removeNode);
        this.root.key = deleteLeaf.key;
        this.root.value = deleteLeaf.value;
        delete(deleteLeaf);
      }
    }
    boolean removeNodeRightSide = parent.right.key.equals(removeNode.key);
    TreeNode<K, V> removeNodeSibling = (removeNodeRightSide) ? parent.left : parent.right;
    if (removeNode.right == null && removeNode.left == null) {
      // delete leaf
      if (removeNodeRightSide)
        parent.right = null;
      else
        parent.left = null;
      // if red we don't have to do anything, sibling is also red or null
      // leaf is black, black in sibling tree
      if (removeNode.color == BLACK) {
        if (removeNodeSibling.color == BLACK) { // now sibling is black leaf
          removeNodeSibling.color = RED; // set to red and fix the violations
          TreeNode<K, V> violator = removeNodeSibling;
          while (checkViolationRecent(violator)) {
            violator = fixRedViolation(removeNodeSibling);
          }
        } else { // sibling is red node with two children both black and parent is black
          removeNodeSibling.right.color = RED; // set to red and fix the violations
          removeNodeSibling.left.color = RED; // set to red and fix the violations
          TreeNode<K, V> saveRight = removeNodeSibling.right; // to fix this violation later
          TreeNode<K, V> violator = removeNodeSibling.left;
          while (checkViolationRecent(violator)) {
            violator = fixRedViolation(removeNodeSibling);
          }
          violator = saveRight;
          while (checkViolationRecent(violator)) {
            violator = fixRedViolation(removeNodeSibling);
          }
        }
      }
    } else if (removeNode.right == null) {// single child, only possible for one red child
      // delete internal Node which is black as child red, with the red child which becomes black
      if (removeNodeRightSide) {
        parent.right = removeNode.left;
        parent.right.color = BLACK;
      } else {
        parent.left = removeNode.left;
        parent.left.color = BLACK;
      }
    } else if (removeNode.left == null) {// single child, only possible for one red child
      // delete internal Node which is black as child red, with the red child which becomes black
      if (removeNodeRightSide) {
        parent.right = removeNode.right;
        parent.right.color = BLACK;
      } else {
        parent.left = removeNode.right;
        parent.left.color = BLACK;
      }
    } else { // both children present
      // find predecessor, replace as node, delete predecessor
      TreeNode<K, V> deleteLeaf = getPredecessor(removeNode.left);
      removeNode.key = deleteLeaf.key;
      removeNode.value = deleteLeaf.value;
      delete(deleteLeaf);
    }
  }
