# Q001 — Find Missing Numbers in Sequence

### 📖 Context
You are given a list of integers ranging from 1 to N with some numbers missing.  
You need to identify the missing numbers.

---

### ❓ Question
Write a Python function to find missing numbers from a given list.

---

### 📊 Input Example
```python
nums = [1, 2, 4, 6, 7, 9, 10]
N = 10
```

---

### ✅ Expected Output
```python
[3, 5, 8]
```

---

### 🗝️ Python Solution
```python
def find_missing(nums, N):
    return [i for i in range(1, N+1) if i not in nums]

nums = [1, 2, 4, 6, 7, 9, 10]
N = 10
print(find_missing(nums, N))  # [3, 5, 8]
```
