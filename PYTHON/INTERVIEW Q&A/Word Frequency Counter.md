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
