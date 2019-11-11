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

trie는 digital tree 또는 prefix tree 또는 re<b>trie</b>val tree라 불리며, search tree의 종류 중 하나이다.

trie는 문자열을 키로 사용하는 동적 Set 또는 연관 배열을 저장하는 트리의 확장된 구조이다.

예로 위 그림과 같이 tea를 찾으려면 't'를 먼저 찾고 그다음 'e', 'a' 순서대로 찾으면 된다.

### Trie 구현(insert, search)

```python
class TreeNode:
    def __init__(self):
        global ALPHABET_SIZE
        self.children = [None] * ALPHABET_SIZE
        self.isEndOfWord = False


class Trie:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = self.getNode()

    def getNode(self):
        return TreeNode()

    def _getChar2Index(self, ch):
        return ord(ch) - ord('a')

    def insert(self, key: str) -> None:
        """
        Inserts a word into the trie.
        """

        pCrawl = self.root
        length = len(key)
        for level in range(length):
            index = self._getChar2Index(key[level])

            # if current character is not present
            if not pCrawl.children[index]:
                pCrawl.children[index] = self.getNode()

            pCrawl = pCrawl.children[index]

        pCrawl.isEndOfWord = True

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """

        pCrawl = self.root
        length = len(word)

        for level in range(length):
            index = self._getChar2Index(word[level])
            if not pCrawl.children[index]:
                return False
            pCrawl = pCrawl.children[index]

        return pCrawl != None and pCrawl.isEndOfWord

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        pCrawl = self.root
        length = len(prefix)

        for level in range(length):
            index = self._getChar2Index(prefix[level])
            if not pCrawl.children[index]:
                return False

            pCrawl = pCrawl.children[index]

        return pCrawl != None

    # if __name__ == '__main__':
    #     obj = Trie()
    #     word = "apple"
    #     prefix = "app"
    #     obj.insert(word)
    #     print("insert : "+word)
    #     print("search : "+ word + ", result = "+str(obj.search(word)))
    #     print("startsWith : " + prefix + ", result = "+str( obj.startsWith(prefix)))
    #     print("startsWith : " + "the" + ", result = " + str(obj.startsWith("the")))
```

```sh
insert : apple
search : apple, result = True
startsWith : app, result = True
startsWith : the, result = False
```

### 참고

* [Implement Trie (Prefix Tree)- test code](https://leetcode.com/problems/implement-trie-prefix-tree/)
* [GreekforGreek-Trie](https://www.geeksforgeeks.org/trie-insert-and-search/)
