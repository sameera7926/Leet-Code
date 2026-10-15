# 18. 4Sum

## 🟡 Difficulty

Medium

## 🧩 Pattern

**Two Pointers**

---

## 📌 Problem

Given an integer array `nums` and an integer `target`, return all **unique quadruplets**:

```text
nums[a] + nums[b] + nums[c] + nums[d] = target
```

The answer must not contain duplicate quadruplets.

### Example

```text
Input:
nums = [1,0,-1,0,-2,2]
target = 0

Output:
[
    [-2,-1,1,2],
    [-2,0,0,2],
    [-1,0,0,1]
]
```

---

# 💡 Core Idea

4Sum is basically:

```text
4Sum = 2 fixed numbers + Two Pointers
```

We:

```text
Sort
  ↓
Fix i
  ↓
Fix j
  ↓
Use left and right pointers
  ↓
Find remaining 2 numbers
```

---

# 🚀 Approach

### Step 1: Sort the array

```java
Arrays.sort(nums);
```

Sorting helps us:

* Use Two Pointers
* Move pointers based on the sum
* Skip duplicates

---

### Step 2: Fix the first number

```java
for (int i = 0; i < nums.length - 3; i++)
```

`i` represents the first number.

---

### Step 3: Fix the second number

```java
for (int j = i + 1; j < nums.length - 2; j++)
```

`j` represents the second number.

Now we need two more numbers.

---

### Step 4: Use Two Pointers

```text
left = j + 1
right = n - 1
```

Calculate:

```text
sum = nums[i] + nums[j] + nums[left] + nums[right]
```

---

# 🔄 Pointer Movement

### If:

```text
sum == target
```

We found a quadruplet.

```text
add quadruplet
left++
right--
```

---

### If:

```text
sum < target
```

The sum is too small.

Because the array is sorted:

```text
left++
```

increases the sum.

---

### If:

```text
sum > target
```

The sum is too large.

So:

```text
right--
```

decreases the sum.

---

# 👨‍💻 Java Code

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {

        List<List<Integer>> result = new ArrayList<>();

        Arrays.sort(nums);

        int n = nums.length;

        for (int i = 0; i < n - 3; i++) {

            // Skip duplicate first values
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            for (int j = i + 1; j < n - 2; j++) {

                // Skip duplicate second values
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }

                int left = j + 1;
                int right = n - 1;

                while (left < right) {

                    long sum = (long) nums[i]
                             + nums[j]
                             + nums[left]
                             + nums[right];

                    if (sum == target) {

                        result.add(Arrays.asList(
                            nums[i],
                            nums[j],
                            nums[left],
                            nums[right]
                        ));

                        // Skip duplicate left values
                        while (left < right &&
                               nums[left] == nums[left + 1]) {
                            left++;
                        }

                        // Skip duplicate right values
                        while (left < right &&
                               nums[right] == nums[right - 1]) {
                            right--;
                        }

                        left++;
                        right--;

                    } else if (sum < target) {

                        left++;

                    } else {

                        right--;
                    }
                }
            }
        }

        return result;
    }
}
```

---

# 🔍 Why `long` for Sum?

This is important in 4Sum.

```java
long sum = (long) nums[i]
         + nums[j]
         + nums[left]
         + nums[right];
```

The values can be large enough that adding four `int` values can overflow.

So we convert the first value to `long`:

```java
(long) nums[i]
```

Then the entire addition is performed using `long`.

### Remember:

```text
3Sum → int is usually enough

4Sum → use long for safety
```

---

# 🧠 Code Explanation

### `i`

```java
for (int i = 0; i < n - 3; i++)
```

Fixes the **first number**.

We stop at `n - 3` because three positions are still needed.

---

### `j`

```java
for (int j = i + 1; j < n - 2; j++)
```

Fixes the **second number**.

After `j`, we still need:

```text
left + right
```

---

### `left`

```java
int left = j + 1;
```

Starts immediately after `j`.

---

### `right`

```java
int right = n - 1;
```

Starts at the last element.

---

# 🧪 Dry Run

### Input

```text
nums = [1,0,-1,0,-2,2]
target = 0
```

### Sort

```text
[-2,-1,0,0,1,2]
```

---

### Fix `i`

```text
i = 0
nums[i] = -2
```

### Fix `j`

```text
j = 1
nums[j] = -1
```

Now:

```text
left = 2
right = 5
```

Values:

```text
-2  -1   0   0   1   2
 ↑   ↑   ↑           ↑
 i   j  left       right
```

Calculate:

```text
-2 + (-1) + 0 + 2
= -1
```

Since:

```text
sum < target
```

move:

```text
left++
```

---

Eventually:

```text
-2 + (-1) + 1 + 2
= 0
```

Found:

```text
[-2,-1,1,2]
```

---

Another combination:

```text
-2 + 0 + 0 + 2
= 0
```

Found:

```text
[-2,0,0,2]
```

And later:

```text
-1 + 0 + 0 + 1
= 0
```

Found:

```text
[-1,0,0,1]
```

### Final Output

```text
[
    [-2,-1,1,2],
    [-2,0,0,2],
    [-1,0,0,1]
]
```

---

# 🔁 Duplicate Handling

4Sum has **four possible duplicate positions**, so duplicate handling is important.

### Duplicate `i`

```java
if (i > 0 && nums[i] == nums[i - 1]) {
    continue;
}
```

---

### Duplicate `j`

```java
if (j > i + 1 && nums[j] == nums[j - 1]) {
    continue;
}
```

Notice:

```text
j > i + 1
```

We only skip `j` when it is a duplicate **within the current `i` search**.

---

### Duplicate `left`

```java
while (left < right &&
       nums[left] == nums[left + 1]) {
    left++;
}
```

---

### Duplicate `right`

```java
while (left < right &&
       nums[right] == nums[right - 1]) {
    right--;
}
```

---

# 🆚 3Sum vs 4Sum

| 3Sum              | 4Sum                  |
| ----------------- | --------------------- |
| Fix 1 number      | Fix 2 numbers         |
| 2 pointers        | 2 pointers            |
| 1 loop + pointers | 2 loops + pointers    |
| `O(n²)`           | `O(n³)`               |
| Usually `int sum` | Use `long sum` safely |

### Easy Memory Trick

```text
3Sum:
1 fixed + 2 pointers

4Sum:
2 fixed + 2 pointers
```

---

# ⚠️ Common Mistakes

### 1. Forgetting to sort

```java
Arrays.sort(nums);
```

is necessary.

### 2. Not skipping duplicates

Can produce duplicate quadruplets.

### 3. Moving the wrong pointer

Remember:

```text
sum < target → left++

sum > target → right--

sum == target → save + move both
```

### 4. Using `int` carelessly for the sum

Use:

```java
long sum
```

to avoid integer overflow.

### 5. Wrong duplicate condition for `j`

Use:

```java
j > i + 1
```

not simply:

```java
j > 0
```

---

# ⏱️ Complexity

### Time

```text
O(n³)
```

Why?

```text
i loop       → O(n)
j loop       → O(n)
two pointers → O(n)

Total        → O(n³)
```

### Space

```text
O(1)
```

Extra space excluding the output list.

---

# 🎯 Interview Trigger

When you see:

> Find four numbers whose sum equals a target.

Think:

```text
4Sum
 ↓
Sort
 ↓
Fix i
 ↓
Fix j
 ↓
Two Pointers
 ↓
Skip duplicates
```

---

# 🧠 30-Second Revision

```text
1. Sort the array.

2. Loop i.

3. Skip duplicate i.

4. Loop j from i + 1.

5. Skip duplicate j.

6. left = j + 1
   right = n - 1

7. Calculate:
   sum = i + j + left + right

8. sum == target:
      save
      skip duplicates
      left++
      right--

9. sum < target:
      left++

10. sum > target:
      right--

Time: O(n³)
Space: O(1)
```

## ⭐ Interview One-Liner

**Sort the array, fix two numbers, then use two pointers to find the remaining two numbers while skipping duplicates.**
