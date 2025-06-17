# Algorithm Training Camp - Day 1

## Today's Problems

- [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)
- [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)
- [202. Happy Number](https://leetcode.com/problems/happy-number/)
- [1. Two Sum](https://leetcode.com/problems/two-sum/)

---

## 1️⃣ Hash Table (基础知识整理)

- **Hash Table / Hash Function**
  - 快速判断一个元素是否在某个集合中。
  - Hash Function: 将数据用函数映射到固定的 hash table 位置。
  - **Hash Collision (碰撞)**: 两个不同的数据映射到相同的位置。
  - 常用处理方法：线性探测法（Linear Probing）。
- **Hash Table 常见实现:** Array, Set, Map

---

## 2️⃣ 242. Valid Anagram

**Question:** Given two strings s and t, return true if t is an anagram of s, false otherwise.

**My idea:**
- Use a hash array (len 26) to store the occurrences of each letter in s.
- For t, subtract occurrences.
- Finally, check if the array is all 0, if not, they are not anagrams.

**Code Logic:**

```python
record = [0] * 26

for i in s:
    record[ord(i) - ord("a")] += 1

for i in t:
    record[ord(i) - ord("a")] -= 1

for i in range(26):
    if record[i] != 0:
        return False

return True
```

---

## 3️⃣ 349. Intersection of Two Arrays

**Question:**  
Given two integer arrays `nums1` and `nums2`, return their intersection. Each element in the result must be unique and you may return the result in any order.

**My idea:**  
- Since we only need distinct elements, we can directly use set intersection:  
  `list(set(nums1) & set(nums2))`
- Alternatively, we can use a dictionary to record elements in `nums1`, and check whether elements from `nums2` exist in the dictionary.

**Code Logic (using dict + set):**

```python
store = {}

for num in nums1:
    store[num] = store.get(num, 0) + 1

res = set()

for num in nums2:
    if num in store:
        res.add(num)

return list(res)
```

---

## 4️⃣ 202. Happy Number

**Question:**  
Write an algorithm to determine if a number `n` is a happy number.  
A happy number is a number defined by the following process:
- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
- Return true if `n` is a happy number, and false if not.

**My idea:**  
- Use a set to record all numbers we have seen before.
- If the loop ever revisits a number, it means we fall into an infinite cycle, return False.
- If we eventually reach 1, return True.

**Code Logic:**

```python
exist = set()

while n not in exist:
    exist.add(n)
    new_num = 0
    n_str = str(n)

    for i in n_str:
        new_num += int(i) ** 2

    if new_num == 1:
        return True
    else:
        n = new_num

return False
```

---

## 5️⃣ 1. Two Sum

**Question:**  
Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.  
You may assume that each input would have exactly one solution, and you may not use the same element twice.

**My idea:**  
- Use a dictionary to store the number and its corresponding index.
- For each number, calculate its complement (`target - val`) and check whether it exists in the dictionary.
- If yes, return both indices.

**Code Logic:**

```python
records = dict()

for idx, val in enumerate(nums):
    if target - val in records:
        return [records[target - val], idx]
    records[val] = idx

return []
```


