---
title: Trie 자료구조
date: 2019-03-13 19:11:06
category: Data-Structure
---
## Trie 자료구조
![trie](https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Trie_example.svg/1920px-Trie_example.svg.png)

* Goal
  * Trie 자료 구조란
  * Trie 구현

### Trie 자료 구조란

trie는 digital tree 또는 prefix tree 또는 re<span style="color:blue">trie</span>val tree라 불리며, search tree의 종류 중 하나이다.

trie는 문자열을 키로 사용하는 동적 Set 또는 연관 배열을 저장하는 트리의 확장된 구조이다.

예로 위 그림과 같이 tea를 찾으려면 't'를 먼저 찾고 그다음 'e', 'a' 순서대로 찾으면 된다.

### Trie 구현(insert, search)

```sh
           root
        /   \    \
        t   a     b
        |   |     |
        h   p     y
        |   |     |
        e   p     e
        |   |
        i   l
        |   |
        r   e
```

```python
class TreeNode:
    def __init__(self):
        self.children = {}
        self.children_num = {}


class Trie:

    def __init__(self):
        self.root = self.getNode()

    def getNode(self):
        return TreeNode()

    def _getChar2Index(self, ch):
        return ord(ch) - ord('a')

    def insert(self, key: str) -> None:
        pCrawl = self.root
        length = len(key)

        for level in range(length):
            index = self._getChar2Index(key[level])

            # if current character is not present
            if index not in pCrawl.children:
                pCrawl.children[index] = self.getNode()
                pCrawl.children_num[index] = 1
            else:
                pCrawl.children_num[index] += 1

            if level == (length-1):
                pCrawl.children_num[index] = 0

            pCrawl = pCrawl.children[index]


    def search(self, word: str) -> bool:
        root = self.root
        length = len(word)

        for level in range(length):
            index = self._getChar2Index(word[level])
            if index not in root.children_num:
                return False

            if level == (length-1) and (root.children_num[index] is 0):
                return True

                root = root.children[index]

    def startsWith(self, prefix: str) -> bool:
        root = self.root
        length = len(prefix)

        for level in range(length):
            index = self._getChar2Index(prefix[level])
            if (index not in root.children_num) or (root.children_num[index] is 0):
                return False

            root = root.children[index]

        return True

    #  if __name__ == '__main__':
    #     obj = Trie()
    #     word = "apple"
    #     prefix = "app"
    #     obj.insert(word)
    #     print("insert : "+word)
    #     print("search : "+ word + ", result = "+str(obj.search(word)))
    #     print("search : " + word + ", result = " + str(obj.search(prefix)))
    #     print("startsWith : " + prefix + ", result = "+str( obj.startsWith(prefix)))
    #     print("startsWith : " + "the" + ", result = " + str(obj.startsWith("the")))
```

root가 처음 가지고 있는 children의 개수는 (ALPHABET_SIZE)이며 `key('a') 경우 index 값은 0이다`

그 `다음 key('p') node는 'a' node children(index : 'p'-'a')`이 된다.

```sh
insert : apple
search : apple, result = False
search : apple, result = False
startsWith : app, result = True
startsWith : the, result = False
```

### 참고

* [Implement Trie (Prefix Tree)- test code](https://leetcode.com/problems/implement-trie-prefix-tree/)
* [GreekforGreek-Trie](https://www.geeksforgeeks.org/trie-insert-and-search/)
