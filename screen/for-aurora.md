# For-Aurora

- [x] [1. Number of Distinct Islands](#1-number-of-distinct-islands)
- [x] [2. Diameter of Binary Tree](#2-diameter-of-binary-tree)
- [x] [3. LRU Cache](#3-lru-cache)
- [x] [4. Word Search II](#4-word-search-ii)
- [x] [5. High Five (Easy)](#5-high-five-easy)
- [x] [6. First Unique Character in a String (Easy)](#6-first-unique-character-in-a-string-easy)
- [x] [7. Design HashMap (Easy)](#7-design-hashmap-easy)
- [x] [8. Two Sum (Easy)](#8-two-sum-easy)
- [x] [9. Robot Return to Origin (EASY)](#9-robot-return-to-origin-easy)
- [x] [10. Group Anagrams (Medium)](#10-group-anagrams-medium)
- [x] [11. Fraction to Recurring Decimal (Medium)](#11-fraction-to-recurring-decimal-medium)
- [x] [12. Longest Substring Without Repeating Characters (Medium)](#12-longest-substring-without-repeating-characters-medium)
- [x] [13. Kth Largest Element in an Array (Medium)](#13-kth-largest-element-in-an-array-medium)
- [x] [14. Maximum Sum Circular Subarray (Medium)](#14-maximum-sum-circular-subarray-medium)
- [x] [15. Minimum Path Sum (MEDIUM)](#15-minimum-path-sum-medium)
- [x] [16.  Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold (MEDIUM)](#16-number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold-medium)
- [x] [17. Car Pooling (MEDIUM)](#17-car-pooling-medium)
- [x] [18. String to Integer (atoi)](#18-string-to-integer-atoi)
- [x] [19. Roman to Integer](#19-roman-to-integer)
- [x] [20. 3Sum](#20-3sum)
- [x] [21. Remove Duplicates from Sorted Array](#21-remove-duplicates-from-sorted-array)
- [x] [22. Next Permutation](#22-next-permutation)
- [x] [23. Multiply Strings](#23-multiply-strings)
- [x] [24. Add Binary](#24-add-binary)
- [x] [25. Minimum Window Substring](#25-minimum-window-substring)
- [x] [26. Merge Sorted Array](#26-merge-sorted-array)
- [x] [27. Valid Palindrome](#27-valid-palindrome)
- [x] [28. Product of Array Except Self](#28-product-of-array-except-self)
- [x] [29. Move Zeroes](#29-move-zeroes)
- [x] [30. Valid Palindrome II](#30-valid-palindrome-ii)

  
### 1. Number of Distinct Islands
```c++
grid = [[1,1,0,0,0],[1,1,0,0,0],[0,0,0,1,1],[0,0,0,1,1]]
# Output: 1

grid = [[1,1,0,1,1],[1,0,0,0,0],[0,0,0,0,1],[1,1,0,1,1]]
# Output: 3


#include <bits/stdc++.h>
using namespace std;

int main() {
  vector<vector<int>> grid = {
    {1,1,0,1,1},
    {1,0,0,0,0},
    {0,0,0,0,1},
    {1,1,0,1,1}
  };

  int m = grid.size();
  int n = grid[0].size();
  int dirs[5] = {-1, 0, 1, 0, -1};
  unordered_set<string> paths;

  function<void(int,int,int,string&)> dfs = [&](int i, int j, int dir, string &path) {
    grid[i][j] = 0;
    path += to_string(dir);
    for (int h = 1; h < 5; h++) {
      int x = i + dirs[h - 1];
      int y = j + dirs[h];
      if (x >= 0 && y >= 0 && x < m && y < n && grid[x][y] == 1) {
        dfs(x, y, h, path);
      }
    }
    path += to_string(dir);
  };

  for (int i = 0; i < m; i++) {
    for (int j = 0; j < n; j++) {
      if (grid[i][j] == 1) {
        string path;
        dfs(i, j, 0, path);
        paths.insert(path);
      }
    }
  }

  cout << paths.size() << endl;
  return 0;
}
```


### 2. Diameter of Binary Tree
```c++
#include <bits/stdc++.h>
using namespace std;

struct Node {
  int val;
  Node *left;
  Node *right;
  Node(int x): val(x), left(nullptr), right(nullptr) {}
};

int height(Node *root, int &d) {
  if (root == NULL) return 0;
  int left = height(root->left, d);
  int right = height(root->right, d);
  d = max(d, left + right);
  return max(left, right) + 1;
}

int diameterOfBinaryTree(Node *root) {
  int dia = 0;
  height(root, dia);
  return dia;
}

int main() {
  Node* root = new Node(1);
  root->left = new Node(2);
  root->right = new Node(3);
  root->left->left = new Node(4);
  root->left->right = new Node(5);

  cout << diameterOfBinaryTree(root) << endl;
  return 0;
}
```


### 3. LRU Cache
```c++
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
# Output: [null, null, null, 1, null, -1, null, -1, 3, 4]


#include <bits/stdc++.h>
#include <unordered_map>
using namespace std;

class LRUCache {
public:
  struct Node {
    int key;
    int val;
    Node* prev;
    Node* next;
    Node(int k, int v): key(k), val(v), prev(nullptr), next(nullptr) {}
  };

  size_t cap;
  unordered_map<int, Node*> m;
  Node* head = new Node(-1, -1);
  Node* tail = new Node(-1, -1);

  LRUCache(int capacity) {
    cap = capacity;
    head->next = tail;
    tail->prev = head;
  }

  void addNode(Node* newNode) {
    newNode->next = head->next;
    head->next->prev = newNode;
    head->next = newNode;
    newNode->prev = head;
  }

  void deleteNode(Node* delNode) {
    delNode->prev->next = delNode->next;
    delNode->next->prev = delNode->prev;
  }

  int get(int key) {
    if(m.find(key) != m.end()) {
      Node* resNode = m[key];
      int ans = resNode->val;
      m.erase(key);
      deleteNode(resNode);
      addNode(resNode);
      m[key] = head->next;
      return ans;
    }
    return -1;
  }

  void put(int key, int value) {
    if(m.find(key) != m.end()) {
      Node* curr = m[key];
      m.erase(key);
      deleteNode(curr);
    }
    if(m.size() == cap) {
      m.erase(tail->prev->key);
      deleteNode(tail->prev);
    }
    addNode(new Node(key, value));
    m[key] = head->next;
  }
};

int main() {
  LRUCache lru(2);
  lru.put(1, 1);
  lru.put(2, 2);
  cout << lru.get(1) << endl;
  lru.put(3, 3);
  cout << lru.get(2) << endl;
  lru.put(4, 4);
  cout << lru.get(1) << endl;
  cout << lru.get(3) << endl;
  cout << lru.get(4) << endl;
  return 0;
}
```



### 4. Word Search II
```c++
board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
# Output: ["eat","oath"]

board = [["a","b"],["c","d"]], words = ["abcb"]
# Output: []
```


### 5. High Five (Easy)
```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
  // Output: [[1, 87], [2, 88]]
  vector<vector<int>> items = {{1, 91}, {1, 92}, {2, 93}, {2, 97}, {1, 60}, {2, 77}, {1, 65}, {1, 87}, {1, 100}, {2, 100}, {2, 76}}; 

  unordered_map<int, vector<int>> m;
  vector<pair<int, int>> v;

  for (auto i: items) m[i[0]].push_back(i[1]);

  for (auto [i, j]: m) {
    sort(j.begin(), j.end(), greater<int>());
    int top5 = accumulate(j.begin(), j.begin() + 5, 0);
    int avg = top5 / 5;
    v.push_back({i, avg});
  }

  for (auto [i, j]: v) {
    cout << i << " " << j << endl;
  }

  return 0;
}
```


### 6. First Unique Character in a String (Easy)
```c++
#include <bits/stdc++.h>
using namespace std;

int firstUniqChar(string s) {
  unordered_map<char, int> m;
  for (auto i: s) m[i]++;

  for (int i = 0; i < (int) s.size(); i++) {
    if (m[s[i]] == 1) {
      return i;
    }
  }

  return -1;
}

int main() {
  cout << firstUniqChar("leetcode") << endl; // Output: 0
  cout << firstUniqChar("loveleetcode") << endl; // Output: 2
  cout << firstUniqChar("aabb") << endl; // Output: -1
}
```

### 7. Design HashMap (Easy)
```c++
#include <bits/stdc++.h>
using namespace std;

class MyHashMap {
private:
  unordered_map<int, int> m;

public:
  void put(int key, int val) {
    m[key] = val;
  }

  int get(int key) {
    if (m.count(key)) {
      return m[key];
    }
    return -1;
  } 

  void remove(int key) {
    if (m.count(key)) {
      m.erase(key);
    }
  }
};

int main() {
  MyHashMap m;
  m.put(1, 10);
  m.put(2, 20);

  cout << m.get(1) << endl;
  cout << m.get(3) << endl;

  m.remove(2);
  cout << m.get(2) << endl;
  return 0;
}
```


### 8. Two Sum (Easy)
```c++
#include <bits/stdc++.h>
using namespace std;

vector<int> twoSum(vector<int> nums, int target) {
  unordered_map<int, int> m;

  for (int i = 0; i < (int) nums.size(); i++) {
    int x = target - nums[i];
    if (m.count(x)) {
      return {m[x], i};
    }
    m[nums[i]] = i;
  }

  return {};
}

int main() {
  for (auto i: twoSum({2,7,11,15}, 9)) { cout << i; } cout << endl; 
  // Output: [0,1]
  for (auto i: twoSum({3,2,4}, 6)) { cout << i; } cout << endl; 
  // Output: [1,2]
  for (auto i: twoSum({3,3}, 6)) { cout << i; } cout << endl; 
  // Output: [0,1]

  return 0;
}
```


### 9. Robot Return to Origin (EASY)
```c++
#include <bits/stdc++.h>
using namespace std;

bool judgeCircle(string moves) {
  int x = 0;
  int y = 0;

  for (auto m: moves) {
    if (m == 'U') y++;
    else if (m == 'D') y--;
    else if (m == 'L') x--;
    else if (m == 'R') x++;
  }

  return (x == 0 && y == 0);
}

int main() {
  cout << boolalpha << judgeCircle("UD") << endl; // Output: true
  cout << boolalpha << judgeCircle("LL") << endl; // Output: false
}
```


### 10. Group Anagrams (Medium)
```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
  vector<string> strs = {"eat","tea","tan","ate","nat","bat"};
  unordered_map<string, vector<string>> m;

  for (auto s: strs) {
    string c = s;
    sort(s.begin(), s.end());
    m[s].push_back(c);
  }

  for (auto [i, j]: m) {
    for (auto k: j) {
      cout << k << " ";
    }
    cout << endl;
  }

  return 0;
}
```


### 11. Fraction to Recurring Decimal (Medium)
```c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

string fractionToDecimal(ll numerator, ll denominator) {
  if (numerator == 0) return 0;

  string res;
  if ((numerator > 0 && denominator < 0) || (numerator < 0 && denominator > 0)) {
    res.push_back('-');
  }

  ll n = llabs(numerator);
  ll d = llabs(denominator);
  res += to_string(n / d);
  int r = n % d;
  if (r == 0) {
    return res;
  }

  res.push_back('.');
  unordered_map<ll, int> pos;
  while (r != 0 && !pos.count(r)) {
    pos[r] = (int) res.size();
    r *= 10;
    res.push_back(char((r / d) + '0'));
    r %= d;
  }

  if (r != 0) {
    res.insert(pos[r], 1, '(');
    res.push_back(')');
  }

  return res;
}

int main() {
  cout << fractionToDecimal(1, 2) << endl; // Output: "0.5"
  cout << fractionToDecimal(2, 1) << endl; // Output: "2"
  cout << fractionToDecimal(4, 333) << endl; // Output: "0.(012)"

  return 0;
}
```


### 12. Longest Substring Without Repeating Characters (Medium)
```c++
#include <bits/stdc++.h>
using namespace std;

int lengthOfLongestSubstring(string s) {
  unordered_set<char> x;
  int maxLen = 0;
  int left = 0;

  for (int i = 0; i < (int) s.size(); i++) {
    if (!x.count(s[i])) {
      x.insert(s[i]);
      maxLen = max(maxLen, (int) x.size());
    } else {
      while (x.count(s[i])) {
        x.erase(s[left++]);
      }
      x.insert(s[i]);
    }
  }

  return maxLen;
}

int main() {
  cout << lengthOfLongestSubstring("abcabcbb") << endl; // Output: 3
  cout << lengthOfLongestSubstring("bbbbb") << endl; // Output: 1
  cout << lengthOfLongestSubstring("pwwkew") << endl; // Output: 3

  return 0;
}
```


### 13. Kth Largest Element in an Array (Medium)
```c++
#include <bits/stdc++.h>
using namespace std;

int findKthLargest(vector<int> nums, int k) {
  unordered_map<int, int> m;
  for (auto n: nums) m[n]++;

  int mx = *max_element(nums.begin(), nums.end());
  int mn = *min_element(nums.begin(), nums.end());
  int kCount = k;
  for (int i = mx; i >= mn; i--) {
    if (m.count(i)) {
      kCount -= m[i];
    }
    if (kCount <= 0) {
      return i;
    }
  }
  return 0;
}

int main() {
  cout << findKthLargest({3,2,1,5,6,4}, 2) << endl; // Output: 5
  cout << findKthLargest({3,2,3,1,2,4,5,5,6}, 4) << endl; // Output: 4

  return 0;
}
```


### 14. Maximum Sum Circular Subarray (Medium)
```c++
#include <bits/stdc++.h>
using namespace std;

int maxSubarraySumCircular(vector<int> nums) {
  int cmx, cmn, mx, mn, tot;
  cmx = cmn = mx = mn = tot = nums[0];

  for (int i = 1; i < (int) nums.size(); i++) {
    cmx = max(nums[i], nums[i] + cmx);
    mx = max(mx, cmx);
    cmn = min(nums[i], nums[i] + cmn);
    mn = min(mn, cmn);
    tot += nums[i];
  }

  if (mn == tot) {
    return mx;
  } else {
    return max(mx, tot - mn);
  }
}

int main() {
  cout << maxSubarraySumCircular({1,-2,3,-2}) << endl; // Output: 3
  cout << maxSubarraySumCircular({5,-3,5}) << endl; // Output: 10
  cout << maxSubarraySumCircular({-3,-2,-3}) << endl; // Output: -2

  return 0;
}
```


### 15. Minimum Path Sum (MEDIUM)
```c++
#include <bits/stdc++.h>
using namespace std;

int minPathSum(vector<vector<int>>& grid) {
  int m = grid.size();
  int n = grid[0].size();

  vector<int> dp(n);
  dp[0] = grid[0][0];
  for (int i = 1; i < n; i++) {
    dp[i] = dp[i - 1] + grid[0][i];
  }

  for (int i = 1; i < m; i++) {
    for (int j = 0; j < n; j++) {
      if (j == 0) {
        dp[j] += grid[i][j];
      } else {
        dp[j] = grid[i][j] + min(dp[j], dp[j - 1]);
      }
    }
  }

  return dp[n - 1];
}

int main() {
  vector<vector<int>> grid1 = {{1,3,1},{1,5,1},{4,2,1}};
  vector<vector<int>> grid2 = {{1,2,3},{4,5,6}};
  vector<vector<int>> grid3 = {{1,2},{1,1}};

  cout << minPathSum(grid1) << endl; // Output: 7
  cout << minPathSum(grid2) << endl; // Output: 12
  cout << minPathSum(grid3) << endl; // Output: 3

  return 0;
}
```


### 16.  Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold (MEDIUM)
```c++
#include <bits/stdc++.h>
using namespace std;

int numOfSubarrays(vector<int> arr, int k, int threshold) {
  int c = 0;
  int t = accumulate(arr.begin(), arr.begin() + k, 0);
  if (t / k >= threshold) {
    c += 1;
  }

  for (int i = 0; i < (int) arr.size() - k; i++) {
    t = t - arr[i] + arr[i + k];
    if (t / k >= threshold) {
      c += 1;
    }
  }

  return c;
}

int main() {
  cout << numOfSubarrays({2,2,2,2,5,5,5,8}, 3, 4) << endl; // Output: 3
  cout << numOfSubarrays({11,13,17,23,29,31,7,5,2,3}, 3, 5) << endl; // Output: 6

  return 0;
}
```


### 17. Car Pooling (MEDIUM)
```c++
#include <bits/stdc++.h>
using namespace std;

bool carPooling(vector<vector<int>> trips, int capacity) {
  vector<int> loc(1001, 0);
  int last = 0;
  for (auto t: trips) {
    int pos = t[0], src = t[1], dst = t[2];
    loc[src] += pos;
    loc[dst] -= pos;
    last = max(last, dst);
  }

  for (int i = 1; i < last; i++) {
    loc[i] += loc[i - 1];
    if (loc[i] > capacity) {
      return false;
    }
  }

  return true;
}

int main() {
  vector<vector<int>> grid1 = {{2,1,5},{3,3,7}};
  vector<vector<int>> grid2 = {{2,1,5},{3,3,7}};

  cout << carPooling(grid1, 4) << endl; // Output: false
  cout << carPooling(grid2, 5) << endl; // Output: True

  return 0;
}
```


### 18. String to Integer (atoi)
```c++
#include <bits/stdc++.h>
using namespace std;

int StringToInteger(string s) {
  int i = 0;
  for (char c: s) {
    if (isspace(c)) i++;
  }

  int sign = 1;
  if (s[i] == '-') {
    i++;
    sign = -1;
  } else if (s[i] == '+') {
    i++;
  }

  long long res;
  while (i < (int) s.size() && isdigit(s[i])) {
    res = res * 10 + (s[i] - '0');
    i++;
  }

  return res * sign;
}

int main() {
  cout << StringToInteger("42") << endl; // Output: 42
  cout << StringToInteger(" -042") << endl; // Output: -42
  cout << StringToInteger(" -1337c0d3") << endl; // Output: -1337
  cout << StringToInteger("0-1") << endl; // Output: 0
  cout << StringToInteger("words and 987") << endl; // Output: 0

  return 0;
}
```


### 19. Roman to Integer
```c++
#include <bits/stdc++.h>
using namespace std;

int romanToInteger(string s) {
  auto romanToInt = [](char c) {
    switch(c) {
      case 'I': return 1;
      case 'V': return 5;
      case 'X': return 10;
      case 'L': return 50;
      case 'C': return 100;
      case 'D': return 500;
      case 'M': return 1000;
      default: return 0;
    }
  };

  int res = 0, prev = 0;
  for (int i = (int) s.size(); i >= 0; i--) {
    int curr = romanToInt(s[i]);
    if (curr < prev) {
      res -= curr;
    } else {
      res += curr;
      prev = curr;
    }
  }

  return res;
}

int main() {
  cout << romanToInteger("III") << endl; // Output: 3
  cout << romanToInteger("LVIII") << endl; // Output: 58
  cout << romanToInteger("MCMXCIV") << endl; // Output: 1994

  return 0;
}
```


### 20. 3Sum
```c++
#include <bits/stdc++.h>
using namespace std;

int main() {
  vector<int> nums = {-1,0,1,2,-1,-4}; // Output: [[-1,-1,2],[-1,0,1]]
  // vector<int> nums = {0,1,1}; // Output: []
  // vector<int> nums = {0,0,0}; // Output: [[0,0,0]]

  set<vector<int>> res;
  sort(nums.begin(), nums.end());
  for (int i = 0; i < (int) nums.size(); i++) {
    int j = i + 1;
    int k = (int) nums.size() - 1;
    while (j < k) {
      int t = nums[i] + nums[j] + nums[k];
      if (t == 0) {
        res.insert({nums[i], nums[j], nums[k]});
        j++; k--;
      } else if (t < 0) {
        j++;
      } else {
        k--;
      }
    }
  }

  for (auto i: res) {
    for (auto j: i) {
      cout << j << " ";
    }
    cout << endl;
  }
}
```


### 21. Remove Duplicates from Sorted Array
```c++
#include <bits/stdc++.h>
using namespace std;

int removeDuplicates(vector<int> nums) {
  if ((int) nums.size() != 0) {
    int i = 0;
    for (int j = 0; j < (int) nums.size(); j++) {
      if (nums[i] != nums[j]) {
        i++;
        nums[i] = nums[j];
      }
    }
    return i + 1;
  }

  return 0;
}

int main() {
  cout << removeDuplicates({1,1,2}) << endl; // Output: 2
  cout << removeDuplicates({0,0,1,1,1,2,2,3,3,4}) << endl; // Output: 5

  return 0;
}
```


### 22. Next Permutation
```c++
```


### 23. Multiply Strings
```c++
#include <bits/stdc++.h>
using namespace std;

string multiplyStrings(string num1, string num2) {
  return to_string(stoll(num1) * stoll(num2));
}

int main() {
  cout << multiplyStrings("2", "3") << endl; // Output: 6
  cout << multiplyStrings("123", "456") << endl; // Output: 56088

  return 0;
}
```


### 24. Add Binary
```c++
#include <bits/stdc++.h>
using namespace std;

string addBinary(string b1, string b2) {
  string res;
  int i = b1.size() - 1;
  int j = b2.size() - 1;
  int carry = 0;

  while (i >= 0 || j >= 0 || carry) {
    int bit_a = (i >= 0) ? b1[i] - '0' : 0;
    int bit_b = (j >= 0) ? b2[j] - '0' : 0;
    int t = bit_a + bit_b + carry;
    res += to_string(t % 2);
    carry = t / 2;
    i--; j--;
  }

  reverse(res.begin(), res.end());
  return res;
}

int main() {
  cout << addBinary("11", "1") << endl; // Output: "100"
  cout << addBinary("1010", "1011") << endl; // Output: "10101"

  return 0;
}
```


### 25. Minimum Window Substring
```c++
#include <bits/stdc++.h>
using namespace std;

string minimunWindowSubstring(string s, string t) {
  unordered_map<char, int> need;
  for (char c: t) need[c]++;

  unordered_map<char, int> count;
  int l = 0, start = 0, end = 0;
  int missing = (int) t.size();
  for (int r = l; r <= (int) s.size(); r++) {
    char c = s[r - 1];
    if (need.count(c) && need[c] > 0) {
      count[c]++;
      if (count[c] <= need[c]) {
        missing--;
      }
    }

    while (missing == 0) {
      if (end == 0 || r - l < end - start) {
        start = l;
        end = r;
      }
      if (need.count(s[l])) {
        count[s[l]]--;
        if (count[s[l]] < need[s[l]]) {
          missing++;
        }
      }
      l++;
    }
  }

  if (end == 0) {
    return "";
  } else {
    return s.substr(start, end - start);
  }
} 

int main() {
  cout << minimunWindowSubstring("ADOBECODEBANC", "ABC") << endl; // Output: "BANC"
  cout << minimunWindowSubstring("a", "a") << endl; // Output: "a"
  cout << minimunWindowSubstring("a", "aa") << endl; // Output: ""

  return 0;
}
```


### 26. Merge Sorted Array
```c++
#include <bits/stdc++.h>
using namespace std;

vector<int> mergeSortedArray(vector<int> nums1, int m, vector<int> nums2, int n) {
  int space = (int) nums1.size() - 1;
  while (m > 0 && n > 0) {
    if (nums2[n - 1] >= nums1[m - 1]) {
      nums1[space] = nums2[n - 1];
      n--;
    } else {
      nums1[space] = nums1[m - 1];
      m--;
    }
    space--;
  }

  while (n > 0) {
    nums1[space] = nums2[n - 1];
    n--; space--;
  }

  return nums1;
}

int main() {
  // vector<int> v = mergeSortedArray({1}, 1, {}, 0); // Output: [1]
  // vector<int> v = mergeSortedArray({0}, 0, {1}, 1); // Output: [1]
  // Output: [1,2,2,3,5,6]
  vector<int> v = mergeSortedArray({1,2,3,0,0,0}, 3, {2,5,6}, 3);
  for(auto &i: v) cout << i << " ";

  return 0;
}
```

### 27. Valid Palindrome
```c++
#include <bits/stdc++.h>
using namespace std;

bool validPal(string str) {
  string s = "";
  for (char &c: str) {
    if (isalnum(c)) s.push_back(tolower(c));
  }

  int i = 0;
  int j = s.size() - 1;
  while (i < j) {
    if (s[i] == s[j]) {
      i++; j--;
    } else {
      return false;
    }
  }

  return true;
}

int main() {
  cout << validPal("A man, a plan, a canal: Panama") << endl; // Output: true
  cout << validPal("race a car") << endl; // Output: false
  cout << validPal(" ") << endl; // Output: true

  return 0;
}
```


### 28. Product of Array Except Self
```c++
#include <bits/stdc++.h>
using namespace std;

vector<int> productExceptSelf(vector<int> nums) {
  vector<int> res((int) nums.size(), 1);

  int prefix = 1;
  for (int i = 0; i < (int) nums.size(); i++) {
    res[i] = prefix;
    prefix *= nums[i];
  }

  int postfix = 1;
  for (int i = (int) nums.size() - 1; i >= 0; i--) {
    res[i] *= postfix;
    postfix *= nums[i];
  }

  return res;
}

int main() {
  // vector<int> v = productExceptSelf({-1,1,0,-3,3}); // Output: [0,0,9,0,0]]
  vector<int> v = productExceptSelf({1,2,3,4}); // Output: [24,12,8,6]
  for(auto &i: v) cout << i << " ";

  return 0;
}
```


### 29. Move Zeroes
```c++
#include <bits/stdc++.h>
using namespace std;

vector<int> moveZeroes(vector<int> nums) {
  int j = 0;
  for (int i = 0; i < (int) nums.size(); i++) {
    if (nums[i] != 0 && nums[j] == 0) {
      swap(nums[i], nums[j]);
    }
    if (nums[j] != 0) j++;
  }

  return nums;
}

int main() {
  // vector<int> v = moveZeroes({0}); // Output: {0}
  vector<int> v = moveZeroes({0,1,0,3,12}); // Output: [1,3,12,0,0]
  for(auto &i: v) cout << i << " ";

  return 0;
}
```


### 30. Valid Palindrome II
```c++
#include <bits/stdc++.h>
using namespace std;

bool isPal(string s) {
  int i = 0;
  int j = (int) s.size() - 1;

  while (i < j) {
    if (s[i] == s[j]) {
      i++; j--;
    } else {
      string a = s.substr(i, j - i);
      string b = s.substr(i + 1, j - i);

      string ra = a; reverse(ra.begin(), ra.end());
      string rb = b; reverse(rb.begin(), rb.end());

      return ra == a || rb == b;
    }
  }

  return true;
}

int main() {
  cout << isPal("aba") << endl;  // Output: true
  cout << isPal("abca") << endl; // Output: true
  cout << isPal("abc") << endl;  // Output: false
  return 0;
}
```
