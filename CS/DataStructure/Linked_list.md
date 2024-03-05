# Linked List
Linked List는 각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식으로 데이터를 저장하는 자료 구조  

## 문제 예시
주어진 N(2<= N <=100)개의 수를 순서대로 Linked List에 넣은 후, 2개의 간격으로 하나씩 데이터를 뺄 때 마지막에 남아 있는 데이터를 출력하시오.  
Ex) 1 2 3 4 5 -> 2 3 4 5 -> 2 3 5 -> 2 5 -> 2  
|Input|Output|
|---|---|
2 //테스트 케이스 수
5 //입력 수
1 2 3 4 5 //입력 데이터|2
6
|1 2 3 4 5 6|5|

```python
class ListNode:
    data = 0
    prev = None
    next = None

    def __init__(self):
        self.data = 0
        self.prev = self
        self.next = self

    def appendListNode(head, data):
        node = ListNode()
        node.data = data
        if head == None:
            head = node
        else:
            last = head.prev
            last.next = node
            head.prev = node
            node.prev = last
            node.next = head
        return head

    def removeListNode(head, node):
        if head == head.next:
            return None
        else:
            prevNode = node.prev
            nextNode = node.next
            prevNode.next = nextNode
            nextNode.prev = prevNode
            if head == node:
                return nextNode
            else:
                return head


def main():
    T = int(input())

    for test_case in range(1, T + 1):
        N = int(input())
        data = list(map(int, input().split()))

        head = None

        for i in range(N):
            head = ListNode.appendListNode(head, data[i])

        node = head

        while head != head.next:
            nextNode = node.next

            head = ListNode.removeListNode(head, node)
            node = nextNode.next.next

        print("#%d %d" % (test_case, head.data))
```
```java
import java.util.Scanner;


class ListNode {
	int data;
	ListNode prev;
	ListNode next;
	
	public ListNode()
	{
		data = 0;
		prev = this;
		next = this;
	}

	public static ListNode appendListNode(ListNode head, int data)
	{
		ListNode node = new ListNode();
		node.data = data;
		if (head == null)
		{
			head = node;
		}
		else 
		{
			ListNode last = head.prev;
			last.next = node;
			head.prev = node;
			node.prev = last;
			node.next = head;
		}
		return head;
	}
	
	public static ListNode removeListNode(ListNode head, ListNode node)
	{
		if (head == head.next)
		{
			return null;
		}
		else 
		{
			ListNode prevNode = node.prev;
			ListNode nextNode = node.next;
			prevNode.next = nextNode;
			nextNode.prev = prevNode;
			return (head == node) ? nextNode : head;
		}
		
	}
}
	

class Solution
{
	public static void main(String args[]) throws Exception
	{
		Scanner sc = new Scanner(System.in);
		int T = sc.nextInt();
		for (int test_case = 1; test_case <= T; test_case++)
		{
			ListNode head = null;
			int N = sc.nextInt();
			for (int i = 0; i < N; i++)
			{
				int data = sc.nextInt();
				head = ListNode.appendListNode(head, data);
			}
			ListNode node = head;
			while(head != head.next)
			{
				ListNode nextNode = node.next;
				head = ListNode.removeListNode(head, node);
				node = nextNode.next.next;
			}
			System.out.printf("#%d %d\n", test_case, head.data);
		}
		sc.close();
	}
}```