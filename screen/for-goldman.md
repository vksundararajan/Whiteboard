# For-Goldman

- [x] [First Unique Character in a String (Easy)](#2-first-unique-character-in-a-string-easy)
- [x] [Design HashMap (Easy)](#3-design-hashmap-easy)
- [x] [Two Sum (Easy)](#4-two-sum-easy)
- [x] [Robot Return to Origin (EASY)](#5-robot-return-to-origin-easy)
- [x] [Group Anagrams (Medium)](#6-group-anagrams-medium)
- [x] [Fraction to Recurring Decimal (Medium)](#7-fraction-to-recurring-decimal-Medium)
- [x] [Longest Substring Without Repeating Characters (Medium)](#8-longest-substring-without-repeating-characters-medium)
- [x] [Kth Largest Element in an Array (Medium)](#9-kth-largest-element-in-an-array-medium)
- [ ] [Open the Lock (Medium)](#10-open-the-lock-medium)
- [x] [Maximum Sum Circular Subarray (Medium)](#11-maximum-sum-circular-subarray-medium)
- [x] [Minimum Path Sum (MEDIUM)](#12-minimum-path-sum-medium)
- [x] [Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold (MEDIUM)](#13-number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold-medium)
- [x] [Car Pooling (MEDIUM)](#14-car-pooling-medium)
- [ ] [Number of Islands (MEDIUM)](#15-number-of-islands-medium)
- [ ] [Container With Most Water (MEDIUM)](#16-container-with-most-water-medium)
- [ ] [Trapping Rain Water (Hard)](#17-Trapping-Rain-Water-Hard)
- [ ] [Median of Two Sorted Arrays (HARD)](#18-Median-of-two-Sorted-Arrays-HARD)
---

## 1. High Five (Easy)
```python
items = [[1, 91], [1, 92], [2, 93], [2, 97], [1, 60], [2, 77], [1, 65], [1, 87], [1, 100], [2, 100], [2, 76]] 
# [[1, 87], [2, 88]]

from collections import defaultdict
from heapq import nlargest

d, l = defaultdict(list), []
for i, j in items:
  d[i].append(j)
for i in range(1, len(d)+1):
  avg = sum(nlargest(5, d[i])) // 5
  l.append([i, avg])
print(l)
```

## 2. First Unique Character in a String (Easy)
```python
s = "leetcode" # Output: 0
s = "loveleetcode" # Output: 2
s = "aabb" # Output: -1

d, c = {}, -1
for i in s:
  d[i] = d.get(i, 0) + 1
for i in range(0, len(s)):
  if d[s[i]] == 1:
    c = i
    break
print(c)
```

## 3. Design HashMap (Easy)
```python
class MyHashMap:
  def __init__(self):
    self.hash = {}
  def put(self, key: int, value: int) -> None:
    self.hash[key] = value
  def get(self, key:int) -> int:
    return self.hash[key] if key in self.hash else -1
  def remove(self, key:int) -> None:
    if key in self.hash:
      del self.hash[key]

m = MyHashMap()
m.put(1, 10)     # Map: {1: 10}
m.put(2, 20)     # Map: {1: 10, 2: 20}

print(m.get(1))  # Output: 10 (key 1 exists)
print(m.get(3))  # Output: -1 (key 3 doesn't exist)

m.remove(2)      # Map becomes: {1: 10}
print(m.get(2))  # Output: -1 (key 2 was removed)
```

## 4. Two Sum (Easy)
```python
nums = [2,7,11,15], target = 9 # Output: [0,1]
nums = [3,2,4], target = 6 # Output: [1,2]
nums = [3,3], target = 6 # Output: [0,1]

d = {}
for i in range(len(nums)):
  c = abs(nums[i] - target)
  if c in d:
    print([d[c], i])
    break
  d[nums[i]] = i
else:
  print([])
```

## 5. Robot Return to Origin (EASY)
```python
moves = "UD" # Output: true
moves = "LL" # Output: false

x = y = 0
for m in moves:
  if m == 'U': y -= 1
  elif m == 'D': y += 1
  elif m == 'L': x -= 1
  elif m == 'R': x += 1
print(x == y == 0)
```

## 6. Group Anagrams (Medium)
```python
strs = ["eat","tea","tan","ate","nat","bat"] # Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
strs = [""] # Output: [[""]]
strs = ["a"] # Output: [["a"]]

from collections import defaultdict
h = defaultdict(list)
for i in strs:
  ans = ''.join(sorted(i))
  h[ans].append(i)
print(list(h.values()))
```

## 7. Fraction to Recurring Decimal (Medium)
```python
numerator = 1, denominator = 2 # Output: "0.5"
numerator = 2, denominator = 1 # Output: "2"
numerator = 4, denominator = 333 # Output: "0.(012)"

if numerator == 0:
  print('0')
else:
  l = []
  if numerator > 0 and denominator < 0 or numerator < 0 and denominator > 0:
    l.append('-')
  n, d = abs(numerator), abs(denominator)
  l.append(str(n // d))
  r = n % d
  if r == 0:
    print(''.join(l))
  else:
    l.append('.')
    p = {}
    while r != 0 and r not in p:
      p[r] = len(l)
      r *= 10
      l.append(str(r // d))
      r %= d
    if r in p:
      l.insert(p[r], '(')
      l.append(')')
    print(''.join(l))
```

## 8. Longest Substring Without Repeating Characters (Medium)
```python
s = "abcabcbb" # Output: 3
s = "bbbbb" # Output: 1
s = "pwwkew" # Output: 3

x = set()
left = 0
maxLen = 0
for i in range(len(s)):
  if s[i] not in x:
    x.add(s[i])
    maxLen = max(maxLen, len(x))
  else:
    while s[i] in x:
      x.remove(s[left])
      left += 1
    x.add(s[i])
print(maxLen)
```

## 9. Kth Largest Element in an Array (Medium)
```python
nums = [3,2,1,5,6,4], k = 2 # Output: 5
nums = [3,2,3,1,2,4,5,5,6], k = 4 # Output: 4

from collections import Counter

kCount = k
d = Counter(nums)
for i in range(max(nums), min(nums)-1, -1):
  if d.get(i, 0) > 0:
    kCount -= d[i]
  if kCount <= 0:
    print(i)
    break
```

## 10. Open the Lock (Medium)
```python
deadends = ["0201","0101","0102","1212","2002"], target = "0202" # Output: 6
deadends = ["8888"], target = "0009" # Output: 1
deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888" # Output: -1

from collections import deque

deadEndSet = set(deadends)
queue = deque()
queue.append(('0000', 0))
visited = set('0000')
found = False
while queue:
  curStr, curSteps = queue.popleft()
  if curStr == target:
    print(curSteps)
    found = True
    break
  if curStr in deadEndSet:
    continue
  for i in range(4):
    digit = int(curStr[i])
    for dir in [1, -1]:
      newDigit = (digit + dir) % 10
      newStr = curStr[:i] + str(newDigit) + curStr[i+1:]
      if newStr not in visited:
        visited.add(newStr)
        queue.append((newStr, curSteps+1))
if not found:
  print(-1)
```

## 11. Maximum Sum Circular Subarray (Medium)
```python
nums = [1,-2,3,-2] # Output: 3
nums = [5,-3,5] # Output: 10
nums = [-3,-2,-3] # Output: -2

cmn = cmx = mn = mx = tot = nums[0]
for i in range(1, len(nums)):
  cmx = max(nums[i], cmx + nums[i])
  mx = max(mx, cmx)
  cmn = min(nums[i], cmn + nums[i])
  mn = min(mn, cmn)
  tot += nums[i]
if mn == tot:
  print(mx)
else:
  print(max(mx, tot - mn))
```

## 12. Minimum Path Sum (MEDIUM)
```python
grid = [[1,3,1],[1,5,1],[4,2,1]] # Output: 7
grid = [[1,2,3],[4,5,6]] # Output: 12
grid = [[1,2],[1,1]] # Output: 3

from itertools import accumulate

dp = list(accumulate(grid[0]))
for i in range(1, len(grid)):
  for j in range(0, len(dp)):
    dp[j] = grid[i][j] + (dp[j] if j == 0 else min(dp[j], dp[j-1]))
print(dp[-1])
```

## 13. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold (MEDIUM)
```python
arr = [2,2,2,2,5,5,5,8], k = 3, threshold = 4 # Output: 3
arr = [11,13,17,23,29,31,7,5,2,3], k = 3, threshold = 5 # Output: 6

c = 0
t = sum(arr[:k])
if t // k >= threshold:
  c += 1
for i in range(len(arr) - k):
  t = t - arr[i] + arr[i + k]
  if t // k >= threshold:
    c += 1
print(c)
```

## 14. Car Pooling (MEDIUM)
```python
trips = [[2,1,5],[3,3,7]], capacity = 4 # Output: false
trips = [[2,1,5],[3,3,7]], capacity = 5 # Output: true

loc = [0] * 1001
last, b = 0, True
for p, src, dst in trips:
  loc[src] += p
  loc[dst] -= p
  last = max(last, dst)
for i in range(1, last+1):
  if i > 0:
    loc[i] += loc[i-1]
  if loc[i] > capacity:
    b = False
    break
print(b)
```

## 15. Number of Islands (MEDIUM)
```python
grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
] # Output: 1

grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
] # Output: 3

if not grid:
  print(0)
else:
  def dfs(i, j):
    if i < 0 or i >= len(grid) or j < 0 or j >= len(grid[0]) or grid[i][j] != '1':
      return
    grid[i][j] = '0'  # mark as visited
    dfs(i+1, j)
    dfs(i-1, j)
    dfs(i, j+1)
    dfs(i, j-1)

  num_islands = 0
  for i in range(len(grid)):
    for j in range(len(grid[0])):
      if grid[i][j] == '1':
        num_islands += 1
        dfs(i, j)
  print(num_islands)
```

## 16. Container With Most Water (MEDIUM)
```python
height = [1,8,6,2,5,4,8,3,7] # Output: 49
height = [1,1] # Output: 1

left = 0
right = len(height) - 1
maxArea = 0
while left < right:
  currentArea = min(height[left], height[right]) * (right - left)
  maxArea = max(maxArea, currentArea)
  if height[left] < height[right]:
    left += 1
  else:
    right -= 1
print(maxArea)
```

## 17. Trapping Rain Water (Hard)
```python
height = [0,1,0,2,1,0,1,3,2,1,2,1] # Output: 6
height = [4,2,0,3,2,5] # Output: 9

left = 0
right = len(height) - 1
left_max = height[left]
right_max = height[right]
water = 0
while left < right:
  if left_max < right_max:
    left += 1
    left_max = max(left_max, height[left])
    water += left_max - height[left]
  else:
    right -= 1
    right_max = max(right_max, height[right])
    water += right_max - height[right]
print(water)
```

## 18. Median of Two Sorted Arrays (HARD)
```python
nums1 = [1,3], nums2 = [2] # Output: 2.00000
nums1 = [1,2], nums2 = [3,4] # Output: 2.50000

n, m = len(nums1), len(nums2)
i = j = m1 = m2 0

# Find median.
for count in range(0, (n + m) // 2 + 1):
  m2 = m1
  if i < n and j < m:
    if nums1[i] > nums2[j]:
      m1 = nums2[j]
      j += 1
    else:
      m1 = nums1[i]
      i += 1
  elif i < n:
    m1 = nums1[i]
    i += 1
  else:
    m1 = nums2[j]
    j += 1

# Check if the sum of n and m is odd.
if (n + m) % 2 == 1:
  print(float(m1))
else:
  ans = float(m1) + float(m2)
  print(ans / 2.0)
```