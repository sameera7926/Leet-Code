# 9. Palindrome Number

## 🟢 Difficulty

Easy

## 🔗 Problem

[LeetCode 9 - Palindrome Number](https://leetcode.com/problems/palindrome-number/)

---

## 📌 Problem

Given an integer `x`, return `true` if `x` is a **palindrome**, otherwise return `false`.

A palindrome reads the same from left to right and right to left.

### Examples

```text
121 → true
-121 → false
10 → false
```

---

# 🧩 Pattern

**Math / Digit Manipulation**

---

# 💡 Key Idea

Instead of converting the number into a String, we can **reverse the digits mathematically**.

For example:

```text
x = 121

Last digit = 1
Then = 2
Then = 1

Reverse = 121
```

Finally:

```text
original == reversed
```

If they are equal → palindrome.

---

# 👨‍💻 Java Code

```java
class Solution {
    public boolean isPalindrome(int x) {

        if (x < 0) {
            return false;
        }

        int original = x;
        int reverse = 0;

        while (x > 0) {

            int digit = x % 10;

            reverse = reverse * 10 + digit;

            x = x / 10;
        }

        return original == reverse;
    }
}
```

---

# 🔍 Code Explanation

### 1. Negative numbers

```java
if (x < 0) {
    return false;
}
```

A negative number cannot be a palindrome because of the `-` sign.

Example:

```text
-121

Reverse:
121-

Not equal
```

So directly return `false`.

---

### 2. Save the original number

```java
int original = x;
```

Why?

Because we modify `x` inside the loop.

We need the original value at the end:

```text
original == reverse
```

---

### 3. Create reverse

```java
int reverse = 0;
```

This variable will store the reversed number.

---

### 4. Get the last digit

```java
int digit = x % 10;
```

`% 10` gives the **last digit**.

Example:

```text
121 % 10 = 1
```

```text
12 % 10 = 2
```

```text
1 % 10 = 1
```

---

### 5. Build the reversed number

```java
reverse = reverse * 10 + digit;
```

Why `reverse * 10`?

Because we need to shift the existing digits one position left.

Example:

```text
reverse = 1

1 × 10 = 10
10 + 2 = 12
```

Then:

```text
12 × 10 = 120
120 + 1 = 121
```

---

### 6. Remove the last digit

```java
x = x / 10;
```

Integer division removes the last digit.

Example:

```text
121 / 10 = 12
12 / 10 = 1
1 / 10 = 0
```

When `x` becomes `0`, the loop stops.

---

# 🧪 Dry Run

### Input

```text
x = 121
```

Initially:

```text
original = 121
reverse = 0
```

### Iteration 1

```text
x = 121

digit = 121 % 10
      = 1

reverse = 0 × 10 + 1
        = 1

x = 121 / 10
  = 12
```

---

### Iteration 2

```text
x = 12

digit = 12 % 10
      = 2

reverse = 1 × 10 + 2
        = 12

x = 12 / 10
  = 1
```

---

### Iteration 3

```text
x = 1

digit = 1 % 10
      = 1

reverse = 12 × 10 + 1
        = 121

x = 1 / 10
  = 0
```

Loop stops.

Now:

```text
original = 121
reverse  = 121
```

Therefore:

```text
121 == 121
```

### Output

```text
true
```

---

# ❌ Example: x = 10

```text
original = 10
```

Reverse:

```text
10 → 1
1  → 0
```

So:

```text
reverse = 1
```

Comparison:

```text
10 != 1
```

Therefore:

```text
false
```

---

# ❌ Example: x = -121

First condition:

```java
if (x < 0)
```

Since:

```text
-121 < 0
```

Return:

```text
false
```

---

# ⚠️ Common Mistakes

### 1. Forgetting negative numbers

```java
if (x < 0) {
    return false;
}
```

### 2. Forgetting to save the original

If you don't do:

```java
int original = x;
```

you lose the original number because `x` keeps changing.

### 3. Confusing `%` and `/`

Remember:

```text
x % 10 → get last digit

x / 10 → remove last digit
```

### 4. Building reverse incorrectly

Correct:

```java
reverse = reverse * 10 + digit;
```

---

# ⏱️ Complexity

Let `n` be the number of digits.

### Time

```text
O(log₁₀ n)
```

We process every digit once.

### Space

```text
O(1)
```

Only a few variables are used.

---

# 🎯 Interview Trigger

If the interviewer says:

> "Check whether an integer reads the same forwards and backwards."

Think:

```text
Palindrome Number
        ↓
Digit Manipulation
        ↓
% 10 → get last digit
        ↓
/ 10 → remove last digit
        ↓
Build reverse
        ↓
original == reverse
```

---

# 🧠 Must Remember

```text
% 10 → last digit

/ 10 → remove last digit

reverse = reverse * 10 + digit
```

These three lines are the main idea.

---

# ⭐ 30-Second Revision

```text
1. Negative → false

2. Store original number.

3. reverse = 0

4. While x > 0:
      digit = x % 10
      reverse = reverse * 10 + digit
      x = x / 10

5. Compare:
      original == reverse
```

### Interview One-Liner

**Extract each digit from the end using `% 10`, build the reversed number using `reverse * 10 + digit`, and compare it with the original.**
