# 20. Valid Parentheses

Given a string containing just the characters '\(', '\)', '{', '}', '\[' and '\]', determine if the input string is valid.An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

Example 1:

```text
Input: "()"
Output: true
```

Example 2:

```text
Input: "()[]{}"
Output: true
```

Example 3:

```text
Input: "(]"
Output: false
```

Example 4:

```text
Input: "([)]"
Output: false
```

Example 5:

```text
Input: "{[]}"
Output: true
```

使用 stack 儲存 目前的 "\[", 當碰到 "\]" 就pop stack 最後一筆,

檢查是否合法

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        ParenthesesPair = {'(': ')', '{':'}', '[': ']'}
        if len(s) == 1 or len(s) % 2 !=0 :
            return False        
        checkStack = []
        
        for each in s:
            if each in ParenthesesPair:
                checkStack.append(each)
            
            if checkStack and each in ParenthesesPair.values():
                top = checkStack.pop()
                if ParenthesesPair[top] != each:
                    return False
        
        if checkStack:
            return False
        else:
            return True
```



### Solution

\*\*Intuition\*\*

Imagine you are writing a small compiler for your college project and one of the tasks \(or say sub-tasks\) for the compiler would be to detect if the parenthesis are in place or not.The algorithm we will look at in this article can be then used to process all the parenthesis in the program your compiler is compiling and checking if all the parenthesis are in place. This makes checking if a given string of parenthesis is valid or not, an important programming problem.The expressions that we will deal with in this problem can consist of three different type of parenthesis:

* \(\),
* {} and
* \[\]

Before looking at how we can check if a given expression consisting of these parenthesis is valid or not, let us look at a simpler version of the problem that consists of just one type of parenthesis. So, the expressions we can encounter in this simplified version of the problem are e.g.

```text
(((((()))))) -- VALID

()()()()     -- VALID

(((((((()    -- INVALID

((()(())))   -- VALID
```

Let's look at a simple algorithm to deal with this problem.

1. We process the expression one bracket at a time starting from the left.
2. Suppose we encounter an opening bracket i.e. \(, it may or may not be an invalid expression because there can be a matching ending bracket somewhere in the remaining part of the expression. Here, we simply increment the counter keeping track of left parenthesis till now. left += 1
3. If we encounter a closing bracket, this has two meanings:
   1. One, there was no matching opening bracket for this closing bracket and in that case we have an invalid expression. This is the case when left == 0 i.e. when there are no unmatched left brackets available.
   2. We had some unmatched opening bracket available to match this closing bracket. This is the case when left &gt; 0 i.e. we have unmatched left brackets available.
4. If we encounter a closing bracket i.e. \) when left == 0, then we have an invalid expression on our hands. Else, we decrement left thus reducing the number of unmatched left parenthesis available.
5. Continue processing the string until all parenthesis have been processed.
6. If in the end we still have unmatched left parenthesis available, this implies an invalid expression.

The reason we discussed this particular algorithm here is because the approach for the original problem derives its inspiration from this very solution. Have a look at the following dry run of the algorithm we discussed to have a better understanding.



![](blob:https://wan-huiyuan.gitbook.io/5e8df9bb-2b15-4510-9fec-f8f209a896b5)

If we try and follow the same approach for our original problem, then it simply won't work. The reason a simple counter based approach works above is because all the parenthesis are of the same type. So when we encounter a closing bracket, we simply assume a corresponding opening matching bracket to be available i.e. if left &gt; 0.But, in our problem, if we encounter say \], we don't really know if there is a corresponding opening \[ available or not. You could say:

> Why not maintain a separate counter for the different types of parenthesis?

This doesn't work because the relative placement of the parenthesis also matters here. e.g.:

```text
[{]
```

If we simply keep counters here, then as soon as we encounter the closing square bracket, we would know there is an unmatched opening square bracket available as well. But, the closest unmatched opening bracket available is a curly bracket and not a square bracket and hence the counting approach breaks here. 

**Approach 1: Stacks**

An interesting property about a valid parenthesis expression is that a sub-expression of a valid expression should also be a valid expression. \(Not every sub-expression\) e.g.



![](blob:https://wan-huiyuan.gitbook.io/3307b0dc-9b88-43f7-be9b-b8bf6f900577)

Also, if you look at the above structure carefully, the color coded cells mark the opening and closing pairs of parenthesis. The entire expression is valid, but sub portions of it are also valid in themselves. This lends a sort of a recursive structure to the problem. For e.g. Consider the expression enclosed within the two green parenthesis in the diagram above. The opening bracket at index 1 and the corresponding closing bracket at index 6.

> What if whenever we encounter a matching pair of parenthesis in the expression, we simply remove it from the expression?

Let's have a look at this idea below where remove the smaller expressions one at a time from the overall expression and since this is a valid expression, we would be left with an empty string in the end.![](blob:https://wan-huiyuan.gitbook.io/d400da66-aae2-45d8-8bf7-7c4e06631ad6)  
1 / 5

> The stack data structure can come in handy here in representing this recursive structure of the problem. We can't really process this from the inside out because we don't have an idea about the overall structure. But, the stack can help us process this recursively i.e. from outside to inwards.

Let us have a look at the algorithm for this problem using stacks as the intermediate data structure.

Algorithm

1. Initialize a stack S.
2. Process each bracket of the expression one at a time.
3. If we encounter an opening bracket, we simply push it onto the stack. This means we will process it later, let us simply move onto the sub-expression ahead.
4. If we encounter a closing bracket, then we check the element on top of the stack. If the element at the top of the stack is an opening bracket of the same type, then we pop it off the stack and continue processing. Else, this implies an invalid expression.
5. In the end, if we are left with a stack still having elements, then this implies an invalid expression.

We'll have a look a dry run for the algorithm and then move onto the implementation.

![](blob:https://wan-huiyuan.gitbook.io/215424a8-7219-484d-8e89-170cde8f88b1)  
1 / 12Let us now have a look at the implementation for this algorithm.



