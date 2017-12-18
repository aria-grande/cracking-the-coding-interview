# Arrays and Strings

## [Hash table](https://en.wikipedia.org/wiki/Hash_table)
key-value 형태의 자료구조로, 효율적인 탐색을 위해 `function hash(key)`가 value를 찾을 **index로 활용**된다.

| 연산 |  평균 복잡도 | 최악의 경우 복잡도 |
|----------|:-------------:|:------:|
| Insert |  O(1) | O(n) |
| Delete |  O(1) | O(n) |
| Search |  O(1) | O(n) |

```
[Hash Collision]

효율적이지 않은 hash function을 고를 경우, 하나 이상의 키에 대해 같은 index 값이 나오는 현상으로, 
value가 override되어 올바른 값을 찾을 수 없게 되는 문제이다.

가능한 모든 키 값을 고려하려면, 극도로 큰 배열을 만들어야 하는데, 
Linked List나 BST(Binary Search Tree)를 이용함으로써 배열의 크기를 줄일 수 있다.

(hash(key) % array_length)에 linked list(or bst)를 저장하여, 해당 노드를 일일히 비교하는 방법이 있다.

O(1) 탐색 시간을 유지하기 위해서는 최적화된 hash function을 선택하는 것이 매우 중요하다. 
최악의 경우, 해당 인덱스에 대해 모든 노드를 비교하게 된다면, 연산 복잡도는 O(n)이 될 것이다.
```


## [ArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)
동적으로 크기를 조정되는 배열을 구현한 자료구조로, 배열이 가득 찰때마다 그 크기가 두배씩 늘어나도록 구현된다.

| 연산 |  평균 복잡도 | 최악의 경우 복잡도 |
|----------|:-------------:|:------:|
| add |  O(1) | O(n) |
| remove |  O(1) | O(1) |
| get |  O(1) | O(1) |


## [StringBuffer](https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuffer.html)
가변성이 있는 문자들의 시퀀스를 저장하는 자료구조로, thread-safe하다.
자바에서 String은 객체이므로 concatenation할 경우, 매번 새로운 객체를 생성한다.  하여, 가변성이 높은 String을 사용할때에는 StringBuffer를 선택하는 것이 좋다.
```java
public String joinWords(String[] words) {
  /* bad way */
  String sentence = "";
  // 문자열의 길이가 x일 경우, 복사에 걸리는 시간은 O(x+2x+...nx) = O(xn^2)
  for (String word : words) {
      sentence += word;
  }
  return sentence;
  
  /* good way */
  StringBuffer sentence = new StringBuffer();
  for (String word : words) {
    sentence.append(word);
  }
  return sentence.toString();
}
```

---------------------------
## Problems
*1.1 문자열에 포함된 문자들이 전부 유일한지 검사하는 알고리즘을 구현하라.*
<details>
  <summary>확실히 할 것</summary>
  
  > 인풋의 제약조건은?
</details>
<details>
 <summary>Solution1</summary>
인풋은 영소문자로 가정하고, 26개 characters의 출현 여부를 담은 배열을 이용한다.
  
```java
boolean isUnique(char[] input) {
  if (input.length > 26) { return false; }
  boolean[] chars = new boolean[26];
  for(char c : input) {
    int index = c - 'a';
    if(chars[index]) {
      return false;
    }
    chars[index] = true;
  }
  return true;
}
```
| category | complexity |
|----------|:-----:|
|space |O(1)|
|time |O(n)|
</details>
<details>
 <summary>Solution2</summary>
비트연산을 이용한다.
  
```java
boolean isUnique(char[] input) {
  if (input.length > 26) { return false; }
  int checker = 0;
  for(char c : input) {
    int val = c - 'a';
    if ((checker & (1 << val)) > 0) {
      return false;
    }
    checker |= (1 << val);
  }
  return true;
}
```
| category | complexity |
|----------|:-----:|
|space |O(1)|
|time |O(n)|
</details>
<br/>
*1.3 문자열 두개를 입력 받아, 그중 하나가 다른 하나의 순열인지 판별하는 메서드를 작성하라.*
<details>
  <summary>확실히 할 것</summary>
  
  > 인풋의 제약조건은?
</details>
<details>
 <summary>Solution1</summary>
  인풋은 영소문자로 가정하고, 26개 characters의 출현 갯수를 담은 배열을 이용한다.
  두 인풋의 길이가 같을 경우, 한 문자의 갯수가 많으면 반드시 다른 한 문자의 갯수가 적다는 논리를 이용한다.

```java
boolean isPermutation(char[] s1, char[] s2) {
  if (s1.legnth != s2.length) { return false; }
  int[] chars = new int[26];
  for(char c1 : s1) {
    chars[c1-'a'] += 1;
  }
  for(char c2 : s2) {
    char[c2-'a'] -= 1;
    if (char[c2-'a'] < 0) {
      return false;
    }
  }
  return true;
}
```
| category | complexity |
|----------|:-----:|
|space |O(1)|
|time |O(n)|
</details>
