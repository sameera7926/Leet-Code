# ⚡ Kadane's Algorithm — Pattern Recognition

**Kadane's Algorithm** is a very important pattern for solving problems related to the **maximum sum of a contiguous subarray**.

The core idea is:

> At every position, decide whether it is better to **continue the current subarray** or **start a new subarray from the current element**.

The main formula is:

```text
currentSum = max(nums[i], currentSum + nums[i])
```

And we keep track of:

```text
maxSum = max(maxSum, currentSum)
```

---

# 🧠 How to Identify Kadane's Algorithm?

Don't look only for the words:

> "Use Kadane's Algorithm."

The question can be written in many different ways.

Look for:

```text
Maximum subarray sum
Maximum sum of contiguous subarray
Largest sum of consecutive elements
Maximum sum segment
Best contiguous subarray
Maximum profit from consecutive values
Maximum gain
Largest sum of a continuous subarray
```

The strongest signal is:

```text
CONTIGUOUS + MAXIMUM SUM
```

Think:

```text
⚡ KADANE'S ALGORITHM
```

---

# 🎯 The Most Important Signal

Whenever you see:

```text
CONTIGUOUS
        +
MAXIMUM SUM
```

think:

```text
Kadane's Algorithm
```

For example:

```text
[-2, 1, -3, 4, -1, 2, 1, -5, 4]
```

Question:

> Find the contiguous subarray with the largest sum.

Answer:

```text
[4, -1, 2, 1]
```

Sum:

```text
4 + (-1) + 2 + 1 = 6
```

Therefore:

```text
maximum sum = 6
```

---

# 🧠 Why Kadane's Algorithm Works

Suppose we are currently calculating the sum of a subarray.

We reach:

```text
currentSum
```

and the next number is:

```text
nums[i]
```

We have two choices:

### Choice 1 — Continue the current subarray

```text
currentSum + nums[i]
```

### Choice 2 — Start a new subarray

```text
nums[i]
```

So we choose the better one:

```text
currentSum = max(
    nums[i],
    currentSum + nums[i]
)
```

That is the main idea behind Kadane's Algorithm.

---

# 🔥 The Golden Question

At every element ask:

```text
"Should I continue my previous subarray,
or should I start a new subarray here?"
```

Formula:

```text
currentSum = max(nums[i], currentSum + nums[i])
```

---

# 🧪 Example

Consider:

```text
[-2, 1, -3, 4, -1, 2, 1, -5, 4]
```

We want:

```text
Maximum contiguous subarray sum
```

Start:

```text
currentSum = -2
maxSum = -2
```

---

## Step 1

Current value:

```text
-2
```

```text
currentSum = -2
maxSum = -2
```

---

## Step 2

Current value:

```text
1
```

We compare:

```text
1
```

vs

```text
-2 + 1 = -1
```

Choose:

```text
1
```

Therefore:

```text
currentSum = 1
maxSum = 1
```

We started a new subarray:

```text
[1]
```

---

## Step 3

Current value:

```text
-3
```

Compare:

```text
-3
```

vs

```text
1 + (-3) = -2
```

Choose:

```text
-2
```

So:

```text
currentSum = -2
maxSum = 1
```

---

## Step 4

Current value:

```text
4
```

Compare:

```text
4
```

vs

```text
-2 + 4 = 2
```

Choose:

```text
4
```

So:

```text
currentSum = 4
maxSum = 4
```

Start again from:

```text
[4]
```

---

## Step 5

Current value:

```text
-1
```

Compare:

```text
-1
```

vs

```text
4 + (-1) = 3
```

Choose:

```text
3
```

Current subarray:

```text
[4, -1]
```

---

## Step 6

Current value:

```text
2
```

Compare:

```text
2
```

vs

```text
3 + 2 = 5
```

Choose:

```text
5
```

Current subarray:

```text
[4, -1, 2]
```

---

## Step 7

Current value:

```text
1
```

Compare:

```text
1
```

vs

```text
5 + 1 = 6
```

Choose:

```text
6
```

Current subarray:

```text
[4, -1, 2, 1]
```

Maximum:

```text
6
```

---

## Step 8

Current value:

```text
-5
```

Continue:

```text
6 + (-5) = 1
```

So:

```text
currentSum = 1
maxSum = 6
```

---

## Step 9

Current value:

```text
4
```

Compare:

```text
4
```

vs

```text
1 + 4 = 5
```

Choose:

```text
5
```

Maximum remains:

```text
6
```

---

# 🏆 Final Answer

```text
Array:
[-2, 1, -3, 4, -1, 2, 1, -5, 4]

Maximum subarray:
[4, -1, 2, 1]

Maximum sum:
6
```

---

# 💻 Basic Kadane's Algorithm

```java
public int maxSubArray(int[] nums) {

    int currentSum = nums[0];
    int maxSum = nums[0];

    for (int i = 1; i < nums.length; i++) {

        currentSum = Math.max(
            nums[i],
            currentSum + nums[i]
        );

        maxSum = Math.max(
            maxSum,
            currentSum
        );
    }

    return maxSum;
}
```

---

# 🧠 The Two Variables You Must Remember

Kadane mainly needs two variables:

```text
currentSum
maxSum
```

### `currentSum`

Means:

> Maximum sum of a subarray **ending at the current index**.

### `maxSum`

Means:

> Maximum sum found **anywhere so far**.

So:

```text
currentSum
    ↓
Best subarray ending HERE

maxSum
    ↓
Best subarray found SO FAR
```

This distinction is extremely important.

---

# 🔥 The Kadane Formula

Memorize this:

```text
currentSum = max(
    nums[i],
    currentSum + nums[i]
)
```

Then:

```text
maxSum = max(maxSum, currentSum)
```

Or in Java:

```java
currentSum = Math.max(nums[i], currentSum + nums[i]);

maxSum = Math.max(maxSum, currentSum);
```

---

# ⚠️ Important: Why Not Just Reset When Sum Becomes Negative?

You may see another version:

```java
currentSum += nums[i];

if (currentSum < 0) {
    currentSum = 0;
}
```

This works for the classic maximum-subarray problem **when an empty subarray is allowed / when the problem's constraints permit this interpretation**.

But for the standard problem where the subarray must be **non-empty**, the safer general implementation is:

```java
currentSum = Math.max(nums[i], currentSum + nums[i]);
```

This also correctly handles:

```text
[-5, -2, -8]
```

Answer:

```text
-2
```

rather than incorrectly returning `0`.

---

# 🧪 All Negative Numbers

Consider:

```text
[-5, -2, -8, -1]
```

The maximum contiguous subarray is:

```text
[-1]
```

Sum:

```text
-1
```

Using:

```java
int currentSum = nums[0];
int maxSum = nums[0];
```

we correctly get:

```text
maxSum = -1
```

This is why initializing with `0` can be dangerous for the non-empty-subarray version.

---

# 🎯 How to Recognize Kadane From Different Question Forms

The same pattern can be hidden behind different wording.

### Question 1

> Find the maximum sum subarray.

Think:

```text
Kadane
```

---

### Question 2

> Find the largest sum of consecutive elements.

Think:

```text
Kadane
```

Because:

```text
consecutive = contiguous
```

---

### Question 3

> Find the best contiguous segment.

Think:

```text
Kadane
```

---

### Question 4

> Find the maximum possible sum from a continuous portion of the array.

Think:

```text
Kadane
```

---

### Question 5

> Find the subarray with maximum sum.

Think:

```text
Kadane
```

---

# 🧠 Key Word Recognition

When you see:

```text
MAXIMUM
+
SUM
+
CONTIGUOUS / CONSECUTIVE / SUBARRAY
```

think:

```text
⚡ KADANE
```

---

# 🔄 Kadane vs Sliding Window

These two patterns can look similar because both process a contiguous portion of an array.

But they solve different types of problems.

## Sliding Window

Usually:

```text
Subarray / Substring
+
Fixed size / condition
+
Longest / minimum / frequency
```

Example:

```text
Maximum sum of K consecutive elements
```

Here the window size is fixed:

```text
K
```

---

## Kadane

Usually:

```text
Maximum sum
+
Any contiguous subarray
```

The size is **not fixed**.

Example:

```text
[-2, 1, -3, 4, -1, 2, 1]
```

Kadane decides:

```text
Continue?
OR
Start new?
```

---

# 🧠 Easy Difference

Remember:

```text
SLIDING WINDOW
→ Window is controlled by size/condition
```

```text
KADANE
→ Window is controlled by SUM
→ Continue or restart
```

---

# 🔥 Kadane as a Decision

At every index:

```text
                 nums[i]
                    │
                    ↓
          ┌─────────┴─────────┐
          ↓                   ↓
      START NEW            CONTINUE
       nums[i]        currentSum + nums[i]
          │                   │
          └─────────┬─────────┘
                    ↓
                  MAX
                    ↓
              currentSum
```

Then:

```text
currentSum
    ↓
Update maxSum
```

---

# 🎯 Finding the Actual Subarray

Sometimes the question asks:

> Return the maximum sum.

But sometimes it asks:

> Return the subarray that produces the maximum sum.

Then we need to store indexes.

Use:

```java
int currentSum = nums[0];
int maxSum = nums[0];

int start = 0;
int bestStart = 0;
int bestEnd = 0;

for (int i = 1; i < nums.length; i++) {

    if (nums[i] > currentSum + nums[i]) {
        currentSum = nums[i];
        start = i;
    } else {
        currentSum += nums[i];
    }

    if (currentSum > maxSum) {
        maxSum = currentSum;
        bestStart = start;
        bestEnd = i;
    }
}
```

Now:

```text
bestStart
    ↓
[ maximum subarray ]
                    ↑
                 bestEnd
```

---

# 🧪 Example — Find Actual Subarray

```text
[-2, 1, -3, 4, -1, 2, 1, -5, 4]
```

Kadane identifies:

```text
bestStart = 3
bestEnd = 6
```

Therefore:

```text
[4, -1, 2, 1]
```

Sum:

```text
6
```

---

# 🚨 Important Variations

Kadane's idea can be extended beyond the basic maximum subarray problem.

Common variations include:

```text
Maximum subarray sum
Minimum subarray sum
Maximum circular subarray sum
Maximum product subarray
Maximum profit-like contiguous gain problems
```

But don't blindly use normal Kadane for all of them.

Each variation has a slightly different state.

---

# 1️⃣ Maximum Subarray Sum

Basic:

```text
currentSum = max(
    nums[i],
    currentSum + nums[i]
)
```

Use when:

```text
Maximum + contiguous + sum
```

---

# 2️⃣ Minimum Subarray Sum

The idea is reversed.

Instead of:

```text
maximum
```

we calculate:

```text
minimum
```

Formula:

```text
currentMin = min(
    nums[i],
    currentMin + nums[i]
)
```

Then:

```text
minSum = min(minSum, currentMin)
```

So:

```text
Maximum:
max()

Minimum:
min()
```

---

# 3️⃣ Maximum Circular Subarray

Sometimes the array is considered circular.

Example:

```text
[5, -3, 5]
```

The maximum circular subarray can wrap around:

```text
[5, 5]
```

Sum:

```text
10
```

The common idea is:

```text
maximum normal subarray
OR
totalSum - minimum subarray
```

Formula:

```text
max(
    normalMax,
    totalSum - minimumSubarray
)
```

### ⚠️ Special Case

If all numbers are negative:

```text
[-3, -2, -5]
```

then:

```text
totalSum - minimumSubarray
```

can incorrectly represent an empty subarray.

So handle the all-negative case separately.

---

# 4️⃣ Maximum Product Subarray

This looks similar but is **not the same simple Kadane**.

Why?

Because with multiplication:

```text
negative × negative = positive
```

So the smallest negative product can become the largest positive product.

Therefore we maintain:

```text
maxProduct
minProduct
```

At every element:

```text
newMax = max(
    nums[i],
    nums[i] * maxProduct,
    nums[i] * minProduct
)

newMin = min(
    nums[i],
    nums[i] * maxProduct,
    nums[i] * minProduct
)
```

The important recognition is:

```text
MAX PRODUCT
+
CONTIGUOUS SUBARRAY
```

Think:

```text
Kadane-like DP
+
Track BOTH max and min
```

---

# 🧠 Kadane vs Prefix Sum

Another common confusion.

### Prefix Sum

Usually useful for:

```text
Range Sum
Subarray Sum Equals K
Fast range queries
```

### Kadane

Usually:

```text
Maximum contiguous sum
```

Think:

```text
PREFIX SUM
→ "What is the sum from L to R?"

KADANE
→ "What contiguous subarray gives the maximum sum?"
```

---

# 🗺️ Pattern Recognition Cheat Sheet

| Question asks                         | Think            |
| ------------------------------------- | ---------------- |
| Maximum subarray sum                  | Kadane           |
| Maximum sum of contiguous subarray    | Kadane           |
| Largest sum of consecutive elements   | Kadane           |
| Best contiguous segment               | Kadane           |
| Maximum continuous sum                | Kadane           |
| Maximum subarray                      | Kadane           |
| Minimum subarray sum                  | Kadane variation |
| Maximum circular subarray             | Circular Kadane  |
| Maximum product subarray              | Kadane-like DP   |
| Maximum sum of exactly K elements     | Sliding Window   |
| Maximum sum of K consecutive elements | Sliding Window   |

---

# ⚠️ Very Important Distinction

Compare these two questions:

### Question A

> Find the maximum sum of **K consecutive elements**.

Think:

```text
🪟 SLIDING WINDOW
```

Because the size is fixed:

```text
K
```

---

### Question B

> Find the maximum sum of **any contiguous subarray**.

Think:

```text
⚡ KADANE
```

Because the size is not fixed.

Kadane decides whether to:

```text
CONTINUE
```

or:

```text
RESTART
```

---

# 🔥 Universal Kadane Template

For maximum subarray sum:

```java
int currentSum = nums[0];
int maxSum = nums[0];

for (int i = 1; i < nums.length; i++) {

    currentSum = Math.max(
        nums[i],
        currentSum + nums[i]
    );

    maxSum = Math.max(
        maxSum,
        currentSum
    );
}

return maxSum;
```

Memorize:

```text
CURRENT → MAX OF (CURRENT + NUM OR NEW NUM)
GLOBAL  → MAX OF (GLOBAL OR CURRENT)
```

---

# 🧠 Final Mental Framework

When you see:

```text
ARRAY
  │
  ↓
CONTIGUOUS / CONSECUTIVE?
  │
  ↓
SUM?
  │
  ↓
MAXIMUM?
  │
  ↓
⚡ KADANE
```

Then ask:

```text
"Should I continue the previous subarray
or start a new one?"
```

Formula:

```text
currentSum = max(
    nums[i],
    currentSum + nums[i]
)
```

Then:

```text
maxSum = max(
    maxSum,
    currentSum
)
```

---

# ⭐ One-Line Memory Trick

```text
KADANE =
CONTIGUOUS SUBARRAY
+
MAXIMUM SUM
+
CONTINUE OR RESTART
```

And remember:

```text
currentSum
    ↓
Best sum ending HERE

maxSum
    ↓
Best sum found SO FAR
```

---

# 🏆 Final Pattern Recognition

```text
                  ARRAY
                    │
                    ↓
              CONTIGUOUS?
                    │
                   YES
                    ↓
                  SUM?
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
      MAXIMUM SUM         FIXED SIZE K
          │                   │
          ↓                   ↓
       KADANE           SLIDING WINDOW
          │
          ↓
  Continue OR Restart
          │
     ┌────┴────┐
     ↓         ↓
 Continue    Restart
     ↓         ↓
current +   nums[i]
 nums[i]
     │         │
     └────┬────┘
          ↓
        MAX
          ↓
    currentSum
          ↓
      maxSum
```

# 🔑 Golden Rules

```text
CONTIGUOUS + MAX SUM
        ↓
     KADANE
```

```text
CONTIGUOUS + FIXED K
        ↓
SLIDING WINDOW
```

```text
CONTIGUOUS + MIN SUM
        ↓
KADANE VARIATION
```

```text
CONTIGUOUS + MAX PRODUCT
        ↓
KADANE-LIKE DP
+
MAX & MIN PRODUCT
```

```text
CIRCULAR + MAX SUM
        ↓
CIRCULAR KADANE
```

**The main thing to recognize is not the name "Kadane." When the question asks for the best sum over an arbitrary contiguous portion, ask:**

```text
"Should I extend what I already have,
or is starting fresh better?"
```

That decision is Kadane's Algorithm.
