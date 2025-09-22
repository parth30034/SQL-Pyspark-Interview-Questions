# 🐍 Python Problems – Day 1

Organized set of dictionary/hashmap-style problems solved in Python.  
Each problem is written as a function with test cases – interview ready.  

---

## 1. Word Frequency Counter
**Problem:** Count how many times each word appears in a list.  

```python
def word_frequency(words: list) -> dict:
    """
    Count frequency of words in a list.
    Example: ["data", "engineer", "data"] -> {'data': 2, 'engineer': 1}
    """
    res = {}
    for w in words:
        res[w] = res.get(w, 0) + 1
    return res

# ✅ Test
print(word_frequency(["data","engineer","data"]))
# Output: {'data': 2, 'engineer': 1}
```

---

## 2. Find Duplicates in a List
**Problem:** Return list of elements that appear more than once.  

```python
def find_duplicates(nums: list) -> list:
    """
    Find duplicate elements in a list.
    Example: [1,2,3,2,1] -> [1,2]
    """
    seen, duplicates = set(), set()
    for num in nums:
        if num in seen:
            duplicates.add(num)
        else:
            seen.add(num)
    return list(duplicates)

# ✅ Test
print(find_duplicates([1,2,3,1,4,5,2,5]))
# Output: [1, 2, 5]
```

---

## 3. Group Anagrams
**Problem:** Group words that are anagrams of each other.  

```python
def group_anagrams(words: list) -> list:
    """
    Group words that are anagrams.
    Example: ["eat","tea","tan"] -> [["eat","tea"],["tan"]]
    """
    groups = {}
    for w in words:
        key = tuple(sorted(w))
        groups.setdefault(key, []).append(w)
    return list(groups.values())

# ✅ Test
print(group_anagrams(["eat","tea","tan","ate","nat","bat"]))
# Output: [['eat','tea','ate'], ['tan','nat'], ['bat']]
```

---

## 4. Two-Sum with Dictionary
**Problem:** Return indices of two numbers that add up to target.  

```python
def two_sum(nums: list, target: int) -> tuple:
    """
    Return indices of two numbers that add up to target.
    Example: [2,7,11,15], target=9 -> (0,1)
    """
    num_map = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in num_map:
            return (num_map[complement], i)
        num_map[num] = i
    return ()

# ✅ Test
print(two_sum([2,7,11,15], 9))
# Output: (0,1)
```

---

## 5. Most Frequent Element
**Problem:** Find the element that appears most frequently in a list.  

```python
from collections import Counter

def most_frequent(nums: list) -> int:
    """
    Return the element that appears most frequently.
    Example: [1,2,2,3,3,3] -> 3
    """
    if not nums:
        raise ValueError("Input list cannot be empty.")
    return Counter(nums).most_common(1)[0][0]

# ✅ Test
print(most_frequent([4,1,2,2,3,4,4,5,2]))
# Output: 4
```

---

## 6. Mini Project – CSV Cleaning
**Problem:** Read a CSV, drop rows with nulls, print stats.  

```python
import pandas as pd

def clean_csv(path: str):
    """
    Load CSV, drop rows with null 'age',
    print row counts before/after,
    print unique cities,
    return cleaned DataFrame.
    """
    df = pd.read_csv(path)
    print(f"Rows before cleaning: {len(df)}")
    df_cleaned = df.dropna(subset=['age'])
    print(f"Rows after cleaning: {len(df_cleaned)}")
    print("Unique cities:", df_cleaned['city'].unique())
    return df_cleaned

# ✅ Test (use sample.csv)
# clean_csv("sample.csv")
```

---

📌 **Day 1 Wrap-up**  
- 5 core problems solved (dict/hashmap style).  
- 1 CSV mini-project.  
- Ready for GitHub push 🚀  
