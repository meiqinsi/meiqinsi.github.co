# Reverse Integer
## Description
Given a 32-bit signed integer, reverse digits of an integer.

#### Example 1:

Input: 123
Output: 321

#### Example 2:

Input: -123
Output: -321

#### Example 3:

Input: 120
Output: 21

## Note:
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

## Solution
Approach 1: Pop and Push Digits & Check before Overflow

### Intuition

We can build up the reverse integer one digit at a time. While doing so, we can check beforehand whether or not appending another digit would cause overflow.

### Algorithm

Reversing an integer can be done similarly to reversing a string.

We want to repeatedly "pop" the last digit off of xxx and "push" it to the back of the rev. In the end, rev will be the reverse of the xxx.

To "pop" and "push" digits without the help of some auxiliary stack/array, we can use math.

```
//pop operation:
pop = x % 10;
x /= 10;

eg.  to reverse 231,  
     pop = 231 % 10 = 1;
     x = 231/10 = 23;

//push operation:
temp = rev * 10 + pop;
rev = temp;
eg. temp = 23 * 10 + 1 = 231
    rev = 231;
```

However, this approach is dangerous, because the statement temp=rev⋅10+pop\text{temp} = \text{rev} \cdot 10 + \text{pop}temp=rev⋅10+pop can cause overflow.

Luckily, it is easy to check beforehand whether or this statement would cause an overflow.

To explain, lets assume that rev\text{rev}rev is positive.

    If temp=rev⋅10+pop causes overflow, then it must be that rev≥INTMAX / 10
    If rev>INTMAX/10, then temp=rev⋅10+pop is guaranteed to overflow.
    If rev==INTMAX/10 ​, then temp=rev⋅10+pop  will overflow if and only if pop>7

Similar logic can be applied when rev is negative.


### Java
```
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

eg.  x = 231;
     ret = 0l
     then while( 231 !=0){
       int pop = 231 %10 = 1;
       x = 231/10 = 23;

       ret = 0  * 10 + 1 = 1;  (the first digit)
     }

     then while( 23 !=0){
       int pop = 23 % 10 = 3;
       x = 23 /10 = 2;
       ret = 1 *10 + 3 = 13;
     }

     then while (2 !=0){
       int pop = 2%10 = 2 ???
       x = 2 /10 =0;

       ret = 13 * 10 + 2 = 132;
     }
```

#### So the idea is for 10 （decimal）
the last digit is x % 10

to construct the decimal   y = higher digit  * 10（for decimal） + lower digit


### Complexity Analysis

    Time Complexity: O(log(x)). There are roughly log10(x) digits in xxx.
    Space Complexity: O(1)



### MY JAVA
```
class Solution {
    public int reverse(int x) {
        int input =x;
        boolean positive = true;
        if (x <0){
            positive = false;
            input = 0-x;
        }
        String inputStr =  String.valueOf(input+"");
        int len = inputStr.length();      
        int[] stack = new int[len];
        int j =0;
        for(int i=len-1;i>=0;i--){
            Double inner = Double.parseDouble(Double.toString(Math.pow(10.0,i*1.0)));
            int miner = inner.intValue();

            stack[j] = input/miner;
            input = input - (input/miner)*miner;
            j++;           
        }

        int output = 0;
          for(int i=0;i<len;i++){
                Double inner = Double.parseDouble(Double.toString(Math.pow(10.0,i*1.0)));
               int miner = inner.intValue();
              output = output + stack[i]* miner;
          }

        return positive ? output: 0- output;
    }
}
```
