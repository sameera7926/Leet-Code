# 11. Container With Most Water

## 🟡 Difficulty

Medium

## 🧩 Pattern

**Two Pointers**

---

## 📌 Problem

You are given an array `height`.

Each value represents the height of a vertical line.

Choose **two lines** that can hold the maximum amount of water.

### Example

```text
Input:
height = [1,8,6,2,5,4,8,3,7]

Output:
49
```

---

# 💡 Core Idea

The area of water is:

```text
Area = width × height
```

Where:

```text
width = right - left

height = min(height[left], height[right])
```

So:

```text
Area = (right - left) × min(height[left], height[right])
```

---

# 🚀 Approach: Two Pointers

Start with:

```text
left = 0
right = n - 1
```

This gives the **maximum possible width**.

Calculate the area.

Then move the pointer with the **smaller height**.

### Why move the smaller height?

The container's height is limited by the shorter line.

For example:

```text
left height  = 2
right height = 7
```

The water height is only:

```text
2
```

Moving the `right` pointer cannot increase the height beyond `2` if `left` stays there.

So we move:

```text
left++
```

We always try to find a taller shorter-side line.

---

# 👨‍💻 Java Code

```java
class Solution {
    public int maxArea(int[] height) {

        int left = 0;
        int right = height.length - 1;

        int maxArea = 0;

        while (left < right) {

            int width = right - left;

            int currentHeight = Math.min(
                height[left],
                height[right]
            );

            int area = width * currentHeight;

            maxArea = Math.max(maxArea, area);

            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }
}
```

---

# 🔍 Code Explanation

### `left`

```java
int left = 0;
```

Points to the first line.

---

### `right`

```java
int right = height.length - 1;
```

Points to the last line.

We start from both ends because this gives the maximum width.

---

### Width

```java
int width = right - left;
```

Distance between the two lines.

---

### Current Height

```java
int currentHeight = Math.min(
    height[left],
    height[right]
);
```

The shorter line decides how high the water can be.

---

### Area

```java
int area = width * currentHeight;
```

Formula:

```text
Area = width × shorter height
```

---

### Update Maximum

```java
maxArea = Math.max(maxArea, area);
```

Keep the largest area found so far.

---

### Move Pointer

```java
if (height[left] < height[right]) {
    left++;
} else {
    right--;
}
```

Move the pointer having the smaller height.

```text
smaller height → move that pointer
```

---

# 🧪 Dry Run

### Input

```text
[1,8,6,2,5,4,8,3,7]
```

Initial:

```text
left = 0
right = 8
```

Heights:

```text
height[left]  = 1
height[right] = 7
```

Width:

```text
8 - 0 = 8
```

Height:

```text
min(1,7) = 1
```

Area:

```text
8 × 1 = 8
```

Since:

```text
1 < 7
```

move:

```text
left++
```

---

### Important Maximum

Eventually:

```text
left = 1
right = 8
```

Values:

```text
8 and 7
```

Width:

```text
8 - 1 = 7
```

Height:

```text
min(8,7) = 7
```

Area:

```text
7 × 7 = 49
```

So:

```text
maxArea = 49
```

Final answer:

```text
49
```

---

# 🧠 Pointer Rule

Remember this:

```text
smaller height → move that pointer
```

Because:

```text
Area = width × shorter height
```

Width always decreases when we move a pointer.

Therefore, to improve the area, we need a chance to find a **taller shorter side**.

---

# ⚠️ Common Mistakes

### 1. Moving the taller pointer

Wrong:

```text
taller → move
```

Correct:

```text
shorter → move
```

---

### 2. Using the taller height

Wrong:

```java
Math.max(height[left], height[right])
```

Correct:

```java
Math.min(height[left], height[right])
```

The shorter line limits the water.

---

### 3. Starting both pointers at the same side

Wrong:

```java
left = 0;
right = 0;
```

Correct:

```java
left = 0;
right = height.length - 1;
```

---

# ⏱️ Complexity

### Time

```text
O(n)
```

Each pointer moves from one side toward the other only once.

### Space

```text
O(1)
```

Only a few variables are used.

---

# 🎯 Interview Trigger

When you see:

> Find two elements/lines that maximize an area.

Think:

```text
Container With Most Water
        ↓
Two Pointers
        ↓
left = 0
right = n - 1
        ↓
calculate area
        ↓
move smaller height
```

---

# ⭐ 30-Second Revision

```text
Area = (right - left)
       × min(height[left], height[right])

Start:
left = 0
right = n - 1

While left < right:

1. Calculate area.
2. Update maxArea.
3. If left height is smaller:
       left++
   Else:
       right--
```

### Interview One-Liner

**Start with two pointers at both ends, calculate the area, and always move the pointer with the smaller height because it limits the container.**
