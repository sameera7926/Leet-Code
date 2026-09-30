# 📏 Intervals — Pattern Recognition

**Intervals** are a very common DSA pattern.

An interval usually looks like:

```text id="int01"
[start, end]
```

Example:

```text id="int02"
[1, 5]
```

means:

```text id="int03"
1 ───────── 5
```

The main idea behind Interval problems is:

> Understand how different ranges **overlap, merge, separate, or fit inside each other**.

---

# 🧠 How to Identify an Interval Problem?

Don't look only for the word `"interval"`.

The question can be written in many different ways.

Look for:

```text id="int04"
Range
Interval
Start time / End time
Meeting
Schedule
Calendar
Overlapping
Merge
Non-overlapping
Free time
Insert a range
Remove overlapping intervals
Minimum rooms
Maximum meetings
Events
Time slots
```

These are strong signals that you should think:

```text id="int05"
📏 INTERVAL PATTERN
```

---

# 🎯 The Most Important First Step

When you see intervals:

```text id="int06"
[[1,3], [2,6], [8,10], [9,12]]
```

your **first thought should usually be:**

```text id="int07"
SORT
```

Most interval problems become much easier after sorting by:

```text id="int08"
START TIME
```

So:

```java id="int09"
Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
```

After sorting:

```text id="int10"
[1,3]
[2,6]
[8,10]
[9,12]
```

Now you can process intervals from left to right.

---

# 🔥 Golden Rule

Whenever you see:

```text id="int11"
INTERVALS
```

think:

```text id="int12"
1. Sort by start
2. Compare current interval with previous/current
3. Check overlap
4. Merge / count / remove / insert depending on question
```

---

# 1️⃣ Merge Intervals

This is the most important interval pattern.

Question may say:

> Merge all overlapping intervals.

Example:

```text id="int13"
[1,3]
[2,6]
[8,10]
[9,12]
```

Visual:

```text id="int14"
1────3
  2────────6

8──10
  9──────────12
```

After merging:

```text id="int15"
[1,6]
[8,12]
```

---

# 🧠 How to Recognize Merge Intervals?

Look for:

```text id="int16"
Merge overlapping intervals
Combine overlapping ranges
Combine time periods
Merge ranges
Return non-overlapping intervals
Combine meetings
```

Think:

```text id="int17"
SORT + MERGE
```

---

# 🎯 Step 1 — Sort

Input:

```text id="int18"
[[1,3], [2,6], [8,10], [9,12]]
```

Sorted by start:

```text id="int19"
[1,3]
[2,6]
[8,10]
[9,12]
```

---

# 🎯 Step 2 — Compare Intervals

Take:

```text id="int20"
current = [1,3]
next    = [2,6]
```

We need to determine whether they overlap.

---

# 🔥 The Most Important Overlap Condition

For two intervals:

```text id="int21"
[a,b]
[c,d]
```

assuming:

```text
a <= c
```

They overlap if:

```text id="int22"
c <= b
```

In words:

> The next interval's start is less than or equal to the current interval's end.

So:

```text id="int23"
current.end >= next.start
```

means:

```text id="int24"
OVERLAP
```

---

# 🧪 Example

```text id="int25"
[1,3]
[2,6]
```

Check:

```text id="int26"
3 >= 2
```

Yes.

Therefore:

```text id="int27"
OVERLAP
```

Merge them:

```text id="int28"
start = min(1,2) = 1
end   = max(3,6) = 6
```

Result:

```text id="int29"
[1,6]
```

---

# ❌ Example — No Overlap

```text id="int30"
[1,3]
[5,7]
```

Check:

```text id="int31"
3 >= 5
```

False.

Therefore:

```text id="int32"
NO OVERLAP
```

Keep both:

```text id="int33"
[1,3]
[5,7]
```

---

# 💻 Merge Intervals Template

```java id="int34"
public int[][] merge(int[][] intervals) {

    Arrays.sort(
        intervals,
        (a, b) -> Integer.compare(a[0], b[0])
    );

    List<int[]> result = new ArrayList<>();

    for (int[] interval : intervals) {

        if (result.isEmpty() ||
            result.get(result.size() - 1)[1] < interval[0]) {

            result.add(interval);

        } else {

            result.get(result.size() - 1)[1] =
                Math.max(
                    result.get(result.size() - 1)[1],
                    interval[1]
                );
        }
    }

    return result.toArray(new int[result.size()][]);
}
```

---

# 🧠 Merge Interval Mental Model

After sorting:

```text id="int35"
previous
   ↓
[1────6]

current
      [4────8]
```

If:

```text id="int36"
current.start <= previous.end
```

then:

```text id="int37"
OVERLAP
```

Merge:

```text id="int38"
[1────────8]
```

Otherwise:

```text id="int39"
NO OVERLAP
```

Start a new interval.

---

# 🔑 One-Line Memory Trick

```text id="int40"
current.start <= previous.end
        ↓
     OVERLAP
        ↓
MERGE using max end
```

---

# 2️⃣ Check If Two Intervals Overlap

Sometimes the question simply asks:

> Do these intervals overlap?

Example:

```text id="int41"
[1,5]
[4,8]
```

Visual:

```text id="int42"
1────────5
    4────────8
```

They overlap.

Condition:

```text id="int43"
max(start1, start2) <= min(end1, end2)
```

Therefore:

```text id="int44"
OVERLAP
```

---

# 🧠 Another Easy Way

If intervals are already sorted by start:

```text id="int45"
[a,b]
[c,d]
```

where:

```text
a <= c
```

then simply check:

```text id="int46"
c <= b
```

---

# 3️⃣ Insert Interval

Question:

> Insert a new interval into a list of non-overlapping sorted intervals and merge if necessary.

Example:

```text id="int47"
intervals:
[1,3]
[6,9]

newInterval:
[2,5]
```

Expected:

```text id="int48"
[1,5]
[6,9]
```

---

# 🧠 Insert Interval Has 3 Cases

This is extremely important.

For each existing interval, it can be:

```text id="int49"
1. Completely BEFORE new interval
2. OVERLAPPING new interval
3. Completely AFTER new interval
```

---

# Case 1 — Before

```text id="int50"
existing: [1,2]
new:      [4,7]
```

Existing interval is completely before the new interval.

Condition:

```text id="int51"
existing.end < new.start
```

So add existing interval.

---

# Case 2 — Overlapping

```text id="int52"
existing: [3,5]
new:      [4,8]
```

They overlap.

Merge:

```text id="int53"
new.start = min(new.start, existing.start)
new.end   = max(new.end, existing.end)
```

Result:

```text id="int54"
[3,8]
```

---

# Case 3 — After

```text id="int55"
existing: [10,12]
new:      [4,8]
```

Existing interval comes after the new interval.

So first add the new interval.

Then add the remaining intervals.

---

# 💻 Insert Interval Template

```java id="int56"
public int[][] insert(int[][] intervals, int[] newInterval) {

    List<int[]> result = new ArrayList<>();

    int i = 0;

    // Intervals before newInterval
    while (i < intervals.length &&
           intervals[i][1] < newInterval[0]) {

        result.add(intervals[i]);
        i++;
    }

    // Merge overlapping intervals
    while (i < intervals.length &&
           intervals[i][0] <= newInterval[1]) {

        newInterval[0] =
            Math.min(newInterval[0], intervals[i][0]);

        newInterval[1] =
            Math.max(newInterval[1], intervals[i][1]);

        i++;
    }

    result.add(newInterval);

    // Remaining intervals
    while (i < intervals.length) {

        result.add(intervals[i]);
        i++;
    }

    return result.toArray(new int[result.size()][]);
}
```

---

# 4️⃣ Non-Overlapping Intervals

Question:

> Find the minimum number of intervals that must be removed so that the remaining intervals do not overlap.

Example:

```text id="int57"
[1,2]
[2,3]
[3,4]
[1,3]
```

We need to remove intervals to make the remaining ones non-overlapping.

This is closely related to:

```text id="int58"
Greedy + Intervals
```

---

# 🧠 Important Idea

Sort by:

```text id="int59"
END TIME
```

This is different from the normal merge-interval problem.

For merging:

```text
SORT BY START
```

For selecting the maximum number of non-overlapping intervals:

```text
SORT BY END
```

Why?

Because we want to finish as early as possible and leave maximum room for future intervals.

---

# 🎯 Greedy Rule

Choose the interval with the smallest end.

Example:

```text id="int60"
[1,3]
[2,4]
[3,5]
```

If we choose:

```text id="int61"
[1,3]
```

we finish earlier than:

```text id="int62"
[2,4]
```

Therefore we leave more space for future intervals.

---

# 💻 Non-Overlapping Intervals

```java id="int63"
Arrays.sort(
    intervals,
    (a, b) -> Integer.compare(a[1], b[1])
);

int remove = 0;
int prevEnd = intervals[0][1];

for (int i = 1; i < intervals.length; i++) {

    if (intervals[i][0] < prevEnd) {

        // Overlap
        remove++;

    } else {

        // No overlap
        prevEnd = intervals[i][1];
    }
}

return remove;
```

---

# 🔥 Important Distinction

Remember:

```text id="int64"
MERGE INTERVALS
→ Sort by START
```

```text id="int65"
MAXIMUM NON-OVERLAPPING INTERVALS
→ Sort by END
```

This is one of the most important Interval rules.

---

# 5️⃣ Meeting Rooms

Interval problems are often disguised as **meeting problems**.

Example:

```text id="int66"
[0,30]
[5,10]
[15,20]
```

Question:

> Can a person attend all meetings?

This is simply an interval overlap problem.

---

# 🧠 Recognition

Look for:

```text id="int67"
Meeting
Schedule
Calendar
Conference
Event
Time slot
Start time
End time
```

Think:

```text id="int68"
INTERVALS
```

---

# 🎯 Meeting Room I

Question:

> Can one person attend all meetings?

Sort by start:

```text id="int69"
[0,30]
[5,10]
[15,20]
```

Compare:

```text id="int70"
previous.end
        vs
current.start
```

If:

```text id="int71"
current.start < previous.end
```

there is an overlap.

Therefore:

```text id="int72"
Cannot attend all meetings.
```

---

# 💻 Meeting Rooms I

```java id="int73"
Arrays.sort(
    intervals,
    (a, b) -> Integer.compare(a[0], b[0])
);

for (int i = 1; i < intervals.length; i++) {

    if (intervals[i][0] < intervals[i - 1][1]) {
        return false;
    }
}

return true;
```

---

# 6️⃣ Meeting Rooms II

This question is slightly different.

> What is the minimum number of meeting rooms required?

Example:

```text id="int74"
[0,30]
[5,10]
[15,20]
```

At time `5`:

```text id="int75"
[0,30]
[5,10]
```

Two meetings are happening.

Therefore we need:

```text id="int76"
2 rooms
```

---

# 🧠 How to Solve?

There are two common approaches.

### Approach 1

```text id="int77"
Sort by start
+
Min Heap of end times
```

### Approach 2

Separate:

```text id="int78"
start times
end times
```

and use two pointers.

---

# 💻 Meeting Rooms II — Min Heap

```java id="int79"
Arrays.sort(
    intervals,
    (a, b) -> Integer.compare(a[0], b[0])
);

PriorityQueue<Integer> minHeap =
    new PriorityQueue<>();

for (int[] interval : intervals) {

    if (!minHeap.isEmpty() &&
        minHeap.peek() <= interval[0]) {

        minHeap.poll();
    }

    minHeap.offer(interval[1]);
}

return minHeap.size();
```

The heap stores:

```text id="int80"
end times of currently active meetings
```

The smallest end time tells us which room becomes free first.

---

# 🧠 Meeting Rooms II Mental Model

```text id="int81"
New meeting starts
       ↓
Is any room free?
       ↓
YES → reuse room
       ↓
NO → need new room
```

---

# 7️⃣ Maximum Number of Overlapping Intervals

Sometimes the question asks:

> What is the maximum number of intervals active at the same time?

This is similar to:

```text id="int82"
Meeting Rooms II
```

Think:

```text id="int83"
Start Event → +1
End Event   → -1
```

Sort events by time and maintain:

```text id="int84"
currentActive
maxActive
```

---

# 🎯 Example

Intervals:

```text id="int85"
[1,5]
[2,6]
[4,8]
```

Events:

```text id="int86"
1 → +1
2 → +1
4 → +1
5 → -1
6 → -1
8 → -1
```

Active count:

```text id="int87"
time 1 → 1
time 2 → 2
time 4 → 3
time 5 → 2
time 6 → 1
time 8 → 0
```

Maximum:

```text id="int88"
3
```

---

# ⚠️ Boundary Condition — Very Important

Suppose:

```text id="int89"
[1,3]
[3,5]
```

Do these overlap?

Usually in meeting/scheduling problems, if one meeting ends exactly when another begins:

```text id="int90"
[1,3]
    [3,5]
```

they **do not overlap**.

So:

```text id="int91"
end == start
```

usually means:

```text id="int92"
NO OVERLAP
```

Therefore use:

```java id="int93"
current.start < previous.end
```

for overlap.

Not:

```java id="int94"
current.start <= previous.end
```

However, always check the exact problem definition because some interval problems treat endpoints differently.

---

# 🧠 Three Important Overlap Formulas

For:

```text id="int95"
A = [a,b]
B = [c,d]
```

### General overlap

```text id="int96"
max(a,c) <= min(b,d)
```

for inclusive intervals.

---

### After sorting by start

If:

```text id="int97"
a <= c
```

then overlap if:

```text id="int98"
c <= b
```

---

### No overlap

```text id="int99"
b < c
```

when `A` comes before `B`.

---

# 🔥 Interval Pattern — Start vs End Sorting

This is the biggest thing to remember.

## Sort by START when:

```text id="int100"
Merge intervals
Insert interval
Check overlap
Process intervals from left to right
```

Think:

```text id="int101"
SORT → START
```

---

## Sort by END when:

```text id="int102"
Maximum number of non-overlapping intervals
Minimum removals
Activity selection
```

Think:

```text id="int103"
SORT → END
```

---

# 🧠 Why Sort by Start for Merging?

Example:

```text id="int104"
[1,5]
[2,3]
[4,8]
```

After sorting:

```text id="int105"
[1,5]
[2,3]
[4,8]
```

We can process from left to right.

The current merged interval tells us how far we currently reach.

```text id="int106"
[1────────5]
    [2─3]
       [4────8]
```

Final:

```text id="int107"
[1────────8]
```

---

# 🧠 Why Sort by End for Selection?

Suppose:

```text id="int108"
[1,10]
[2,3]
[4,5]
```

If we choose:

```text id="int109"
[1,10]
```

we block everything else.

If we choose:

```text id="int110"
[2,3]
```

we finish early and can choose:

```text id="int111"
[4,5]
```

Therefore:

```text id="int112"
EARLIEST END
```

is the greedy idea.

---

# 🗺️ Pattern Recognition Cheat Sheet

| Question                          | Pattern               |
| --------------------------------- | --------------------- |
| Merge overlapping intervals       | Sort by Start         |
| Combine ranges                    | Sort by Start         |
| Insert interval                   | Sort/Process by Start |
| Check overlapping intervals       | Sort by Start         |
| Can attend all meetings?          | Sort by Start         |
| Minimum meeting rooms             | Start + Min Heap      |
| Maximum simultaneous events       | Start/End events      |
| Remove minimum intervals          | Sort by End           |
| Maximum non-overlapping intervals | Sort by End           |
| Activity selection                | Sort by End           |

---

# 🔥 How to Recognize From Different Question Forms

### Question:

> Merge all overlapping ranges.

Think:

```text id="int113"
SORT BY START
+
MERGE
```

---

### Question:

> Combine overlapping time periods.

Think:

```text id="int114"
SORT BY START
+
MERGE
```

---

### Question:

> Can a person attend all meetings?

Think:

```text id="int115"
SORT BY START
+
CHECK OVERLAP
```

---

### Question:

> Minimum number of meeting rooms?

Think:

```text id="int116"
SORT BY START
+
MIN HEAP
```

---

### Question:

> Remove the minimum number of intervals to make the rest non-overlapping.

Think:

```text id="int117"
SORT BY END
+
GREEDY
```

---

### Question:

> Find maximum number of activities that can be performed without overlap.

Think:

```text id="int118"
SORT BY END
+
GREEDY
```

---

# ⚡ Interval Decision Framework

When you see an interval problem:

```text id="int119"
             INTERVAL QUESTION
                     │
                     ↓
              What is being asked?
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      MERGE        OVERLAP       SELECT
        │            │            │
        ↓            ↓            ↓
   Sort Start    Sort Start    Sort End
        │            │            │
        ↓            ↓            ↓
     Merge        Compare       Greedy
                                  │
                                  ↓
                           Earliest End
```

If it involves meetings:

```text id="int120"
Meeting Question
      ↓
Interval Problem
      │
 ┌────┴─────┐
 ↓          ↓
Can attend  Min rooms
 ↓          ↓
Sort Start  Min Heap
```

---

# 💻 Basic Interval Templates

## Template 1 — Sort by Start

```java id="int121"
Arrays.sort(
    intervals,
    (a, b) -> Integer.compare(a[0], b[0])
);
```

Use for:

```text id="int122"
Merge
Insert
Overlap
Meeting conflicts
```

---

## Template 2 — Sort by End

```java id="int123"
Arrays.sort(
    intervals,
    (a, b) -> Integer.compare(a[1], b[1])
);
```

Use for:

```text id="int124"
Maximum non-overlapping
Minimum removals
Activity selection
Greedy selection
```

---

## Template 3 — Overlap Check

```java id="int125"
if (currentStart < previousEnd) {
    // overlap
}
```

Or for inclusive intervals:

```java id="int126"
if (currentStart <= previousEnd) {
    // overlap
}
```

Always check the problem's endpoint convention.

---

# 🚨 Common Mistakes

## Mistake 1 — Forgetting to Sort

Given:

```text id="int127"
[[5,7], [1,4], [2,6]]
```

Trying to merge directly makes the logic harder.

Usually:

```text id="int128"
SORT FIRST
```

Then:

```text id="int129"
[[1,4], [2,6], [5,7]]
```

---

## Mistake 2 — Sorting by the Wrong Value

For merge:

```text id="int130"
Sort by START
```

For maximum non-overlap:

```text id="int131"
Sort by END
```

---

## Mistake 3 — Using Wrong Boundary

For:

```text id="int132"
[1,3]
[3,5]
```

whether they overlap depends on the problem.

Don't blindly assume.

Check whether touching endpoints count as overlap.

---

## Mistake 4 — Updating End Incorrectly

When merging:

```text id="int133"
[1,10]
[2,5]
```

The result is:

```text id="int134"
[1,10]
```

not:

```text id="int135"
[1,5]
```

Therefore:

```java id="int136"
end = Math.max(end, currentEnd);
```

---

# 🏆 Final Mental Framework

When you see:

```text id="int137"
[start, end]
```

immediately think:

```text id="int138"
📏 INTERVAL
```

Then ask:

```text id="int139"
"What is the question asking me to do?"
```

### If it says:

```text id="int140"
MERGE / COMBINE
```

think:

```text id="int141"
SORT BY START
+
MERGE OVERLAPS
```

### If it says:

```text id="int142"
OVERLAP / CONFLICT
```

think:

```text id="int143"
SORT BY START
+
COMPARE END WITH NEXT START
```

### If it says:

```text id="int144"
MAXIMUM NON-OVERLAPPING
```

think:

```text id="int145"
SORT BY END
+
GREEDY
```

### If it says:

```text id="int146"
MINIMUM MEETING ROOMS
```

think:

```text id="int147"
SORT BY START
+
MIN HEAP OF END TIMES
```

---

# 🔑 One-Line Memory Tricks

```text id="int148"
INTERVALS
    ↓
SORT FIRST
```

```text id="int149"
MERGE
    ↓
SORT BY START
```

```text id="int150"
OVERLAP
    ↓
current.start vs previous.end
```

```text id="int151"
MAX NON-OVERLAPPING
    ↓
SORT BY END
    ↓
GREEDY
```

```text id="int152"
MEETING ROOMS
    ↓
START + END
    ↓
MIN HEAP / EVENT COUNT
```

---

# 🧩 Important LeetCode Problems

## 🟢 Basic

* **56. Merge Intervals** → Sort by Start + Merge
* **57. Insert Interval** → Before / Overlap / After
* **252. Meeting Rooms** → Sort + Check Overlap

## 🟡 Intermediate

* **253. Meeting Rooms II** → Min Heap
* **435. Non-overlapping Intervals** → Sort by End + Greedy
* **452. Minimum Number of Arrows to Burst Balloons** → Sort by End + Greedy

## 🔴 Advanced

* **759. Employee Free Time** → Merge Intervals
* **986. Interval List Intersections** → Two Pointers
* **1288. Remove Covered Intervals** → Sorting + Greedy

---

# ⭐ Final Pattern Recognition

```text id="int153"
                  INTERVAL
                     │
                     ↓
                  SORT
                     │
             ┌───────┴────────┐
             ↓                ↓
          START              END
             │                │
             ↓                ↓
       Merge / Overlap     Greedy
       Insert / Meeting    Selection
             │                │
             ↓                ↓
        Compare ranges    Earliest finish
```

## 🔥 Golden Rule

```text id="int154"
INTERVAL + MERGE
        ↓
SORT BY START
```

```text id="int155"
INTERVAL + MAXIMUM NON-OVERLAPPING
        ↓
SORT BY END
        ↓
GREEDY
```

```text id="int156"
INTERVAL + MEETING ROOMS
        ↓
SORT START
+
TRACK END TIMES
```

The main goal is **not to memorize individual interval problems**.

When you see an interval, first ask:

```text id="int157"
1. Do I need to MERGE?
2. Do I need to CHECK OVERLAP?
3. Do I need to SELECT maximum non-overlapping intervals?
4. Do I need to COUNT simultaneous intervals?
5. Do I need to INSERT a new interval?
```

Then choose the corresponding pattern.
