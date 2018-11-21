# 7. Reverse Integer

Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

```text
Input: 123
Output: 321
```

**Example 2:**

```text
Input: -123
Output: -321
```

**Example 3:**

```text
Input: 120
Output: 21
```

**Note:**  
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: \[−231, 231 − 1\]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

```python
class Solution(object):
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        if (x >=0 and x > pow(2,31)-1):
            return 0
        if (x<0 and x < pow(2,31)*-1):
            return 0
        x = str(x)
        if(x[0] == "-"):
            x=x[1::]
            if (int(x[::-1])*-1<0 and int(x[::-1])*-1 < pow(2,31)*-1):
                return 0
            return int(x[::-1])*-1
        else:
            if (int(x[::-1]) >=0 and int(x[::-1]) > pow(2,31)-1):
                print int(x[::-1])
                return 0
            return int(x[::-1])
```

## Solution

**Approach 1: Pop and Push Digits & Check before Overflow**

IntuitionWe can build up the reverse integer one digit at a time. While doing so, we can check beforehand whether or not appending another digit would cause overflow.AlgorithmReversing an integer can be done similarly to reversing a string.We want to repeatedly "pop" the last digit off of xx and "push" it to the back of the revrev. In the end, revrev will be the reverse of the xx.To "pop" and "push" digits without the help of some auxiliary stack/array, we can use math.  
However, this approach is dangerous, because the statement temp=rev⋅10+poptemp=rev⋅10+pop can cause overflow.Luckily, it is easy to check beforehand whether or this statement would cause an overflow.To explain, lets assume that revrev is positive.

1. If temp=rev⋅10+pop causes overflow, then it must be tha rev≥10INTMAX
2. If rev&gt;10INTMAX, then temp=rev⋅10+pop is guaranteed to overflow.
3. If rev==10INTMAX, then temp=rev⋅10+pop will overflow if and only if pop&gt;7

Similar logic can be applied when revrev is negative.

```java
class Solution {
    public int reverse(int x) {
        int rev = 0;
        while (x != 0) {
            int pop = x % 10;
            x /= 10;
            if (rev > Integer.MAX_VALUE/10 || (rev == Integer.MAX_VALUE / 10 && pop > 7)) return 0;
            if (rev < Integer.MIN_VALUE/10 || (rev == Integer.MIN_VALUE / 10 && pop < -8)) return 0;
            rev = rev * 10 + pop;
        }
        return rev;
    }
}
```

Complexity Analysis

* Time Complexity: O\(log\(x\)\). There are roughly log10\(x\) digits in x.
* Space Complexity: O\(1\).

