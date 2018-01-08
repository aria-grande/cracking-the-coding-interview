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


### 2.3 단방향 연결 리스트의 중간에 있는 노드 하나를 삭제하는 알고리즘을 구현하라. 삭제할 노드에 대해 접근만 가능하다는 것에 유의하라.

<details>
  <summary>Suggest Constraints</summary> 
 
>입력: 연결리스트 a->b->c->d->e의 노드 c
>결과: 아무것도 반환하지 않고, 결과로 연결리스트가 a->b->d->e면 된다.
</details>
<details>
  <summary>Solution 1</summary>
  삭제할 노드의 데이터를 다음 데이터로 바꿔치자.
 
 ```java
void remove(Node node) {
    if(node == null) { 
        return null; 
    }
    Node next = node.next;
    if (next == null) {
        node = null;
    }
    else {
        node.data = next.data;
        node.next = next.next;
    }
 }
 ```
| category | complexity |
|----------|:-----:|
|space |O(1)|
|time |O(1)|
</details>
<br/>


### 2.5 연결 리스트로 표현된 두 개의 수가 있다고 하자. 리스트의 각 노드는 해당 수의 각 자릿수를 표현한다. 이때 자릿수들은 역순으로 배열되는데, 1의 자릿수가 리스트의 맨 앞에 오도록 배열된다는 뜻이다. 이 두 수를 더하여 그 합을 연결 리스트로 반환하는 함수를 작성하라.
- 예: (7->1->6) + (5->9->2) => (2->1->9)

<details>
    <summary>Suggest Constraints</summary>
> node의 data가 비어있는 경우는 없다고 가정한다.
</details>
<details>
    <summary>Solution 1</summary>
stack overflow가 나지 않을 정도로 input이 들어온다는 가정 하에, 재귀로 풀어본다.
    
```java
static Node sum(Node n1, Node n2, Node result, int val) {
    if (n1 != null || n2 != null) {
        int d1 = (n1 == null) ? 0 : n1.data;
        int d2 = (n2 == null) ? 0 : n2.data;
        int data = d1 + d2 + val;
        int newVal = (data >= 10) ? 1 : 0;
        int newData = data - newVal*10;
        // move result pointer to the end
        Node r = result;
        while(r.next != null) {
            r = r.next;
        }
        r.next = new Node(newData, null);
        return sum((n1 == null ? null : n1.next), (n2 == null ? null : n2.next), result, newVal);
    }
    return result;
}

public static void printNode(Node node) {
    Node n = node;
    while(n != null) {
        System.out.print(n.data);
        n = n.next;
    }
    System.out.println();
}

public static void main(String args[]) {
    // test case 1
    Node n3 = new Node(6, null);
    Node n2 = new Node(1, n3);
    Node n1 = new Node(7, n2);

    Node m3 = new Node(2, null);
    Node m2 = new Node(9, m3);
    Node m1 = new Node(5, m2);

    Node result = sum(n1, m1, new Node(0, null), 0);
    printNode(result.next);
    
    // test case 2
    Node l3 = new Node(6, null);
    Node l2 = new Node(9, l3);
    Node l1 = new Node(2, l2);

    Node k2 = new Node(5, null);
    Node k1 = new Node(3, k2);
    Node result2 = sum(l1, k1, new Node(0, null), 0);
    printNode(result2.next);
}
```

| category | complexity |
|----------|:-----:|
|space |O(n)|
|time |O(n)|
</details>
