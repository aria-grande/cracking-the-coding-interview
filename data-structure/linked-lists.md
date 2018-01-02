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


## Problems

### 2.1 unsorted linked list에서 중복 문자를 제거하는 메서드를 작성하라

<details>
  <summary>Suggest Constraints</summary> 
  
> 구성 문자는 영소문자로 가정한다.<br/>
> 단방향 연결 리스트로 가정한다.<br/>
> 중복이 있다면 최초에 출현한 문자를 제외하고 나머지를 지우는 것으로 가정한다.
</details>
<details>
  <summary>Solution 1</summary>
  해당 영소문자의 출현 여부를 담고 있는 임시 버퍼를 사용하여 해결한다.
 
 ```java
 Node unique(Node head) {
   if (head == null) { return; }
   boolean[] chars = new boolean[26];
   Node n = head;
   chars[n.data-'a'] = true;
   while(n.next != null) {
     int idx = n.next.data - 'a';
     if(chars[idx]) {
        n.next = n.next.next;
     }
     else {
        chars[idx] = true;
        n = n.next;
     }
   }
   return head;
 }
 ```
| category | complexity |
|----------|:-----:|
|space |O(1)|
|time |O(n)|
</details>

<details>
  <summary>Solution 2</summary>
  임시 버퍼를 사용하지 않고 해결해보자.
    
 ```java
Node unique(Node head) {
    Node n = head;
    while(n != null) {
        Node runner = n;
        while(runner.next != null) {
            if(n.data == runner.next.data) {
                runner.next = runner.next.next;
            }
            else {
                runner = runner.next;
            }
        }
        n = n.next;
    }
    return head;
 }
 ```
| category | complexity |
|----------|:-----:|
|space |-|
|time |O(n^2)|
</details>
<br/>

### 2.2 단방향 연결 리스트에서, 뒤에서 k번째 원소를 찾는 알고리즘을 구현하라.

<details>
  <summary>Suggest Constraints</summary> 
 
>k가 1일 경우, 리스트의 마지막 값을 의미한다.
</details>
<details>
  <summary>Solution 1</summary>
   runner 기법(부가포인터 기법)을 사용한다.
    
 ```java
Node findFromBehind(Node head, int k) {
    if(k < 1) { 
        return null; 
    }
    // set runner
    Node runner = head;
    for(int i = 0; i < k-1; ++i) {
        if(runner == null) { 
            return null; 
        }
        runner = runner.next;
    }
    if(runner == null) { 
        return null; 
    }
    // start to find last k node
    Node node = head;
    while(runner.next != null) {
        node = node.next;
        runner = runner.next;
    }
    return node;
 }
 ```
| category | complexity |
|----------|:-----:|
|space |O(1)|
|time |O(n)|
</details>
<br/>
