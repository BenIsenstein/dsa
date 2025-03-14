# Hashing and Hash Maps

Hash maps are an incredible data structure that can use memory to speed up many important operations. They store key-value pairs, and can use almost any value as the key. They store values in an array under the hood. Via a hashing funcion, they turn keys into integer indexes deterministically - the **same key** becomes the **same index**, ***every single time***.

Operations like checking for existence, and deletion of an element take *O(1)* time.

Here's a look at the interface for using a hash map in a couple high-level programming languages:

## Python

```python
# Declaration: a hash map is declared like any other variable. The syntax is {}
hash_map = {}

# If you want to initialize it with some key value pairs, use the following syntax:
hash_map = {1: 2, 5: 3, 7: 2}

# Checking if a key exists: simply use the `in` keyword
1 in hash_map # True
9 in hash_map # False

# Accessing a value given a key: use square brackets, similar to an array.
hash_map[5] # 3

# Adding or updating a key: use square brackets, similar to an array.
# If the key already exists, the value will be updated
hash_map[5] = 6

# If the key doesn't exist yet, the key value pair will be inserted
hash_map[9] = 15

# Deleting a key: use the del keyword. Key must exist or you will get an error.
del hash_map[9]

# Get size
len(hash_map) # 3

# Get keys: use .keys(). You can iterate over this using a for loop.
keys = hash_map.keys()
for key in keys:
    print(key)

# Get values: use .values(). You can iterate over this using a for loop.
values = hash_map.values()
for val in values:
    print(val)
```

## JavaScript

```javascript
// Declaration: use the Map object
let hashMap = new Map();

// If you want to initialize it with some key value pairs, use the following syntax:
let hashMap = new Map([
    [1, 2],
    [5, 3],
    [7, 2]
]);

// Checking if a key exists: use .has()
hashMap.has(1); // true
hashMap.has(9); // false

// Accessing a value given a key: use .get()
hashMap.get(5); // 3

// Adding or updating a key: use .set()
// If the key already exists, the value will be updated
hashMap.set(5, 6);

// If the key doesn't exist yet, the key value pair will be inserted
hashMap.set(9, 15);

// Deleting a key: use .delete()
hashMap.delete(9);

// Get size
hashMap.size; // 3

// Iterate over the key value pairs: use the following code
for (const [key, value] of hashMap) {
    console.log(key + " " + value);
}
```

