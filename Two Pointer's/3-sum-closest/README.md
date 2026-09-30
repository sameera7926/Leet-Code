# 🔢 LeetCode 16 — 3Sum Closest

## 🟢 Problem

Given an integer array `nums` and an integer `target`, choose **three different elements** from the array whose sum is **closest to the target**.

Return the sum of those three numbers.

### Example

```text
Input:
nums = [-1, 2, 1, -4]
target = 1

Output:
2
```

### Why?

Possible combinations:

```text
-1 + 2 + 1 = 2
-1 + 2 + (-4) = -3
-1 + 1 + (-4) = -4
2 + 1 + (-4) = -1
```

Compare them with target `1`:

```text
|2 - 1|  = 1   ← closest
|-3 - 1| = 4
|-4 - 1| = 5
|-1 - 1| = 2
```

Therefore:

```text
Answer = 2
```

---

# 🧠 Pattern

## Sorting + Two Pointers

This problem is an extension of the **3Sum** pattern.

```text
Sort the array
      ↓
Fix one element
      ↓
Use two pointers
      ↓
left →      ← right
      ↓
Calculate 3 numbers' sum
      ↓
Compare with target
      ↓
Move left/right
```

---

# 💡 Main Idea

First, sort the array.

Then:

1. Fix one number using `i`.
2. Set:

   * `left = i + 1`
   * `right = n - 1`
3. Calculate:

```java
sum = nums[i] + nums[left] + nums[right];
```

4. Check whether this sum is closer to `target` than our previous answer.
5. If:

```text
sum < target
```

we need a **larger sum**, so:

```text
left++
```

6. If:

```text
sum > target
```

we need a **smaller sum**, so:

```text
right--
```

7. If:

```text
sum == target
```

we found the exact answer, so return it immediately.

---

# 💻 Java Code

```java
import java.util.Arrays;

class Solution {
    public int threeSumClosest(int[] nums, int target) {

        Arrays.sort(nums);

        int closest = nums[0] + nums[1] + nums[2];

        for (int i = 0; i < nums.length - 2; i++) {

            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {

                int sum = nums[i] + nums[left] + nums[right];

                // Exact target found
                if (sum == target) {
                    return sum;
                }

                // Update closest sum
                if (Math.abs(sum - target) < Math.abs(closest - target)) {
                    closest = sum;
                }

                // Move pointers
                if (sum < target) {
                    left++;
                } else {
                    right--;
                }
            }
        }

        return closest;
    }
}
```

---

# 🔍 Dry Run

### Input

```text
nums = [-1, 2, 1, -4]
target = 1
```

## Step 1 — Sort

```text
[-1, 2, 1, -4]
```

becomes:

```text
[-4, -1, 1, 2]
```

Initially:

```text
closest = -4 + (-1) + 1
        = -4
```

---

## Step 2 — First iteration

```text
i = 0

nums[i] = -4

left = 1
right = 3
```

Array:

```text
[-4, -1, 1, 2]
 ↑    ↑     ↑
 i   left  right
```

Calculate:

```text
sum = -4 + (-1) + 2
    = -3
```

Compare:

```text
target = 1

|-3 - 1| = 4
|-4 - 1| = 5
```

`-3` is closer.

So:

```text
closest = -3
```

Since:

```text
sum < target
-3 < 1
```

we need a bigger sum.

Move:

```text
left++
```

---

## Step 3

Now:

```text
i = 0
left = 2
right = 3
```

```text
[-4, -1, 1, 2]
 ↑        ↑  ↑
 i       left right
```

Calculate:

```text
sum = -4 + 1 + 2
    = -1
```

Compare:

```text
|-1 - 1| = 2
|-3 - 1| = 4
```

So:

```text
closest = -1
```

Again:

```text
sum < target
-1 < 1
```

Therefore:

```text
left++
```

Now:

```text
left = 3
right = 3
```

Since:

```text
left < right
```

is false, this loop ends.

---

# 🔄 Step 4 — Move `i`

Now:

```text
i = 1
```

So:

```text
nums[i] = -1
left = 2
right = 3
```

Array:

```text
[-4, -1, 1, 2]
     ↑   ↑  ↑
     i  left right
```

Calculate:

```text
sum = -1 + 1 + 2
    = 2
```

Compare:

```text
|2 - 1| = 1
|-1 - 1| = 2
```

`2` is closer.

Therefore:

```text
closest = 2
```

Now:

```text
sum = 2
target = 1
```

Since:

```text
sum > target
```

move:

```text
right--
```

Now:

```text
right = 2
```

Loop ends because:

```text
left == right
```

---

# ✅ Final Answer

```text
closest = 2
```

Therefore:

```text
Output = 2
```

---

# 📊 Complete Dry Run Table

| `i` | `left` | `right` | Sum | Target | Difference | Closest |
| --: | -----: | ------: | --: | -----: | ---------: | ------: |
|   0 |      1 |       3 |  -3 |      1 |          4 |      -3 |
|   0 |      2 |       3 |  -1 |      1 |          2 |      -1 |
|   1 |      2 |       3 |   2 |      1 |          1 |       2 |

Final:

```text
Answer = 2
```

---

# 🤔 Why Do We Sort?

Sorting allows us to intelligently move the pointers.

For example:

```text
sum < target
```

We need a **bigger sum**.

Because the array is sorted:

```text
left++
```

gives us a bigger number.

Similarly:

```text
sum > target
```

We need a **smaller sum**.

So:

```text
right--
```

gives us a smaller number.

Without sorting, we cannot make these pointer decisions safely.

---

# 🧩 Why `Math.abs()`?

We need to know **how close** the current sum is to the target.

Example:

```text
target = 10

sum = 8
difference = |8 - 10| = 2

sum = 13
difference = |13 - 10| = 3
```

Therefore `8` is closer.

In Java:

```java
Math.abs(sum - target)
```

gives the absolute difference.

---

# ⏱️ Complexity

### Time Complexity

```text
Sorting        → O(n log n)
Two-pointer    → O(n²)

Overall        → O(n²)
```

### Space Complexity

```text
O(1) extra space
```

Ignoring the space used internally by the sorting implementation.

---

# 🎯 Interview Explanation

You can explain the solution like this:

> "First, I sort the array. Then I fix one element and use two pointers for the remaining two elements. If the current sum is smaller than the target, I move the left pointer to increase the sum. If the sum is greater than the target, I move the right pointer to decrease the sum. At every step, I compare the absolute difference between the current sum and target and store the closest sum. If the sum exactly equals the target, I return immediately."

---

# 🔑 Key Takeaways

```text
3Sum Closest
     ↓
Sort
     ↓
Fix one element
     ↓
Two Pointers
     ↓
Calculate sum
     ↓
sum < target → left++
sum > target → right--
sum == target → return
     ↓
Track closest
```

### Pattern to Remember:

**"Sort → Fix → Two Pointers → Compare Difference"**

---

# 🏷️ Tags

`Array` `Sorting` `Two Pointers` `LeetCode` `Medium`
