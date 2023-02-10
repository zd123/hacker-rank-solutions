## Before starting any hacker-rank techincal interview challenge
* make sure you understand the output.
* sometimes HR asks questions in a funky way.  not the most intitutive.
* test cases get harder and harder.
* some test cases are hidden.
* if multiple questions, find the questions you think you can answer and do them first. 

HackerRank Problem Set 1A
Buying Show Tickets
https://hr.gs/ctp-s22-1a 
Password: ctp2022prep



HackerRank Problem Set 1B (Homework)
Looping Lists and Huffman Decoder
https://hr.gs/ctp-s22-1b
Password:  ctp2022prep
Looping Lists for other languages: https://leetcode.com/problems/linked-list-cycle/ 


# Buying show tickets

```python
def waitingTime(tickets, p):
    # Write your code here
    
    maxTickets = tickets[p]
    total = 0
    i = 0
    while i < len(tickets):
        if tickets[i] < maxTickets:
            total += tickets[i]
        else:
            total += maxTickets
            
        if i==p:
            maxTickets -= 1
        i += 1

    return total
```

```python
def waitingTime(tickets, p):
    # Write your code here
    max_tickets = tickets[p]
    total = 0
    
    i = 0
    while i < len(tickets):
        if i <= p:
            total += min(max_tickets, tickets[i])
        else:
            total += min(max_tickets-1, tickets[i])
        i+=1
    return total
```

```python
def waitingTime(tickets, p):
    total = 0 # aggregate / accumulator
    i = 0

    while tickets[p] > 0:
        if tickets[i] > 0:
            tickets[i] -= 1
            total += 1
        
        i += 1
        
        if i == len(tickets):
            i=0
 
    return total

```


```python
def waitingTime(tickets, p):
    maxTickets = tickets[p]
    total = 0 # aggregate / accumulator
            
    i = 0
    while i < len(tickets):
        if i <= p:
            if tickets[i] < max_tickets:
                total += tickets[i]
            else:
                total += max_tickets
        else:
            total += min(max_tickets-1, tickets[i])
    return total
    

```


# Looping List

We say that a linked list of n nodes has a cycle if the nth node's next field contains a reference to some other prior element in the list (as opposed to being null).

Complete the function in the editor below. It has one parameter: a LinkedListNode, head, denoting the head of a linked list of length n. The function must return a string denoting whether or not the list contains a cycle. If there is a cycle, return YES; otherwise, return NO.


```c++
string check(LinkedListNode* head) {
    set<LinkedListNode*> seen;
    LinkedListNode* curr = head;
    
    while(curr != NULL) {
        if(seen.count(curr)) {
            return "YES";
        }
        
        seen.insert(curr);
        curr = curr->next;
    }
    
    return "NO";
}
```

```python
def check(head):
    seen = set()
    curr = head

    while(curr != None):
        if curr in seen:
            return "YES"

        seen.add(curr)
        curr = curr.next

    return "NO"
```

```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if head is None:
            return False
        slow = head
        fast = head.next
        while slow != fast:
            if fast is None or fast.next is None:
                return False
            slow = slow.next
            fast = fast.next.next
        return True
```

```c++
// slow and fast pointer
string check(LinkedListNode* head) {
    LinkedListNode* slow = head;
    LinkedListNode* fast = head->next;
    
    long count = 0;
    
    while(fast != NULL) {
        if(slow == fast) {
            return "YES";
        }
        if(count % 2) {slow = slow->next;}    
        fast = fast->next;
        count++;
    }
    
    return "NO";
}
```

# Huffman Decoder

In this problem you're given a mapping from characters to huffman codes, and an encoded message. You have to use the map to decode the message.

This solution has a timeout error on some test cases with large input because it doesn't build the decoding tree fast enough (or it uses hashmaps in the first version which aren't fast enough). The optimal solution will use a heap tree and build the tree in O(n) time instead of O(n*lg n) time.

```python
def decode(codes, encoded):
    decoded = ''
    cmap = {}
    for s in codes:
        [char, code] = s.split("\t")
        cmap[code] = char
    
    curr = ''
    for c in encoded:
        curr += c
        if curr in cmap:
            char = cmap[curr]
            if char == '[newline]':
                decoded += '\n'
            else:
                decoded += cmap[curr]
            curr = ''

    return decoded
```



```python
# Complete the function below.
from operator import itemgetter
# left is 0, right is 1

class TNode:
    def __init__(self, value = None):
        self.val = value
        self.left = None
        self.right = None
        
def insertTNode(head, code, char):
    curr = head
    
    for c in code:
        if c == '0':
            if curr.left == None:
                curr.left = TNode()
            curr = curr.left
        else:
            if curr.right == None:
                curr.right = TNode()
            curr = curr.right
    
    curr.val = char



def findOrPartialTraverse(head, codeNum):
    if codeNum == '0':
        newHead = head.left
    else:
        newHead = head.right
        
    if newHead.val == None:
        return newHead
    return newHead.val

def decode(codes, encoded):
    decoded = ''
    
    head = TNode()
    
    for s in codes:
        [char, code] = s.split()
        insertTNode(head, code, char)
    
    currHead = head
    for c in encoded:
        res = findOrPartialTraverse(currHead, c)
        
        if isinstance(res, str):
            if res == '[newline]':
                decoded += '\n'
            else:
                decoded += res
            currHead = head
        else:
            currHead = res

    return decoded
```


```python
def insert(head, code, char):
    if len(code) == 0:
        head.val = char
    else:
        side = code[0]
        
        if side == '0':
            if head.left == None:
                head.left = TNode()
            insert(head.left, code[1:], char)
        else:
            if head.right == None:
                head.right = TNode()
            insert(head.right, code[1:], char)
```

```c++
// Fully working
string decode(vector<string> codes, string encoded) {
    string decoded;
    unordered_map<string,string> map = {};
    
    for(string line: codes) {
        string ch, hcode;
        istringstream iss(line);
        iss >> ch >> hcode;
        if(ch == "[newline]") {
            ch = '\n';
        }
        if(line[0] == ' '){
            ch = line[0];
            hcode = line.substr(2);
        }
        map.insert({hcode, ch});
    }
 
    string curr;
    for(auto cc: encoded) {
        curr += cc;
        
        auto search = map.find(curr);
        if (search != map.end()) {
            decoded += search->second;
            curr = "";
        } 
    }
    
    return decoded;
}
```
