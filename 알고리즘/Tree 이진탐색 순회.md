# Tree 이진탐색 순회

```typescript
class TreeNode<T> {
  data: T;
  children: TreeNode<T>[];

  constructor(data: T) {
    this.data = data;
    this.children = [];
  }

  // 자식 노드 추가
  addChild(child: TreeNode<T>) {
    this.children.push(child);
  }
}

class Tree<T> {
  root: TreeNode<T> | null;

  constructor(data?: T) {
    this.root = data ? new TreeNode(data) : null;
  }

  // 전위 순회 (Preorder Traversal)
  traversePreOrder(node: TreeNode<T> | null = this.root): void {
    if (!node) return;
    console.log(node.data);
    node.children.forEach((child) => this.traversePreOrder(child));
  }

  // 중위 순회 (Inorder Traversal)
  traverseInOrder(node: TreeNode<T> | null = this.root): void {
    if (!node) return;
    // 자식이 여러개일 경우, 왼쪽 자식부터 차례대로 순회
    const mid = Math.floor(node.children.length / 2);
    // 왼쪽 자식들 순회
    node.children.slice(0, mid).forEach((child) => this.traverseInOrder(child));
    // 현재 노드 순회
    console.log(node.data);
    // 오른쪽 자식들 순회
    node.children.slice(mid).forEach((child) => this.traverseInOrder(child));
  }

  // 후위 순회 (Postorder Traversal)
  traversePostOrder(node: TreeNode<T> | null = this.root): void {
    if (!node) return;
    node.children.forEach((child) => this.traversePostOrder(child));
    console.log(node.data);
  }
}

// 트리 사용 예제
const tree = new Tree<string>('Root');
const child1 = new TreeNode('Child1');
const child2 = new TreeNode('Child2');
const child3 = new TreeNode('Child3');

tree.root?.addChild(child1);
tree.root?.addChild(child2);
tree.root?.addChild(child3);

child1.addChild(new TreeNode('Child1.1'));
child1.addChild(new TreeNode('Child1.2'));
child2.addChild(new TreeNode('Child2.1'));

console.log("Preorder Traversal:");
tree.traversePreOrder();

console.log("Postorder Traversal:");
tree.traversePostOrder();

console.log("Inorder Traversal:");
tree.traverseInOrder();
```

tree
