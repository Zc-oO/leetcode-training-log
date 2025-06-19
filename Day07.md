# Algorithm Training Camp - Day 7

## Today's Problems

- [454. 4Sum II](https://leetcode.com/problems/4sum-ii/)
- [383. Ransom Note](https://leetcode.com/problems/ransom-note/)
- [15. 3Sum](https://leetcode.com/problems/3sum/)

---

## 1️⃣ 454. 4Sum II

**Question:** 
Given four integer arrays `nums1`, `nums2`, `nums3`, and `nums4` all of length n, return the number of tuples (i, j, k, l) such that:
```python
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0
```

**My idea:**    
- Pick one element from each array. The total sum should be 0.
- Brute force is O(n⁴), which is too slow.
- Use a hash map:
- Key: sum of a pair from nums1 + nums2
- Value: count of how many times that sum occurs
- Then for each pair from nums3 + nums4, check if -(c + d) exists in the map.
- If yes, add the occurrence count.

**Code Logic:**

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        mapAB = {}
        for a in nums1:
            for b in nums2:
                mapAB[a+b] = mapAB.get(a+b, 0) + 1
        count = 0
        for c in nums3:
            for d in nums4:
                key = -(c+d)
                if key in mapAB:
                    count += mapAB[key]
        return count
```
---

## 2️⃣ 383. Ransom Note

**Question:** 
Given two strings ransomNote and magazine, return True if ransomNote can be constructed by using the letters in magazine, and False otherwise.
Each letter in magazine can only be used once.

**My idea:**
- Store the count of each letter in magazine.
- Then for each letter in ransomNote, check if it exists and decrement from the count.
- If any count goes negative, return False.

**Code Logic:**

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        store = [0] * 26
        for i in magazine:
            store[ord(i) - ord("a")] += 1
        for i in ransomNote:
            if store[ord(i) - ord("a")] > 0:
                store[ord(i) - ord("a")] -= 1
            else:
                return False
        return True
```

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        for i in ransomNote:
            if i not in magazine:
                return False
            else:
                magazine = magazine.replace(i, '', 1)
        return True
```

---

## 3️⃣ 15. 3Sum

**Question:**  
Given an integer array nums, return all the unique triplets [nums[i], nums[j], nums[k]] such that:
```python
nums[i] + nums[j] + nums[k] == 0
```

**My idea:**  
- First sort the array.
- For each element nums[i], use two pointers (left, right) to find the remaining two elements.
- If the sum is too large, move right leftward.
- If the sum is too small, move left rightward.
- Skip duplicates for both i, left, and right.

**Code Logic:**

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()
        for i in range(len(nums)):
            # if the first num already > 0, no need to check
            if nums[0] > 0:
                return result
            # if several numbers are the same, skip
            if i > 0 and nums[i] == nums[i-1]:
                continue
            # define the left and right pointers
            left = i + 1
            right = len(nums) - 1
            while left < right:
                sum_ = nums[i] + nums[left] + nums[right]
                # move the left pointer rightward
                if sum_ < 0:
                    left += 1
                elif sum_ > 0:
                    right -= 1
                else:
                    result.append([nums[i], nums[left], nums[right]])
                    # remove the duplicate num when looping the left right pointers
                    while right > left and nums[right] == nums[right - 1]:
                        right -= 1
                    while right > left and nums[left] == nums[left + 1]:
                        left += 1
                        # finished check, move two pointers
                    left += 1
                    right -= 1
        return result
```

---

---

## 4️⃣ 18. 4Sum

**Question:**  
Given an array `nums` of n integers, return an array of all the unique quadruplets `[nums[a], nums[b], nums[c], nums[d]]` such that:
```python
nums[a] + nums[b] + nums[c] + nums[d] == target
```
Each of a, b, c, and d must be distinct.
Input: nums = [1, 0, -1, 0, -2, 2], target = 0  
Output: [[-2, -1, 1, 2], [-2, 0, 0, 2], [-1, 0, 0, 1]]

**My idea:**
- 4Sum is very similar to 3Sum.
- The only difference is we add one more outer loop to fix a second number.
- After sorting the array, we:
- Use a double loop to fix the first two numbers
- Then apply two-pointer logic to find the rest
- Use the same duplicate skipping technique as in 3Sum to avoid repeated results.
- Note on pruning (剪枝):
- We can add pruning to break early when:
```python
if nums[i] > target and nums[i] > 0 and target > 0:
    break
```
- Also:
```python
if nums[i] + nums[j] > target and target > 0:
    break
```
- BUT this only works when both nums and target are positive.
- If the list has both negative and positive numbers, premature pruning may skip valid results.

**Code Logic:**

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        result = []
        nums.sort()
        for i in range(len(nums)):
            # break only when the both nums are positive and the target is also positive
            # otherwise it might try to break too early without considering 
            # the full combination of negative and positive values.
            if nums[i] > target and nums[i] > 0 and target > 0:# 剪枝（可省）
                break
            if i > 0 and nums[i] == nums[i-1]:
                continue
            # the second for loop is just the same as the 3Sum
            for j in range(i+1, len(nums), 1):
                # break only when the both nums are positive and the target is also positive
                if nums[i] + nums[j] > target and target > 0: #剪枝（可省）
                    break
                if j > i+1 and nums[j] == nums[j-1]:
                    continue
                left = j + 1
                right = len(nums) - 1
                while left < right:
                    sum_ = nums[i] + nums[j] + nums[left] + nums[right]
                    if sum_ < target:
                        left += 1
                    elif sum_ > target:
                        right -= 1
                    else:
                        result.append([nums[i], nums[j], nums[left], nums[right]])
                        while left < right and nums[left] == nums[left+1]:
                            left += 1
                        while left < right and nums[right] == nums[right-1]:
                            right -= 1
                        left += 1
                        right -= 1
        return result
```