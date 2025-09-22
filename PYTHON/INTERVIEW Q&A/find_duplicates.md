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
