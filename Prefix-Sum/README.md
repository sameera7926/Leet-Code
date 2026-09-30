# ➕ Prefix Sum — Pattern Recognition

The **Prefix Sum** technique is used when a problem repeatedly asks for the **sum of a range** or asks us to find a **subarray with a particular sum**.

The main idea is:

> Precompute cumulative sums so that we can calculate a range sum quickly instead of adding the elements again and again.

---

# 🧠 How to Identify Prefix Sum?

Don't look only for the words **"prefix sum"**.

The question can be written in many different ways.

Look for:

```text
Range Sum
Subarray Sum
Sum between index L and R
Sum from L to R
Continuous subarray
Find subarray with sum K
Number of subarrays with sum K
Running sum
Cumulative sum
Range queries
```

Especially remember:

```text
MULTIPLE RANGE SUM QUERIES
        ↓
Think PREFIX SUM
```

and:

```text
SUBARRAY + SUM
        ↓
Think PREFIX SUM
```

---

# 🎯 The Core Idea

Suppose:

```text
nums = [2, 4, 1, 5, 3]
```

Instead of calculating the sum again and again, create:

```text
prefix = [2, 6, 7, 12, 15]
```

Meaning:

```text
prefix[0] = 2

prefix[1] = 2 + 4 = 6

prefix[2] = 2 + 4 + 1 = 7

prefix[3] = 2 + 4 + 1 + 5 = 12

prefix[4] = 2 + 4 + 1 + 5 + 3 = 15
```

So:

```text
nums:
[2, 4, 1, 5, 3]

prefix:
[2, 6, 7, 12, 15]
```

---

# 🔥 The Most Important Formula

For a range:

```text
L → R
```

the sum is:

```text
sum(L...R) = prefix[R] - prefix[L - 1]
```

For example:

```text
nums = [2, 4, 1, 5, 3]
         0  1  2  3  4
```

Find:

```text
sum from index 1 to 3
```

That means:

```text
4 + 1 + 5 = 10
```

Using prefix:

```text
prefix[3] = 12
prefix[0] = 2
```

Therefore:

```text
12 - 2 = 10
```

---

# ⚠️ The Easy Way to Avoid `L - 1` Confusion

A better implementation is to create a prefix array of size:

```text
n + 1
```

and start with:

```text
prefix[0] = 0
```

Example:

```text
nums:
[2, 4, 1, 5, 3]

prefix:
[0, 2, 6, 7, 12, 15]
```

Now the formula becomes:

```text
sum(L...R) = prefix[R + 1] - prefix[L]
```

This is much easier to remember.

---

# 💻 Prefix Sum Construction

```java
int n = nums.length;

int[] prefix = new int[n + 1];

for (int i = 0; i < n; i++) {
    prefix[i + 1] = prefix[i] + nums[i];
}
```

For:

```text
[2, 4, 1, 5, 3]
```

we get:

```text
prefix = [0, 2, 6, 7, 12, 15]
```

---

# 🎯 Range Sum Using Prefix Sum

Suppose:

```text
L = 1
R = 3
```

Then:

```java
int sum = prefix[R + 1] - prefix[L];
```

Therefore:

```text
prefix[4] - prefix[1]

= 12 - 2

= 10
```

Answer:

```text
4 + 1 + 5 = 10
```

---

# 🧠 Why Prefix Sum Is Useful?

Without Prefix Sum:

```text
Range Query
    ↓
Loop from L to R
    ↓
O(n)
```

If there are many queries:

```text
Query 1 → O(n)
Query 2 → O(n)
Query 3 → O(n)
...
```

This can become expensive.

With Prefix Sum:

```text
Build prefix → O(n)

Each range query → O(1)
```

So for many queries:

```text
O(n + q)
```

where:

```text
n = number of elements
q = number of queries
```

---

# 🧪 Example — Multiple Range Queries

Array:

```text
[2, 4, 1, 5, 3]
```

Prefix:

```text
[0, 2, 6, 7, 12, 15]
```

Queries:

```text
[1, 3]
[0, 2]
[2, 4]
```

### Query 1

```text
1 → 3
```

```text
prefix[4] - prefix[1]

12 - 2 = 10
```

---

### Query 2

```text
0 → 2
```

```text
prefix[3] - prefix[0]

7 - 0 = 7
```

---

### Query 3

```text
2 → 4
```

```text
prefix[5] - prefix[2]

15 - 6 = 9
```

---

# 🔑 Prefix Sum Memory Trick

Think:

```text
PREFIX[R + 1]
        -
PREFIX[L]
```

So:

```text
L → R
```

becomes:

```text
prefix[R + 1] - prefix[L]
```

---

# 2️⃣ Prefix Sum for "Subarray Sum = K"

This is one of the **most important Prefix Sum patterns**.

Question:

> Find the number of subarrays whose sum equals `K`.

Example:

```text
nums = [1, 2, 3]
K = 3
```

Subarrays with sum `3`:

```text
[3]
[1, 2]
```

Answer:

```text
2
```

---

# 🧠 The Important Idea

Suppose:

```text
prefix[j] - prefix[i] = K
```

Then:

```text
prefix[j] - K = prefix[i]
```

Therefore:

> If the current prefix sum is `currentSum`, we need to know whether we have already seen:

```text
currentSum - K
```

This is why we combine:

```text
PREFIX SUM
+
HASHMAP
```

---

# 🧪 Example — Subarray Sum Equals K

```text
nums = [1, 2, 3]
K = 3
```

Start:

```text
map = {0 : 1}
sum = 0
count = 0
```

Why:

```text
map[0] = 1
```

Because a prefix sum of `0` exists before we process any element.

---

## Process 1

```text
sum = 1
```

We need:

```text
sum - K
= 1 - 3
= -2
```

`-2` is not in the map.

Store:

```text
map[1] = 1
```

---

## Process 2

```text
sum = 3
```

Need:

```text
3 - 3 = 0
```

`0` exists in the map.

Therefore we found a subarray:

```text
[1, 2]
```

Increase count:

```text
count = 1
```

Store:

```text
map[3] = 1
```

---

## Process 3

```text
sum = 6
```

Need:

```text
6 - 3 = 3
```

`3` exists.

Therefore:

```text
[3]
```

is another valid subarray.

```text
count = 2
```

Answer:

```text
2
```

---

# 💻 Subarray Sum Equals K

```java
Map<Integer, Integer> map = new HashMap<>();

map.put(0, 1);

int sum = 0;
int count = 0;

for (int num : nums) {

    sum += num;

    if (map.containsKey(sum - k)) {
        count += map.get(sum - k);
    }

    map.put(
        sum,
        map.getOrDefault(sum, 0) + 1
    );
}

return count;
```

---

# 🔥 Most Important Formula for Subarray Sum K

Remember:

```text
currentPrefixSum - previousPrefixSum = K
```

Therefore:

```text
previousPrefixSum
=
currentPrefixSum - K
```

So:

```text
Current Sum
     ↓
sum - K
     ↓
Search in HashMap
```

This is the core of:

```text
Subarray Sum Equals K
```

---

# 🧠 Why Do We Store Frequency?

Suppose:

```text
prefix sum = 3
```

has appeared multiple times.

If:

```text
currentSum - K = 3
```

then **every occurrence** of prefix sum `3` gives us a different valid subarray.

Therefore we store:

```text
prefixSum → frequency
```

not simply:

```text
prefixSum → true
```

---

# 3️⃣ Prefix Sum + HashMap Pattern

This combination is extremely important.

When you see:

```text
Count subarrays with sum K
Find number of subarrays satisfying sum
Subarray sum equals K
Count continuous subarrays
```

think:

```text
PREFIX SUM + HASHMAP
```

The general pattern:

```text
currentSum
    ↓
currentSum - K
    ↓
Check HashMap
    ↓
Count matches
    ↓
Store currentSum
```

---

# 4️⃣ Prefix Sum for Running Sum

Sometimes the question simply asks for cumulative/running sums.

Example:

```text
nums = [1, 2, 3, 4]
```

Running sum:

```text
[1, 3, 6, 10]
```

Because:

```text
1

1 + 2 = 3

1 + 2 + 3 = 6

1 + 2 + 3 + 4 = 10
```

Code:

```java
int[] prefix = new int[nums.length];

prefix[0] = nums[0];

for (int i = 1; i < nums.length; i++) {
    prefix[i] = prefix[i - 1] + nums[i];
}
```

---

# 5️⃣ Prefix Sum + Zero

A very common question:

> Find whether a subarray has sum `0`.

Example:

```text
[1, 2, -3, 4]
```

Prefix sums:

```text
1
3
0
4
```

When a prefix sum becomes `0`, it means:

```text
some subarray from the beginning
has sum 0
```

More generally:

> If the **same prefix sum appears twice**, the elements between those two positions have sum `0`.

Example:

```text
nums = [1, 2, -2, 3]
```

Prefix:

```text
1
3
1
4
```

`1` appears twice.

Therefore:

```text
2 + (-2) = 0
```

So a zero-sum subarray exists.

---

# 🔥 Important Prefix Sum Property

If:

```text
prefix[i] == prefix[j]
```

then:

```text
sum(i+1 ... j) = 0
```

This is an extremely important concept.

---

# 6️⃣ Prefix Sum + Modulo

Another important pattern is:

```text
PREFIX SUM + MODULO
```

Look for questions like:

```text
Subarray sum divisible by K
Count subarrays divisible by K
Sum is a multiple of K
```

The key idea:

```text
prefixSum % K
```

If the same remainder appears twice:

```text
prefix[i] % K == prefix[j] % K
```

then:

```text
(prefix[j] - prefix[i]) % K = 0
```

Therefore the subarray between them is divisible by `K`.

---

# 🧪 Example

```text
nums = [4, 5, 0, -2, -3, 1]
K = 5
```

Calculate prefix sums and their remainders.

If the same remainder appears again, we can identify a subarray whose sum is divisible by `5`.

The pattern is:

```text
prefix sum
     ↓
prefix sum % K
     ↓
HashMap
     ↓
Count same remainder
```

---

# 7️⃣ Prefix Sum + Frequency Map

A very useful general template is:

```text
PREFIX SUM
    +
HASHMAP
```

The HashMap can store:

```text
prefix sum → frequency
```

or:

```text
remainder → frequency
```

depending on the question.

---

# 🧠 How to Recognize Prefix Sum Questions

The question may be written differently.

---

## 🔹 Range Sum

Look for:

```text
Find sum from L to R
Range sum query
Sum between indexes
Multiple sum queries
Sum of elements in a range
```

Think:

```text
PREFIX SUM
```

---

## 🔹 Subarray Sum

Look for:

```text
Find subarray with sum K
Count subarrays with sum K
Continuous subarray with target sum
Number of subarrays whose sum is K
```

Think:

```text
PREFIX SUM + HASHMAP
```

---

## 🔹 Zero Sum

Look for:

```text
Find zero-sum subarray
Does a zero-sum subarray exist?
Longest zero-sum subarray
```

Think:

```text
PREFIX SUM
+
HASHMAP / SET
```

---

## 🔹 Divisible by K

Look for:

```text
Sum divisible by K
Sum is a multiple of K
Count subarrays divisible by K
```

Think:

```text
PREFIX SUM + MODULO + HASHMAP
```

---

# 🧠 Prefix Sum vs Sliding Window

This is very important because both can appear in **subarray sum** questions.

### Prefix Sum

Usually useful when:

```text
Negative numbers can exist
Multiple range queries
Count subarrays with exact sum
Need prefix relationships
```

Think:

```text
PREFIX SUM
+
HASHMAP
```

---

### Sliding Window

Often useful when:

```text
Window is contiguous
Numbers are non-negative
Need longest / minimum window
Condition can be maintained by expanding/shrinking
```

Think:

```text
SLIDING WINDOW
```

---

# ⚠️ Important Example

Suppose:

```text
nums = [1, -1, 2, 3]
```

and:

> Find a subarray with sum `3`.

Because negative numbers exist, a simple:

```text
expand → shrink
```

Sliding Window approach may not work correctly.

Prefix Sum + HashMap is much more reliable:

```text
currentSum - K
```

---

# 🆚 Prefix Sum vs Sliding Window

| Question                               | Pattern                           |
| -------------------------------------- | --------------------------------- |
| Sum from L to R                        | Prefix Sum                        |
| Many range sum queries                 | Prefix Sum                        |
| Count subarrays with sum K             | Prefix Sum + HashMap              |
| Zero-sum subarray                      | Prefix Sum                        |
| Subarray sum divisible by K            | Prefix Sum + Modulo               |
| Longest subarray with condition        | Often Sliding Window / Prefix Sum |
| Minimum subarray with positive numbers | Sliding Window                    |
| Fixed-size subarray sum                | Sliding Window                    |
| Maximum sum of K consecutive           | Sliding Window                    |

---

# 🔥 Prefix Sum Pattern Recognition

When you see:

```text
SUM
 +
RANGE / SUBARRAY
```

think:

```text
PREFIX SUM
```

Then ask:

```text
Is it a range query?
       ↓
PREFIX ARRAY

Is it count subarrays with sum K?
       ↓
PREFIX SUM + HASHMAP

Is it divisible by K?
       ↓
PREFIX SUM + MODULO + HASHMAP

Is it zero sum?
       ↓
PREFIX SUM + HASHMAP / SET
```

---

# 🗺️ Complete Pattern Framework

```text
                  SUM QUESTION
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
          RANGE                SUBARRAY
             │                   │
             ↓                   ↓
       PREFIX SUM         What is asked?
                                 │
                ┌────────────────┼────────────────┐
                ↓                ↓                ↓
             Sum = K        Sum = 0          Divisible K
                │                │                │
                ↓                ↓                ↓
          Prefix + Map     Prefix + Map    Prefix + Modulo
```

---

# 🔑 Important Formulas

## Range Sum

With:

```text
prefix[0] = 0
```

use:

```text
sum(L...R)
=
prefix[R + 1] - prefix[L]
```

---

## Subarray Sum K

```text
currentSum - previousSum = K
```

Therefore:

```text
previousSum = currentSum - K
```

So search:

```text
map[currentSum - K]
```

---

## Zero Sum

If:

```text
prefix[i] == prefix[j]
```

then:

```text
sum(i+1...j) = 0
```

---

## Divisible by K

If:

```text
prefix[i] % K == prefix[j] % K
```

then:

```text
sum(i+1...j) % K == 0
```

---

# 💻 Universal Prefix Sum Template

## Basic Prefix Array

```java
int[] prefix = new int[nums.length + 1];

for (int i = 0; i < nums.length; i++) {
    prefix[i + 1] = prefix[i] + nums[i];
}
```

Range:

```java
int sum = prefix[right + 1] - prefix[left];
```

---

# 💻 Prefix Sum + HashMap Template

```java
Map<Integer, Integer> map = new HashMap<>();

map.put(0, 1);

int sum = 0;
int count = 0;

for (int num : nums) {

    sum += num;

    int required = sum - k;

    if (map.containsKey(required)) {
        count += map.get(required);
    }

    map.put(
        sum,
        map.getOrDefault(sum, 0) + 1
    );
}

return count;
```

---

# 🚨 Common Mistakes

## Mistake 1 — Forgetting `map.put(0, 1)`

Always remember:

```java
map.put(0, 1);
```

This handles subarrays that start from index `0`.

Example:

```text
nums = [3]
K = 3
```

Current sum:

```text
3
```

Required:

```text
3 - 3 = 0
```

If `0` wasn't already in the map, we would miss:

```text
[3]
```

---

## Mistake 2 — Using `prefix[R] - prefix[L]` with the n+1 array

If:

```text
prefix[0] = 0
```

then:

```text
sum(L...R)
=
prefix[R + 1] - prefix[L]
```

Remember the `+1`.

---

## Mistake 3 — Confusing Subarray and Subsequence

Prefix Sum generally deals with **contiguous ranges**.

```text
Subarray:
[2, 3, 4]
```

Subsequence:

```text
[2, 4]
```

can skip elements.

---

## Mistake 4 — Forgetting Integer Overflow

If the array values or length can be large, use:

```java
long
```

instead of:

```java
int
```

For example:

```java
long sum = 0;
```

and potentially:

```java
Map<Long, Integer> map = new HashMap<>();
```

---

# ⭐ Final Pattern Recognition

Don't memorize every Prefix Sum problem separately.

Remember:

```text
                    SUM
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
       RANGE                  SUBARRAY
          │                     │
          ↓                     ↓
     PREFIX SUM          What condition?
                                │
             ┌──────────────────┼─────────────────┐
             ↓                  ↓                 ↓
           SUM K              SUM 0          DIVISIBLE K
             ↓                  ↓                 ↓
       Prefix + Map       Prefix + Map    Prefix + Modulo
```

---

# 🏆 One-Line Memory Tricks

```text
RANGE SUM
    ↓
PREFIX SUM
```

```text
SUBARRAY SUM = K
    ↓
PREFIX SUM + HASHMAP
```

```text
ZERO SUM
    ↓
SAME PREFIX SUM
```

```text
DIVISIBLE BY K
    ↓
SAME PREFIX REMAINDER
```

```text
MULTIPLE RANGE QUERIES
    ↓
PREFIX SUM
```

```text
NEGATIVE NUMBERS + SUBARRAY SUM K
    ↓
PREFIX SUM + HASHMAP
```

---

# 🧩 Important LeetCode Problems

## 🟢 Basic

* **1480. Running Sum of 1d Array** → Basic Prefix Sum
* **303. Range Sum Query - Immutable** → Prefix Sum

## 🟡 Intermediate

* **560. Subarray Sum Equals K** → Prefix Sum + HashMap
* **525. Contiguous Array** → Prefix Sum + HashMap
* **974. Subarray Sums Divisible by K** → Prefix Sum + Modulo
* **523. Continuous Subarray Sum** → Prefix Sum + Modulo

## 🔴 Advanced

* **1074. Number of Submatrices That Sum to Target** → 2D Prefix Sum
* **437. Path Sum III** → Prefix Sum + HashMap

---

# 🧠 FINAL PREFIX SUM MENTAL MODEL

When you see a question, ask:

```text
                QUESTION
                    │
                    ↓
              Is SUM involved?
                    │
                   YES
                    ↓
          RANGE or SUBARRAY?
                    │
        ┌───────────┴───────────┐
        ↓                       ↓
      RANGE                  SUBARRAY
        │                       │
        ↓                       ↓
   PREFIX SUM             What is asked?
                                │
               ┌────────────────┼────────────────┐
               ↓                ↓                ↓
             Sum K            Sum 0          Divisible K
               │                │                │
               ↓                ↓                ↓
        Prefix + Map      Same Prefix       Same Remainder
                                │
                                ↓
                            HashMap
```

# 🔥 The Golden Rule

```text
PREFIX SUM =
"How much sum have I accumulated up to here?"
```

Then for a subarray:

```text
Current Prefix
      -
Previous Prefix
      =
Subarray Sum
```

So if you want:

```text
Subarray Sum = K
```

think:

```text
Current Prefix - Previous Prefix = K

Previous Prefix = Current Prefix - K
```

Therefore:

```text
        CURRENT SUM
             ↓
         sum - K
             ↓
       Search HashMap
             ↓
      Found → valid subarray
```

**The goal is not to memorize Prefix Sum problems. The goal is to recognize the relationship between two prefix sums.**
