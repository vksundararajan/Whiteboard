# For-Meta

- [x] [Longest Substring Without Repeating Characters](#1-longest-substring-without-repeating-characters)
- [x] [String to Integer (atoi)](#2-string-to-integer-atoi)
- [x] [Roman to Integer](#3-roman-to-integer)
- [x] [3Sum](#4-3sum)
- [x] [Remove Duplicates from Sorted Array](#5-remove-duplicates-from-sorted-array)
- [x] [Next Permutation](#6-next-permutation)
- [x] [Multiply Strings](#7-multiply-strings)
- [x] [Group Anagrams](#8-group-anagrams)
- [x] [Add Binary](#9-add-binary)
- [x] [Minimum Window Substring](#10-minimum-window-substring)
- [x] [Merge Sorted Array](#11-merge-sorted-array)
- [x] [Valid Palindrome](#12-valid-palindrome)
- [x] [Product of Array Except Self](#13-product-of-array-except-self)
- [ ] [Integer to English Words](#14-integer-to-english-words)
- [x] [Move Zeroes](#15-move-zeroes)
- [ ] [Validate IP Address](#16-validate-ip-address)
- [ ] [Subarray Sum Equals K](#17-subarray-sum-equals-k)
- [x] [Valid Palindrome II](#18-valid-palindrome-ii)



## 1. Longest Substring Without Repeating Characters
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



## 2. String to Integer (atoi)
```python
s = "42" # Output: 42
s = " -042" # Output: -42
s = " -1337c0d3" # Output: -1337
s = "0-1" # Output: 0
s = "words and 987" # Output: 0


s = s.strip()
if not s:
  print(0)
else:
  res, sign, i = 0, 1, 0
  if s[i] == '+':
    i += 1
  elif s[i] == '-':
    i += 1
    sign = -1
  while i < len(s) and s[i].isdigit():
    res = res * 10 + int(s[i])
    if sign * res > 2**31 - 1:
      print(2**31 - 1)
      break
    elif sign * res < -2**31:
      print(-2**31)
      break
    i += 1
  print(sign * res)
```




## 3. Roman to Integer
```python
s = "III" # Output: 3
s = "LVIII" # Output: 58
s = "MCMXCIV" # Output: 1994

def romanToInt(c):
  match c:
    case 'I': return 1
    case 'V': return 5
    case 'X': return 10
    case 'L': return 50
    case 'C': return 100
    case 'D': return 500
    case 'M': return 1000
    case _: return 0
res = prev = 0
for ch in reversed(s):
  curr = romanToInt(ch)
  if curr < prev:
    res -= curr
  else:
    res += curr
    prev = curr
print(res)
```

 

## 4. 3Sum
```python
nums = [-1,0,1,2,-1,-4] # Output: [[-1,-1,2],[-1,0,1]]
nums = [0,1,1] # Output: []
nums = [0,0,0] # Output: [[0,0,0]]

nums.sort()
zero, se = 0, set()
for i in range(0, len(nums)):
  j = i + 1
  k = len(nums) - 1
  while j < k:
    su = nums[i] + nums[j] + nums[k]
    if su == zero:
      se.add((nums[i], nums[j], nums[k]))
      j += 1
      k -= 1
    elif su < zero:
      j += 1
    else:
      k -= 1
print(list(se))
```

 

## 5. Remove Duplicates from Sorted Array
```python
nums = [1,1,2] # Output: 2, nums = [1,2,_]
nums = [0,0,1,1,1,2,2,3,3,4] # Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]

if not nums:
  print(0)
i = 0
for j in range(i, len(nums)):
  if nums[i] != nums[j]:
    i += 1
    nums[i] = nums[j]
print(i + 1)
```

 

## 6. Next Permutation
```python
arr = [1,2,3] # Output: [1,3,2]
arr = [3,2,1] # Output: [1,2,3]
arr = [1,1,5] # Output: [1,5,1]

bp, n = -1, len(arr)
for i in range(n-2,-1,-1):
  if arr[i] >= arr[i+1]: continue
  bp = i
  for j in range(n-1,i,-1):
    if arr[j] > arr[bp]:
      arr[j], arr[bp] = arr[bp], arr[j]
      break
  break
arr[bp+1:] = reversed(arr[bp+1:])
print(arr)
```



## 7. Multiply Strings
```python
num1 = "2", num2 = "3" # Output: "6"
num1 = "123", num2 = "456" # Output: "56088"

print(str(int(num1)*int(num2)))
```



## 8. Group Anagrams
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



## 9. Add Binary
```python
a = "11", b = "1" # Output: "100"
a = "1010", b = "1011" # Output: "10101"

result = []
i, j = len(a) - 1, len(b) - 1
carry = 0
while i >= 0 or j >= 0 or carry:
  bit_a = int(a[i]) if i >= 0 else 0
  bit_b = int(b[j]) if j >= 0 else 0
  t = bit_a + bit_b + carry
  result.append(str(t % 2))
  carry = t // 2
  i -= 1
  j -= 1
print(''.join(reversed(result)))
```


## 10. Minimum Window Substring
```python
s = "ADOBECODEBANC", t = "ABC" # Output: "BANC"
s = "a", t = "a" # Output: "a"
s = "a", t = "aa" # Output: ""

need = {}
for c in t:
  need[c] = need.get(c, 0) + 1

count = {}
l = start = end = 0
missing = len(t)

for r, c in enumerate(s, 1):
  if need.get(c, 0) > 0:
    count[c] = count.get(c, 0) + 1
    if count[c] <= need[c]:
      missing -= 1
  while missing == 0:
    if end == 0 or r - l < end - start:
      start, end = l, r
    if s[l] in need:
      count[s[l]] -= 1
      if count[s[l]] < need[s[l]]:
        missing += 1
    l += 1
  
print(s[start:end])
```


## 11. Merge Sorted Array
```python
nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3 # Output: [1,2,2,3,5,6]
nums1 = [1], m = 1, nums2 = [], n = 0 # Output: [1]
nums1 = [0], m = 0, nums2 = [1], n = 1 # Output: [1]

space = len(nums1) - 1
while n > 0 and m > 0:
  if nums2[n-1] >= nums1[m-1]:
    nums1[space] = nums2[n-1]
    n -= 1
  else:
    nums1[space] = nums1[m-1]
    m -= 1
  space -= 1
while n > 0:
  nums1[space] = nums2[n-1]
  n -= 1
  space -= 1
print(nums1)
```



## 12. Valid Palindrome
```python
s = "A man, a plan, a canal: Panama" # Output: true
s = "race a car" # Output: false
s = " " # Output: true

s = ''.join(c.lower() for c in s if c.isalnum())
i, j, isPal = 0, len(s) - 1, True
while i < j:
  if s[i] != s[j]:
    isPal = False
    break
  i += 1
  j -= 1
print(isPal)
```



## 13. Product of Array Except Self
```python
nums = [1,2,3,4] # Output: [24,12,8,6]
nums = [-1,1,0,-3,3] # Output: [0,0,9,0,0]

result = [1] * len(nums)
prefix = 1
for i in range(0, len(nums)):
  result[i] = prefix
  prefix *= nums[i]
postfix = 1
for i in range(len(nums) - 1, -1, -1):
  result[i] *= postfix
  postfix *= nums[i]
print(result)
```



## 14. Integer to English Words
```python
num = 123 # Output: "One Hundred Twenty Three"
num = 12345 # Output: "Twelve Thousand Three Hundred Forty Five"
num = 1234567 # Output: "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"

class Solution:
  def numberToWords(self, num: int) -> str:
    if num == 0:
      return "Zero"
    ones = ["", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine"]
    tens = ["", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"]
    teens = ["Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"]
    suffixes = ["", "Thousand", "Million", "Billion", "Trillion", "Quadrillion", "Quintillion", "Sextillion", "Septillion", "Octillion", "Nonillion", "Decillion"]
    words = []
    i = 0
    while num > 0:
      triplet = num % 1000
      num = num // 1000
      if triplet == 0:
        i += 1
        continue
      temp = []
      if triplet // 100 > 0:
        temp.append(ones[triplet // 100])
        temp.append("Hundred")
      if triplet % 100 >= 10 and triplet % 100 <= 19:
        temp.append(teens[triplet % 10])
      else:
        if triplet % 100 >= 20:
          temp.append(tens[triplet % 100 // 10])
        if triplet % 10 > 0:
          temp.append(ones[triplet % 10])
      if i > 0:
        temp.append(suffixes[i])
      words = temp + words
      i += 1
    return " ".join(words)
```



## 15. Move Zeroes
```python
nums = [0,1,0,3,12] # Output: [1,3,12,0,0]
nums = [0] # Output: [0]

j = 0
for i in range(0, len(nums)):
  if nums[i] != 0 and nums[j] == 0:
    nums[i], nums[j] = nums[j], nums[i]
  if nums[j] != 0:
    j += 1
print(nums)
```



## 16. Validate IP Address
```python
queryIP = "172.16.254.1" # Output: "IPv4"
queryIP = "2001:0db8:85a3:0:0:8A2E:0370:7334" # Output: "IPv6"
queryIP = "256.256.256.256" # Output: "Neither"

if '.' in queryIP:
  blocks = queryIP.split('.')
  if len(blocks) == 4:
    valid = True
    for block in blocks:
      if not (block.isdigit() and 0 <= int(block) <= 255 and str(int(block)) == block):
        valid = False
        break
    print("IPv4" if valid else "Neither")
elif ':' in queryIP:
  blocks = queryIP.split(':')
  if len(blocks) == 8:
    valid = True
    hex_digits = '0123456789abcdefABCDEF'
    for block in blocks:
      if not (1 <= len(block) <= 4):
        valid = False
        break
      for char in block:
        if char not in hex_digits:
          valid = False
          break
      if not valid:
        break
    print("IPv6" if valid else "Neither")
else:
  print("Neither")
```



## 17. Subarray Sum Equals K
```python
nums = [1,1,1], k = 2 # Output: 2
nums = [1,2,3], k = 3 # Output: 2

from collections import defaultdict
prefixSum = 0
valToCount = defaultdict(int)
valToCount[0] = 1 # This accounts for single sized subarrays
ans = 0
for val in nums:
  prefixSum += val
  target = prefixSum - k
  if target in valToCount:
    ans += valToCount[target]
  valToCount[prefixSum] += 1
print(ans)
```



## 18. Valid Palindrome II
```python
s = "aba" # Output: true
s = "abca" # Output: true
s = "abc" # Output: false

i, j, isPal = 0, len(s) - 1, True
while i < j:
  if s[i] == s[j]:
    i += 1
    j -= 1
  else:
    isPal = (s[i:j] == s[i:j][::-1] or s[i+1:j+1] == s[i+1:j+1][::-1])
    break
print(isPal)
```

