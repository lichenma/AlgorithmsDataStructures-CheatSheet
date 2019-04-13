# Algorithms Data Structures - CheatSheet

A compilation of notes that I made when learning these programming topics online - Note a majority of the content and code was taken off of 
online websites so I do not take any ownership of the below. The following is simply notes compiled for learning purposes.


## Getting Started

Currently AlgorithmsDataStructures-CheatSheet contains various text files for notes but is primarily composed of this README file which supports the markdown formatting language. This makes the notes and code look a lot better and supports many awesome features like quick links 
and makes formatting much easier. 


### Prerequisites

In the future I might add a html document which also performs some formatting and if that comes out, ideally the html and css
would be written for web browsers on large screens which support the newest version of bootstrap and html 5.
I use the following application when building and testing html.

```
Chrome Version 71.0.3578.98
```


## Built With

* [Markdown](https://guides.github.com/features/mastering-markdown/) - Syntax styling



## Authors

* **Lichen Ma** - *compiling information* 



## Acknowledgments

* **content belongs to respective creators**


### Data Structures 
1. [Trie](#trie)


### Algorithms 
1. [Dynamic Programming](#dynamicProgramming)
    * [Fibonacci Numbers](#dynamicProgrammingFibonacciNumbers)
2. [Sorting](#sorting)
    * [Bucket Sort](#sortingBucketSort)
    * [Bubble Sort](#sortingBubbleSort)
    * [Insertion Sort](#sortingInsertionSort)
    * [Selection Sort](#sortingSelectionSort)
    * [Heap Sort](#sortingHeapSort)
    * [Merge Sort](#sortingMergeSort)


<br><br>
# Data Structures 

<br><br><br>
---

<a name="trie"></a>
# Trie 

Implement a trie with insert, search, and startsWith methods
```
Example: 
	Trie trie=new Trie(); 

	trie.insert("apple");
	trie.search("apple");		//returns true 
	trie.search("app");		//returns false
	trie.startsWith("app");		//returns true
	trie.insert("app");
	trie.search("app"); 		//returns true

Note 

* You may assume that all inputs consist of lowercase letters a-z
* All inputs are guaranteed to be non-empty strings 
```

<br><br>
## Solution 


Trie (we pronounce "try") or prefix tree is a tree data structure which is used for the retrieval of a
key in a dataset of strings. THere are various applications of this very efficient data structure such
as: 

* Autocomplete
* Spell checker 
* IP routing (Longest prefix matching) 
* T9 predictive text (text on nine keys - used on phones during the late 1990s) 
* Solving word games 


There are several other data structures like balanced trees and hash tables which give us the 
possibility to search for a word in a dataset of strings so why do we use a trie? Althought a hash 
table has O(1) time complexity for looking for a key, it is not efficient in the following operations: 

* Finding all keys with a common prefix
* Enumerating a dataset of strings in lexicographical order

Another reason why the trie outperforms has table, is that as hash table increases in size, there are
lots of hash collisions and the search time complexity could deteriorate to O(n), where n is the 
number of keys inserted. Tries could use less space compared to Hash Table when storing many keys with
the same prefix. In this case using trie has only O(m) time complexity, where m is the key length.
Searching for a key in balanced tree costs O(m log n) time complexity. 


## Trie node structure 

Trie is a rooted tree. Its nodes have the following fields: 

* Maximum of R links to its children, where each link corresponds to one of R character values from
  dataset alphabet. In this article we assume that R is 26, the number of lowercase latin letters 

* Boolean field which specifies whether the node corresponds to the end of the key, or is just a 
  key prefix


```

Root 

		isEnd: false 
	a b c d e f g h i j k l m n o p q r s t u v w x y z
	                      
			      |
  			     
			     link 

	        isEnd: false
	a b c d e f g h i j k l m n o p q r s t u v w x y z

		|

	       link 

	        isEnd: false
	a b c d e f g h i j k l m n o p q r s t u v w x y z

		|

	       link 

	        isEnd: false
	a b c d e f g h i j k l m n o p q r s t u v w x y z

					      |

	       				     link 

	        isEnd: true 


Representation of a key "leet" in trie 
```

```java 
class TrieNode {

    // R links to node children
    private TrieNode[] links;

    private final int R = 26;

    private boolean isEnd;

    public TrieNode() {
        links = new TrieNode[R];
    }

    public boolean containsKey(char ch) {
        return links[ch -'a'] != null;
    }
    public TrieNode get(char ch) {
        return links[ch -'a'];
    }
    public void put(char ch, TrieNode node) {
        links[ch -'a'] = node;
    }
    public void setEnd() {
        isEnd = true;
    }
    public boolean isEnd() {
        return isEnd;
    }
}
```

Two of the most common operations in a trie are insertion of a key and search for a key

## Insertion of a key to a trie

We insert a key by searching into the trie. We start from the root and search a link, which corresponds
to the first key character. There are two cases: 

* A link exists. Then we move down the tree following the link to the next child level. The algorithm
  continues with searching for the next key character. 

* A link does not exist. Then we create a new node and link it with the parent's link matching the 
  current key character. We repeat this step until we encounter the last character of the key, then 
  we mark the current node as an end node and the algorithm finishes. 


```
Inserting "le" into the Trie


		Root
		 
		 1
  
                  \ l
		    
		    2
 
                      \ e
                     
		        3   End of Key "le"


```

```
Inserting "leet" into the Trie 




			Root

			 1

			  \  l

			     2

			       \ e

			         3   End of key "le"

				   \ e
				     
				     4

				       \ t
				        
					 5   End of key "leet"

```


```
Inserting "code" into the Trie 



				Root
				 
				  1

			      c	 / \  l

			       6     2
			     
			  o   /        \  e

			     7            3    End of key "le"
			 
		       d   /                \   e

			 8                    4

		    e  /                        \   t
		      
End of key "code"     9                           5   End of Key "root"


```

Building a Trie from dataset {le, leet, code}


```java
class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Inserts a word into the trie.
    public void insert(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
            char currentChar = word.charAt(i);
            if (!node.containsKey(currentChar)) {
                node.put(currentChar, new TrieNode());
            }
            node = node.get(currentChar);
        }
        node.setEnd();
    }
}
```

**Complexity Analysis**

```
	Time complexity:  O(m)  where m is the key length. In each iteration of this algorithm we 
	 			either create a node in the trie till we reach the end of the key. 
				This takes only m operations 

	Space complexity: O(m)  In the worst case newly inserted key doesn't share a prefix with the 
				keys already inserted in the trie. We have to add m new nodes, which 
				only takes us O(m) space
```


## Search for a Key in a Trie 


Each key is represented in the trie as a path from the root to the internal node or leaf. We start from
the root with the first key character. We examine the current node for a link corresponding to the key
character. There are two cases: 

* A link exists. We move to the next node in the path following this link, and proceed searching for
  the next key character 
* A link does not exist. If there are no available key characters and current node is marked as isEnd
  we return true. Otherwise there are two possible cases in each of them we return false: 

  * There are key characters left, but it is impossible to follow the key path in the trie, and the
    key is missing
  * No key characters left, but current node is not marked as isEnd. Therefore the search key is only
    a prefix of another key in the trie


```
Searching for key "leet" in the Trie

		                 Root
				 
				  1

			      c	 / \  l    <============================== l

			       6     2
			     
			  o   /        \  e   <=========================== e

			     7            3    End of key "le"
			 
		       d   /                \   e   <===================== e

			 8                    4

		    e  /                        \   t   <================= t
		      
End of key "code"     9                           5   End of Key "root"    <============ Key Found


```

```java
class Trie {
    ...

    // search a prefix or whole key in trie and
    // returns the node where search ends
    private TrieNode searchPrefix(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
           char curLetter = word.charAt(i);
           if (node.containsKey(curLetter)) {
               node = node.get(curLetter);
           } else {
               return null;
           }
        }
        return node;
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
       TrieNode node = searchPrefix(word);
       return node != null && node.isEnd();
    }
}
```

**Complexity Analysis**

```
	Time Complexity:   O(m)    In each step of the algorithm we search for the next key character.
	 			   In the worst case the algorithm performs m operations
	Space Complexity:  O(1)
```


## Search for a key prefix in a trie

The approach is very similar to the one we used for searching a key in a trie. We traverse the trie 
from the root, till there are no character left in key prefix or it is impossible to continue the path
in the trie with the current key character. The only difference with the mentioned above search for a 
key algorithm is that when we come to an end of the key prefix, we always return true. We don't need to
conside the isEnd mark of the current trie node, because we are searching for a prefix of a key, not
for a whole key. 


```
Searching for "co" in the Trie 


				Root
				 
				  1

      	c  ==============>    c	 / \  l

			       6     2
			     
 	o  ============>  o   /        \  e

	Prefix Found         7            3    End of key "le"
			 
		       d   /                \   e

			 8                    4

		    e  /                        \   t
		      
End of key "code"     9                           5   End of Key "root"
```

```java
class Trie {
    ...

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        TrieNode node = searchPrefix(prefix);
        return node != null;
    }
}
```

**Complexity Analysis**
```
	Time Complexity:  O(m)
	Space Complexity: O(1)
```



<br><br><br> 

# Algorithms 

<br><br><br>
***
<a name="dynamicProgramming"></a>
# Dynamic Programming 


Dynamic Programming is mainly an optimization over plain recursion. Whenever we see a recursive 
solution that has repeated calls for the same inputs we can optimize it using Dynamic Programming. The
idea is to simply store the results of the subproblems so that we do not have to re-compute them when
needed later. This simple optimization reduces time complexities from exponential to polynomial. For 
example, if we write the simple recursive solution for Fibonacci numbers, we get exponential time 
complexity and if we optimize it by storing solutions of subproblems time complexity reduces to linear.

We break a complex problem up into a collection of simpler subproblems and solve each of those 
subproblems just once and storing their solutions using a memory-based data structure. 





<br><br>
<a name="dynamicProgrammingFibonacciNumbers"></a>
## Fibonacci Numbers 

<br><br>
### Approach 1: Recursion**

```java 
class fibonacci {
	static int fib(int n) {
		if (n<=1){
			return n;
		}
		return fib(n-1) + fib(n-2);
	}

	public static void main(String args[]) {
		int n=9; 
		System.out.println(fib(n));
	}
}
```

Output : 34


**Complexity Analysis** 

```
Time Complexity:	exponential	T(n) = T(n-1) + T(n-2) which is exponential 

Space Complexity: 	O(n) 		if we consider the function call stack size, otherwise O(1)
```

<br><br>

We can observe that this implementation does a lot of repeated work. This is a bad implementation for 
the nth Fibonacci number. 

```

		       	   fib(5)

                     /                \
               fib(4)                fib(3)   
             /        \              /       \ 
         fib(3)      fib(2)         fib(2)   fib(1)
        /    \       /    \        /      \
  fib(2)   fib(1)  fib(1) fib(0) fib(1) fib(0)
  /     \
fib(1) fib(0)
```


<br><br> 
### Approach 2: Dynamic Programming 

```java 
class fibonacci {
	
	static int fib(int n) {
		
		// Declare an array to store Fibonacci numbers 
		int f[] = new int[n+2]; 	// 1 extra to handle case, n = 0 
		int i; 

		//0th and 1st number of the series are 0 and 1 
		f[0] = 0;
		f[1] = 1;

		for (i=2; i<=n; i++) {
			
			//add the previous two numbers in the series and store it
			f[i] = f[i-1] + f[i-2];
		}

		return f[n];
	}

	public static void main(String args[]) {
		
		int n=9; 
		System.out.println(fib(n));
	}
}
```

**Complexity Analysis**

```
Time Complexity: 	O(n)
Space Complexity: 	O(n)
```





<br><br> 
### Approach 3: Space Optimized Dynamic Programming 


We can optimize the space used in the above method by storing only the previous two numbers because
that is all we need to the get the next Fibonacci Number in Series


```java 
class fibonacci {
	
	static int fib(int n) {
		
		int a=0, b=1, c;

		if (n==0) {
			return a; 
		}

		for (int i=2; i<=n; i++) {
			c=a+b;
			a=b;
			b=c;
		}
		return b;
	}

	public static void main(String args[]) {
		
		int n=0;
		System.out.println(fib(n));
	}
}
```


**Complexity Analysis** 

```
Time Complexity: 	O(n) 
Space Complexity: 	O(1) 
```



<br><br> 
### Further Exploration 

There is also a O(logn) runtime method of implementing fibonacci using matrices and also a O(1) runtime
method which uses the Fibonacci mathematical equation. 

```
Fn = ((sqrt(5)+1)/2)^n)sqrt(5)
```






<br><br><br> 
---
<a name="sorting"></a>
# Sorting 


## Introduction 

Sorting is ordering a list of objects. We can distinguish two types of sorting. IF the number of 
objects is small enough to fit into the main memory, the sorting is called *internal sorting*. If the 
number of objects is so large that some of them reside on external storage during the sort, it is 
called *external sorting*. In this exploration we will cover the following sorting algorithms. 












