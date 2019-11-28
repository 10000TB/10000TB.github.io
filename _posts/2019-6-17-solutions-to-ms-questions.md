---
layout: "post"
title: "My solutions to a few Microsoft interview questions"
categories: code
tags: [leetcode, code, soluton]
---

Originally, there was an online post sharing microsoft interview questions. I was free at the time, and got interested and took some coding up some solutions.

The problems - https://leetcode.com/discuss/interview-question/310636/Microsoft-or-OA-2019-or-Reverse-String-and-Remove-Prime-Numbers-From-List

My solutions

**JAVA & Python - Short & concise solutions with explanations**

1. Reverse string recursively
During reversion, first char in string will become last one. Then if we have reversed version of the string (from 1 to last), then we know the result ! That is reverse(s[1:]) + s[0]. Base case is when it is empty string, in which case we simply return empty string.

*Python*
```python
def reverse(s):
    if not s: return s
    return reverse(s[1:])+s[0]
```

*JAVA*
```java
public static String reverse(String s) {
    if (s==null || s.isEmpty()) { return s; }
    return reverse(s.substring(1)) + s.charAt(0);
}
```

2. Print number ( Assume positive number )
- right to left
  Every time, we print right most digit, and truncate it from the number. Doing so until we hit zero.

*Python*
```python
def printdigits(n):
    if n == 0: print '0'
    while n > 0:
        print n % 10
        if n / 10 > 0: print "\n"
        n /= 10
```

*JAVA*
```java
public static void printDigits(int n) {
    if (n==0) { System.out.println(n); }
    while (n>0) {
        System.out.println(n%10);
        n /= 10;
    }
}
```

- left to right
  We can pursue either of the two ways: 1) stacking up digits from right to left in a list and finally print out digits from left to right according to the list. 2) Or we recursively probe each digit from right to left, and start printing from deepest call upwards. That gives left to right order of all digits. Space complexity of both ways are O(n).

*Python*
```python
def printdigits(n):
    def doit(curr, n):
        if 1 <= curr <= 9: print n if curr == n else curr
        elif n > 9:
            doit(curr/10, n)
            print curr % 10
    if n == 0: print 0
    else: doit(n, n)
```

*JAVA*
```java
private static void doit(int curr, int n) {
    if (1<= curr && curr <=9){
        if (curr==n) { System.out.println(n);}
        else { System.out.println(curr); }
    } else {
        doit(curr/10, n);
        System.out.println(curr%10);}
}
    
public static void printDigits(int n) {
    if (n==0) { System.out.println(n); }
    else { doit(n, n); }
}
```

3. Remove prime numbers
Do it recursively from end to beginning using the call stacks. Use a separate function to check each node's value is prime or not.

*Python*
```python
def isprime(n):
    return all(n%i !=0 for i in xrange(2, math.sqrt(n)+1) ) if n > 0 else False

def remove(head):
    if not head: return None
    head.next = remove(head.next)
    return head.next if isprime(head.val) else head
```

*JAVA*
```java
private static boolean isPrime(int n) {
    if (n==1 || n==2) { return true;}
    else {
        for(int i=2; i < Math.sqrt(n); i++) { if (n%i ==0) {return false;} }
        return true;
    }
}
    
public static void remove(ListNode head) {
    if (head==null) { return None }
    head.next = remove(head.next)
    if isPrime(head.val) { return head.next } else { return head}
}
```

- Regarding using **Sieve of Eratosthenes** algo to decide if a number is prime of or not. If interviewer clearly states that all values in the linked list are smaller than a certain upper bound say N, we can use SoE algorithm to find out all prime numbers that are smaller than N, then it does gives a lot of time boost overall (Note there is extra O(N) space overhead, and O(NloglogN) time overhead for SoE preprocessing). However, if upper bound of nodes' values are not clear, then we can still use SoE, read it out how here on wikipedia: https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes. But for this interview, under time constraints, I favor simplicity.



(Also here - https://leetcode.com/discuss/interview-question/310636/Microsoft-or-OA-2019-or-Reverse-String-and-Remove-Prime-Numbers-From-List/289088)
