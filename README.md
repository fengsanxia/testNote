# 第三章	Spring Boot进阶，开发社区核心功能



## 3.1 过滤敏感词

**• 前缀树**

\- 名称：Trie、字典树、查找树 

\- 特点：查找效率高，消耗内存大 （<font color='red'>空间换时间</font>）

\- 应用：字符串检索、词频统计、字符串排序等 

**• 敏感词过滤器** 

\- 定义前缀树 

\- 根据敏感词，初始化前缀树 

\- 编写过滤敏感词的方法



字典树代码：

```java
public class Code_01_TrieTree {

	public static class TrieNode {
		public int path;
		public int end;
		public TrieNode[] nexts;

		public TrieNode() {
			path = 0;
			end = 0;
			nexts = new TrieNode[26];
		}
	}

	public static class Trie {
		private TrieNode root;

		public Trie() {
			root = new TrieNode();
		}

		public void insert(String word) {
			if (word == null) {
				return;
			}
			char[] chs = word.toCharArray();
			TrieNode node = root;
			int index = 0;
			for (int i = 0; i < chs.length; i++) {
				index = chs[i] - 'a';
				if (node.nexts[index] == null) {
					node.nexts[index] = new TrieNode();
				}
				node = node.nexts[index];
				node.path++;
			}
			node.end++;
		}

		public void delete(String word) {
			if (search(word) != 0) {
				char[] chs = word.toCharArray();
				TrieNode node = root;
				int index = 0;
				for (int i = 0; i < chs.length; i++) {
					index = chs[i] - 'a';
					if (--node.nexts[index].path == 0) {
						node.nexts[index] = null;
						return;
					}
					node = node.nexts[index];
				}
				node.end--;
			}
		}

		public int search(String word) {
			if (word == null) {
				return 0;
			}
			char[] chs = word.toCharArray();
			TrieNode node = root;
			int index = 0;
			for (int i = 0; i < chs.length; i++) {
				index = chs[i] - 'a';
				if (node.nexts[index] == null) {
					return 0;
				}
				node = node.nexts[index];
			}
			return node.end;
		}

		public int prefixNumber(String pre) {
			if (pre == null) {
				return 0;
			}
			char[] chs = pre.toCharArray();
			TrieNode node = root;
			int index = 0;
			for (int i = 0; i < chs.length; i++) {
				index = chs[i] - 'a';
				if (node.nexts[index] == null) {
					return 0;
				}
				node = node.nexts[index];
			}
			return node.path;
		}
	}

}
```

