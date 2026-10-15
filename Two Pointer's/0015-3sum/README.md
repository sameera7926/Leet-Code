# 15. 3Sum

## 🟡 Difficulty

Medium

## 🧩 Pattern

**Two Pointers**

---

## 📌 Problem

Given an integer array `nums`, find all unique triplets:

```text
nums[i] + nums[j] + nums[k] = 0
```

The answer must not contain duplicate triplets.

### Example

```text
Input:
nums = [-1,0,1,2,-1,-4]

Output:
[[-1,-1,2],[-1,0,1]]
```

---

# 💡 Core Idea

We need **3 numbers** whose sum is `0`.

Instead of checking every possible combination, we:

```text
Sort the array
      ↓
Fix one number
      ↓
Use Two Pointers for the other two
```

After sorting:

```text
[-4,-1,-1,0,1,2]
```

---

# 🚀 Approach

### Step 1: Sort

```java
Arrays.sort(nums);
```

Sorting allows us to:

* Use Two Pointers
* Move `left` and `right` intelligently
* Skip duplicates

---

### Step 2: Fix one number

```java
for (int i = 0; i < nums.length - 2; i++)
```

`i` represents the **first number** of our triplet.

Then:

```text
left = i + 1
right = n - 1
```

---

### Step 3: Calculate sum

```text
sum = nums[i] + nums[left] + nums[right]
```

Now there are 3 cases.

### If `sum == 0`

We found a valid triplet.

```text
save triplet
left++
right--
```

### If `sum < 0`

The sum is too small.

Because the array is sorted, increase the sum:

```text
left++
```

### If `sum > 0`

The sum is too large.

Decrease the sum:

```text
right--
```

---

# 👨‍💻 Java Code

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {

        List<List<Integer>> result = new ArrayList<>();

        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2; i++) {

            // Skip duplicate first values
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {

                int sum = nums[i] + nums[left] + nums[right];

                if (sum == 0) {

                    result.add(Arrays.asList(
                        nums[i],
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

                } else if (sum < 0) {

                    left++;

                } else {

                    right--;
                }
            }
        }

        return result;
    }
}
```

---

# 🔍 Code Explanation

### `result`

```java
List<List<Integer>> result = new ArrayList<>();
```

Stores all valid triplets.

---

### Sort

```java
Arrays.sort(nums);
```

Example:

```text
[-1,0,1,2,-1,-4]

        ↓

[-4,-1,-1,0,1,2]
```

Without sorting, the Two Pointer logic will not work.

---

### `i`

```java
for (int i = 0; i < nums.length - 2; i++)
```

`i` fixes the first number.

We stop at `n - 2` because we still need **two numbers** after `i`.

---

### `left`

```java
int left = i + 1;
```

Starts immediately after `i`.

---

### `right`

```java
int right = nums.length - 1;
```

Starts at the last element.

---

### Sum

```java
int sum = nums[i] + nums[left] + nums[right];
```

Checks whether the three numbers add up to `0`.

---

# 🔄 Why Do We Move `left` When `sum < 0`?

Suppose:

```text
nums = [-1,0,1,2]
```

and:

```text
-1 + 0 + 1 = 0
```

Now suppose:

```text
-1 + 0 + 0 = -1
```

The sum is too small.

Because the array is sorted, moving `left` rightward gives us a **larger number**.

Therefore:

```text
sum < 0 → left++
```

---

# 🔄 Why Do We Move `right` When `sum > 0`?

If:

```text
sum > 0
```

the sum is too large.

Because the array is sorted, moving `right` leftward gives us a **smaller number**.

Therefore:

```text
sum > 0 → right--
```

---

# 🧪 Dry Run

### Input

```text
[-1,0,1,2,-1,-4]
```

### After sorting

```text
[-4,-1,-1,0,1,2]
```

---

### `i = 0`

```text
nums[i] = -4
```

```text
left = 1
right = 5
```

Sum:

```text
-4 + (-1) + 2 = -3
```

Since:

```text
sum < 0
```

move:

```text
left++
```

Continue searching.

No valid triplet with `-4`.

---

### `i = 1`

```text
nums[i] = -1
left = 2
right = 5
```

Values:

```text
-1 + (-1) + 2
```

```text
= 0
```

Found:

```text
[-1,-1,2]
```

Move:

```text
left++
right--
```

Now:

```text
left = 3
right = 4
```

Calculate:

```text
-1 + 0 + 1 = 0
```

Found:

```text
[-1,0,1]
```

Final:

```text
[[-1,-1,2],[-1,0,1]]
```

---

# 🔁 Duplicate Handling

This is very important in 3Sum.

### Duplicate `i`

```java
if (i > 0 && nums[i] == nums[i - 1]) {
    continue;
}
```

Example:

```text
[-4,-1,-1,0,1,2]
     ↑  ↑
```

Both `-1`s don't need to start separate searches.

---

### Duplicate `left`

```java
while (left < right &&
       nums[left] == nums[left + 1]) {
    left++;
}
```

Skip repeated values after finding a triplet.

---

### Duplicate `right`

```java
while (left < right &&
       nums[right] == nums[right - 1]) {
    right--;
}
```

Also prevents duplicate triplets.

---

# ⚠️ Common Mistakes

### 1. Forgetting to sort

```java
Arrays.sort(nums);
```

is necessary.

### 2. Not skipping duplicate `i`

Can produce duplicate triplets.

### 3. Moving the wrong pointer

Remember:

```text
sum < 0 → left++

sum > 0 → right--

sum == 0 → save + move both
```

### 4. Forgetting `left < right`

The search should continue only while:

```java
while (left < right)
```

### 5. Not moving after finding a triplet

After:

```text
sum == 0
```

move both pointers.

---

# ⏱️ Complexity

### Time

```text
O(n²)
```

Sorting:

```text
O(n log n)
```

Two Pointer search:

```text
O(n²)
```

Overall:

```text
O(n²)
```

### Space

```text
O(1)
```

Extra space excluding the output list.

---

# 🎯 Interview Trigger

If you see:

> Find three numbers whose sum equals a target.

Think:

```text
3Sum
 ↓
Sort
 ↓
Fix one number
 ↓
Two Pointers
 ↓
Skip duplicates
```

---

# 🧠 30-Second Revision

```text
1. Sort the array.

2. Loop through i.

3. left = i + 1
   right = n - 1

4. Calculate:
   sum = nums[i] + nums[left] + nums[right]

5. sum == 0:
   save triplet
   skip duplicates
   left++
   right--

6. sum < 0:
   left++

7. sum > 0:
   right--

Time: O(n²)
Space: O(1)
```

## ⭐ Interview One-Liner

**Sort the array, fix one element, and use two pointers to find the other two elements while skipping duplicates.**
