---
title: Aho-Corasick Automata
tags: algo
description: 
---

# Aho-Corasick Automata


## Knuth-Morris-Pratt Algorithm

Given a text txt[0..n-1] and a pattern pat[0..m-1], write a function search(char pat[], char txt[]) that prints all occurrences of pat[] in txt[]. You may assume that n > m.

### Failure function

- Prefix array
	- $\pi_i$ is the length of the longest prefix of the substring $S_{0...i}$ which is also its suffix

### Matching

![image alt](https://miro.medium.com/max/700/1*zih18lLx2ibPlf1FgoNj-A.gif)

- Complexity: $O(n)$


---


## Trie

![image alt](https://i.imgur.com/5JsVBeY.png)


### Benefits

- Complexity:
	- Build: $O(\Sigma\ length)$
	- Insert: $O(length)$
	- Search: $O(length)$
	
```cpp
struct TrieNode {
	struct TrieNode[ALPHABET_SIZE] *children;
	bool isEnd;
};
```


---

## Aho-Corasick Automata

Given a string $T$ and $k$ nonempty strings $P_1$, …, $P_k$, find all occurrences of $P_1$, …, $P_k$ in $T$.


### Suffix Links (Failure Links)

If we try to perform a transition using a letter, and there is no corresponding edge in the trie, then we nevertheless must go into some state.

A suffix link for a vertex $p$ is a edge that points to the longest proper suffix of the string corresponding to the vertex $p$. 

![](https://i.imgur.com/JSmhn7a.png)


#### Constructing suffix links

If we want to find a suffix link for some vertex $v$, then we can go to the ancestor $p$ of the current vertex (let $c$ be the letter of the edge from $p$ to $v$), then follow its suffix link, and perform from there the transition with the letter $c$.

- Recursion & memoization
	- Linear time
	
```cpp
int get_link(int v) {
    if (t[v].link == -1) {
        if (v == 0 || t[v].p == 0)
            t[v].link = 0;
        else
            t[v].link = go(get_link(t[v].p), t[v].pch);
    }
    return t[v].link;
}

int go(int v, char ch) {
    int c = ch - 'a';
    if (t[v].go[c] == -1) {
        if (t[v].next[c] != -1)
            t[v].go[c] = t[v].next[c];
        else
            t[v].go[c] = v == 0 ? 0 : go(get_link(v), ch);
    }
    return t[v].go[c];
} 
```
([Full code](https://cp-algorithms.com/string/aho_corasick.html#toc-tgt-1))


### Output links

![](https://i.imgur.com/0mg5nzH.png)

### Matching

- Complexity: The runtime of the match phase is then $\Theta(m + z)$, with the $m$ term coming from the string scanning and the $z$ term coming from the matches.

### Other Uses

- Lexicographical smallest string of a given length that doesn't match any given strings
	- A set of strings and a length L is given. We have to find a string of length L, which does not contain any of the string, and derive the lexicographical smallest of such strings.
	- The vertices from which we can reach a leaf vertex are the states, at which we have a match with a string from the set.
- Shortest string containing all given strings
- Lexicographical smallest string of length L containing k strings

---

## Sources

- http://web.stanford.edu/class/archive/cs/cs166/cs166.1166/lectures/02/Small02.pdf
- https://medium.com/nlp-tsupei/kmp%E7%AE%97%E6%B3%95%E8%A9%B3%E8%A7%A3-1b1050a45850
- https://github.com/detaomega/practice/blob/master/algorithm/trie.cpp
- https://cp-algorithms.com/string/aho_corasick.html