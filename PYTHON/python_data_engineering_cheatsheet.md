🐍 PYTHON FOR DATA ENGINEERS — EXTENDED CHEATSHEET
🎯 GOAL

Demonstrate mastery of clean, functional, and production-grade Python in data pipelines, ETL scripts, and orchestration tasks.

🧠 1. Data Structures & Comprehensions
✅ Lists, Sets, Dicts
nums = [1, 2, 3, 4, 5]
unique = set(nums)
squares = {x: x*x for x in nums}

✅ Comprehensions
# List comprehension
[x*x for x in nums if x % 2 == 0]

# Dict comprehension
{name: len(name) for name in ["Alice", "Bob"]}

# Set comprehension
{len(word) for word in ["apple", "banana", "pear"]}

⚙️ 2. Functional Programming
🔹 map / filter / reduce
from functools import reduce

nums = [1, 2, 3, 4]
squares = list(map(lambda x: x**2, nums))
evens = list(filter(lambda x: x % 2 == 0, nums))
product = reduce(lambda x, y: x*y, nums)

🔹 enumerate / zip / defaultdict
from collections import defaultdict

for idx, val in enumerate(nums): ...
for a, b in zip(['A', 'B'], [1, 2]): ...
d = defaultdict(list)

🧱 3. Clean Code Practices
🔹 Modular Functions + Type Hints
def transform_data(records: list[dict]) -> list[dict]:
    return [r for r in records if r.get("active")]

🔹 Main Guard
if __name__ == "__main__":
    main()

🔹 Docstrings
def clean_text(text: str) -> str:
    """Removes extra spaces and lowercases the text."""
    return text.strip().lower()

🧰 4. Error Handling & Logging
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

try:
    process_data()
except Exception as e:
    logger.error(f"Error while processing: {e}", exc_info=True)

🧩 5. File & JSON Handling
🔹 Reading Files
with open("data.txt") as f:
    lines = [line.strip() for line in f]

🔹 JSON
import json
data = json.load(open("file.json"))
json.dump(data, open("out.json", "w"), indent=4)

🧮 6. Itertools, Collections & Useful Utilities
from itertools import groupby, chain
from collections import Counter, namedtuple

nums = [1,2,2,3,3,3]
counts = Counter(nums)
# Counter({3: 3, 2: 2, 1: 1})

Person = namedtuple("Person", "name age")
p = Person("Alice", 25)

🧮 7. Performance Tips
Technique	Example	Why
Generator expressions	(x*x for x in range(100000))	Saves memory
Lazy evaluation	yield	Stream processing
Built-ins over loops	sum(), any(), all()	Fast C implementations
Comprehensions > map/filter	Readable, optimized	
🧠 8. OOP & Reusability Basics
class DataCleaner:
    def __init__(self, rules: dict):
        self.rules = rules

    def clean(self, text: str) -> str:
        for old, new in self.rules.items():
            text = text.replace(old, new)
        return text

🌩️ 9. CLI & Environment Integration
import os, sys
from dotenv import load_dotenv

load_dotenv()
db_url = os.getenv("DB_URL")

if len(sys.argv) > 1:
    print(f"Input argument: {sys.argv[1]}")

🧩 10. Pandas Essentials (Quick Data Ops)
import pandas as pd

df = pd.read_csv("data.csv")
df = df.drop_duplicates().fillna(0)
df["amount_usd"] = df["amount"] * 82.5

🧩 11. Testing & Validation
import pytest

def test_sum():
    assert sum([1,2,3]) == 6

🧩 12. Python ↔ Spark Parallels
Python	PySpark	Concept
map()	rdd.map()	Transform
filter()	rdd.filter()	Lazy filtering
reduce()	rdd.reduce()	Action trigger
list comprehension	selectExpr	Declarative
dict	DataFrame row	Record representation
🧭 13. Interview Soundbite

“I use Python for data transformation, validation, and orchestration tasks. My focus is on modular, readable, and production-grade code — using comprehensions, type hints, and logging. I occasionally apply functional constructs like map/filter/reduce when the transformation logic is stateless and Spark-like.”

✅ Topics Covered:

✔️ Data structures
✔️ Functional programming
✔️ Clean code
✔️ Logging & error handling
✔️ JSON / file I/O
✔️ OOP
✔️ Environment handling
✔️ Testing
✔️ Python–Spark parallels