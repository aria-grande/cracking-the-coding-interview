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
### 1.1 문자열에 포함된 문자들이 전부 유일한지 검사하는 알고리즘을 구현하라.
<details>
  <summary>Suggest Constraints</summary>
  
> 인풋의 제약조건은 영소문자이다.
</details>
<details>
 <summary>Solution1</summary>
26개 characters의 출현 여부를 담은 배열을 이용한다.
  
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

### 1.3 문자열 두개를 입력 받아, 그중 하나가 다른 하나의 순열인지 판별하는 메서드를 작성하라.
<details>
  <summary>Suggest Constraints</summary>
  
> 인풋의 제약조건은 영소문자이다.
</details>
<details>
 <summary>Solution1</summary>
  26개 characters의 출현 갯수를 담은 배열을 이용한다.
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
<br/>

### 1.4 주어진 문자열 내의 모든 공백을 '%20'으로 바꾸는 메서드를 작성하라.
<details>
  <summary>Suggest Constraints</summary> 
  
> 문자열 끝에 추가로 필요한 문자들을 더할 수 있는 충분한 공간이 있다고 가정한다.<br/>
> 공백을 포함한 문자열의 길이도 인자로 주어진다고 가정한다.
</details>
<details>
  <summary>Solution 1</summary>
  
 ```java
String replaceBlank(char[] str, int length) {
  StringBuffer sb = new StringBuffer();
  for(char c : str) {
    String newVal = ' '.equals?(c) ? "%20" : String.valueOf(c);
    sb.append(newVal);
  }
  return sb.toString();
}
```
| category | complexity |
|----------|:-----:|
|space |O(n)|
|time |O(n)|
</details>
<details>
  <summary>Solution 2</summary>
  추가적인 자료구조를 사용하지 않고 풀어보자.
  
```java
String replaceBlank(char[] str, int length) {
  int blankCnt = 0;
  int ep = -1;
  // 1. 치환해야 할 공백 갯수를 구한다.
  for(int i = 0; i < length; ++i) {
    char c = str[i];
    if(' ' == c) {
      if(2 * blankCnt == length - i) {
        ep = i - 1;
        break;
      }
      ++blankCnt;
    }
  }
  // 2. 마지막 치환 공백을 만나면 치환을 시작하자.
  for(int j = ep; j >= 0; --j) {
    if(' ' == str[j]) {
      int point = j + 2 * --blankCnt;
      str[point+2] = '%';
      str[point+1] = '0';
      str[point]   = '2';
    }
    else {
      str[j + 2 * blankCnt] = str[j];
    }
  }
  return String.valueOf(str);
}
```
| category | complexity |
|----------|:-----:|
|space |O(1)|
|time |O(n)|
</details>
<br/>

### 1.5 같은 문자가 연속으로 반복될 경우, 그 횟수를 사용해 문자열을 압축하는 메서드를 구현하라. 가령 압축해야 할 문자열이 aabccccaaa라면, a5b1c4와 같이 압축되어야 한다. 압축 결과로 만들어지는 문자열이 원래 문자열보다 길이가 길 경우, 이 메서드는 원래 문자열을 반환해야 한다.
<details>
  <summary>Suggest Constraints</summary> 
  
> 문자열의 구성은 영소문자로 가정한다.
</details>
<details>
  <summary>Solution 1</summary>
  
 ```java
String compress(char[] str) {
  StringBuffer sb = new StringBuffer();
  int[] chars = new int[26];
  for(char c : str) {
    chars[c-'a'] += 1;
  }
  
  int alphabetCnt = 0;
  for(int i = 0; i < 26; ++i) {
    char alphabet = (char)('a'+i);
    int count = chars[i];
    if (count > 0) {
      ++alphabetCnt;
      sb.append(alphabet);
      sb.append(count);
    }
  }
  return alphabetCnt * 2 > str.length ? String.valueOf(str) : sb.toString();
}
```
| category | complexity |
|----------|:-----:|
|space |O(n)|
|time |O(n)|
</details>

### 1.6 이미지를 표현하는 NxN 행렬이 있다. 이미지의 각 픽셀은 4바이트로 표현된다. 이때, 이미지를 90도 회전시키는 메서드를 작성하라. 부가적인 행렬을 사용하지 않고서도 할 수 있겠는가?
<details>
  <summary>Suggest Constraints</summary> 
  
> 4바이트이므로, int[][]를 인풋으로 취급한다.
</details>
<details>
  <summary>Solution 1</summary>
  
이미지의 각 픽셀이 4바이트라는 것은, 하나의 셀을 int type으로 표현 가능 하다는 것으로 해석할 수 있다.<br/>
행렬의 outer layer 부터 회전 시키면 부가적인 행렬을 사용하지 않아도 된다.<br/>
d를 depth라고 할 때, 0 <= d <= n/2 이며, img[d][d]~img[d][n-d-1]를 회전시키면 된다.<br/>

```
ex)
[ 1, 2, 3 ]    [ 7, 4, 1 ]
[ 4, 5 ,6 ] -> [ 8, 5, 2 ]
[ 7, 8, 9 ]    [ 9, 6, 3 ]
1. img[0][0] -> img[0][2] -> img[2][2] -> img[2][0]
2. img[0][1] -> img[1][2] -> img[2][1] -> img[1][0]
```
  
 ```java
int[][] rotate(int[][] img) {
  final int N = img.length;
  for(int d = 0; d < N/2; ++d) {  // d means depth
    for(int i = d; i < N-d-1; ++i) {
      int tmp = img[d][i];
      img[d][i] = img[N-i-1][d];          // left -> top
      img[N-i-1][d] = img[N-d-1][N-i-1];  // bottom -> left
      img[N-d-1][N-i-1] = img[i][N-d-1];  // right -> bottom
      img[i][N-d-1] = tmp;                // top -> right
    }
  }
  return img;
}
```
| category | complexity |
|----------|:-----:|
|space |O(1)|
|time |O(n^2)|
</details>

### 1.7 MxN 행렬의 한 원소가 0일 경우, 해당 원소가 속한 행과 열의 모든 원소를 0으로 설정하는 알고리즘을 작성하라.
<details>
  <summary>Suggest Constraints</summary> 
  
> 행렬을 구성하는 것은 정수로 한정한다.
</details>
<details>
  <summary>Solution 1</summary>
  
  행렬을 순차적으로 순회하며 0이 발견되는 경우, 해당 열과 행의 index를 저장해둔다.
  index를 담고있는 배열을 순회하며 행/열을 0으로 만든다.
 
 ```java
int[][] bomb(int[][] matrix, final int M, final int N) {
  Set<Integer> rowIndices = new HashSet<Integer>();
  Set<Integer> colIndices = new HashSet<Integer>();
  for(int i = 0; i < M; ++i) {
    for(int j = 0; j < N; ++j) {
      if (matrix[i][j] == 0) {
        rowIndices.add(i);
        colIndices.add(j);
      }
    }
  }
  rowIndices.forEach(r -> {
    for(int k = 0; k < N; ++k) {
      matrix[r][k] = 0;
    }
  });
  colIndices.forEach(c -> {
    for(int l = 0; l < M; ++l) {
      matrix[l][c] = 0;
    }
  });
  return matrix;
}
```
| category | complexity |
|----------|:-----:|
|space |O(M+N) (for HashSet)|
|time |O(n^2)|
</details>

<details>
  <summary>Solution 2</summary>
  
Solution 1의 경우, rowIndices.forEach에 의해 0으로 바뀐 원소라 해도, colIndices.forEach를 실행하면서 다시 한번 바뀔 수 있다. 조금 더 반복을 줄여볼 수는 없을까?

 ```java
int[][] bomb(int[][] matrix, final int M, final int N) {
  boolean[] rows = new boolean[M];
  boolean[] cols = new boolean[N];
  for(int i = 0; i < M; ++i) {
    for(int j = 0; j < N; ++j) {
      if (matrix[i][j] == 0) {
        rows[i] = true;
        cols[j] = true;
      }
    }
  }
  
  for(int i2 = 0; i2 < M; ++i2) {
    for(int j2 = 0; j2 < N; ++j2) {
      if (rows[i2] || cols[i2]) {
        matrix[i2][j2] = 0;
      }
    }
  }
  return matrix;
}
```
| category | complexity |
|----------|:-----:|
|space |O(M+N)|
|time |O(n^2)|
</details>
