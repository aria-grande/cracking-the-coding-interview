# Linked Lists

## [Linked List](https://en.wikipedia.org/wiki/Linked_list)

노드들을 연결하여 데이터를 저장하는 자료구조이고, 각 노드는 데이터와 다음/이전 노드를 가리키는 포인터로 구성되어 있다. 노드의 중간지점에서도 자료의 추가와 삭제가 O(1) 시간 내에 가능하다. 하지만, 특정 위치의 데이터를 검색해 내는데에는 O(n)의 시간이 걸린다. Linked List의 종류에는 대표적으로 단방향 연결리스트([Singly linked list](https://en.wikipedia.org/wiki/Linked_list#Singly_linked_list)), 양방향 연결리스트([Doubly linked list](https://en.wikipedia.org/wiki/Linked_list#Doubly_linked_list))가 있다.<br/>

##### linked list 구현
###### 삽입

```java
class Node {
    int data;
    Node next;
    
    public Node(int data) {
      this.data = data;
    }
    
    void appendToTail(Node n) {
      Node end = this;
      while(end.next != null) {
        end = end.next;
      }
      end.next = n;
    }
}
```

###### 삭제

```java
Node delete(Node head, int d) {
  if (head.data == d) {
    return head.next;
  }
  Node node = head;
  while(node.next != null) {
    if(node.next.data == d) {
      node.next = node.next.next;
      return head;
    }
    node = node.next;
  }
  return head;
}
```

#### linked list vs. array
| 비교 | linked list | array | dynamic array |
|:---:|:-----------:|:-----:|:-------------:|
|길이|유동적|고정적|유동적|
|index|O(n)|O(1)|O(1)|
|insert/delete at beggining|O(1)|O(n)|O(n)<br/>(shifting)|
|insert/delete in middle|search time + O(1)|O(1)|O(n)<br/>(shifting)|
|insert/delete at end|O(1)|O(1)|O(1)|


연결리스트에 관한 질문을 받을 때에는, 단방향인지 양방향인지 확실히 해두어야 한다.
