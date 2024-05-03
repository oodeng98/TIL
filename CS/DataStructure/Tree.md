# Tree

트리 구조란 그래프의 일종으로, 여러 노드가 한 노드를 가리킬 수 없는 구조  
간단하게는 회로가 없고, 서로 다른 두 노드를 잇는 길이 하나뿐인 그래프를 트리라고 부른다.

## 문제 예시

주어진 입력 값으로 트리를 구성하고, 구성된 트리를 전위순회하고 방문한 노드의 번호를 출력하시오.  
첫 줄에는 전체 테스트 케이스의 수(T), 두 번째 줄에는 노드의 총 수(nodeNum), 간선의 총 수(edgeNum)가 주어진다.  
그 다음 줄에는 간선이 나열 된다. 간선은 그것을 이루는 두 정점으로 표기된다. 간선은 항상 “부모 자식” 순서로 표기 된다.  
예를 들어 “1 2”는 정점 1과 2를 잇는 간선을 의미하며 1이 부모 2가 자식을 의미한다.  
부모는 최대 2개의 자식 노드를 가지며, 최대 노드의 개수는 10000개이다.
|Input|Output|
|---|---|
3 // Testcase 수
13 12 // N: 노드의 총 수, E: 간선의 총 수
1 2 1 3 2 4 3 5 3 6 4 7 7 12 5 9 5 8 6 11 6 10 11 13 // 간선 정보 (“부모 자식” 순서)|1 2 4 7 12 3 5 9 8 6 11 13 10
10 9
1 2 1 3 3 4 4 5 5 6 6 7 7 8 8 9 9 10|1 2 3 4 5 6 7 8 9 10
50 49
31 7 2 17 27 32 14 30 1 21 45 26 44 27 39 11 26 3 48 6 3 44 2 49 42 13 48 8 23 33 11 10 8 42 41 31 17 4 8 22 25 23 21 41 28 25 13 16 46 2 31 35 42 19 32 18 27 50 45 15 28 20 46 28 44 40 40 5 15 48 9 34 1 46 17 29 35 36 21 45 14 37 23 14 6 39 11 9 19 24 26 47 16 38 40 12 47 43|1 21 41 31 7 35 36 45 26 3 44 27 32 18 50 40 5 12 47 43 15 48 6 39 11 10 9 34 8 42 13 16 38 19 24 22 46 2 17 4 29 49 28 25 23 33 14 30 37 20

## 구현

```python
class Tree:
    MAX_CHILD_NUM = 2

    class TreeNode:
        def __init__(self, parent):
            self.parent = parent
            self.child = [-1] * Tree.MAX_CHILD_NUM

    def __init__(self, nodeNum):
        self.nodeNum = nodeNum
        self.treenode = [0] * (nodeNum + 1)

        for i in range(1, nodeNum + 1):
            self.treenode[i] = self.TreeNode(-1)

    def addChild(self, parent, child):
        found = -1

        for i in range(0, self.MAX_CHILD_NUM):
            if self.treenode[parent].child[i] == -1:
                found = i
                break

        if found == -1:
            return

        self.treenode[parent].child[found] = child
        self.treenode[child].parent = parent

    def getRoot(self):
        for i in range(1, self.nodeNum):
            if self.treenode[i].parent == -1:
                return i

        return -1

    def preOrder(self, root):

        print(root, end=' ')

        for i in range(0, self.MAX_CHILD_NUM):
            child = self.treenode[root].child[i]
            if child != -1:
                self.preOrder(child)


def main():
    T = int(input())

    for test_case in range(1, T + 1):
        node, edge = map(int, input().split())

        tree = Tree(node)

        numbers = [int(num) for num in input().split()]

        for i in range(0, edge * 2, 2):
            parent = numbers[i]
            child = numbers[i + 1]
            tree.addChild(parent, child)

        root = tree.getRoot()
        print("#%d" % test_case, end=' ')
        tree.preOrder(root)
        print()
```

````java
import java.util.Scanner;

class Tree {

	static final int MAX_CHILD_NUM = 2;

	class TreeNode {
		int parent;
		int []child = new int[MAX_CHILD_NUM];
		public TreeNode(int parent)
		{
			this.parent = parent;
			for (int i = 0; i < MAX_CHILD_NUM; i++)
			{
				child[i] = -1;
			}
		}
	}

	TreeNode []treenode;
	int nodeNum;

	public Tree(int nodeNum) {
		this.nodeNum = nodeNum;
		treenode = new TreeNode[nodeNum+1];
		for (int i = 0; i <= nodeNum; i++)
		{
			treenode[i] = new TreeNode(-1);
		}
	}

	public void addChild(int parent, int child)
	{
		int found = -1;
		for (int i = 0; i < MAX_CHILD_NUM; i++)
		{
			if (treenode[parent].child[i] == -1)
			{
				found = i;
				break;
			}
		}
		if (found == -1) return;

		treenode[parent].child[found] = child;
		treenode[child].parent = parent;
	}

	public int getRoot()
	{
		for (int i = 1; i < nodeNum; i++)
		{
			if (treenode[i].parent == -1)
			{
				return i;
			}
		}
		return -1;
	}

	public void preOrder(int root)
	{
		System.out.printf("%d ", root);

		for (int i = 0; i < MAX_CHILD_NUM; i++)
		{
			int child = treenode[root].child[i];
			if (child != -1)
			{
				preOrder(child);
			}
		}
	}
}

class Solution {

	public static void main(String arg[]) throws Exception {
		Scanner sc = new Scanner(System.in);

		int T = sc.nextInt();

		for (int test_case = 1; test_case <= T; ++test_case)
		{
			int node = sc.nextInt();
			int edge = sc.nextInt();

			Tree tree = new Tree(node);

			for (int i = 0; i < edge; i++)
			{
				int parent = sc.nextInt();
				int child = sc.nextInt();
				tree.addChild(parent, child);
			}
			int root = tree.getRoot();
			System.out.printf("#%d ", test_case);
			tree.preOrder(root);
			System.out.printf("\n");
		}

		sc.close();
	}
}```
````
