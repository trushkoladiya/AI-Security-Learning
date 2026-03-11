# Memory Basics

## Overview

Memory is the component of a computer that temporarily stores
information while programs are running. When a program executes, its
instructions, variables, and data structures are loaded into memory so
the processor can access them quickly.

A useful analogy: - **Storage (Hard Drive / SSD)** → Like a filing
cabinet for long-term storage - **Memory (RAM)** → Like a desk where you
place documents you are actively working with

Memory is **temporary but fast**, while storage is **permanent but
slower**.

------------------------------------------------------------------------

## Key Concepts

### Memory in Program Execution

When a program runs: - The program code is loaded into memory -
Variables and data structures are stored in memory - When the program
finishes, that memory is cleared

### Stack Memory

-   Stores **temporary data**
-   Used for **function calls and local variables**
-   Operates in **Last-In-First-Out (LIFO)** order
-   **Automatically managed**
-   **Very fast but limited in size**

### Heap Memory

-   Stores **larger and longer-lived data**
-   Used for **objects and dynamic data structures**
-   **Manually managed or garbage collected**
-   **Slower than stack but much larger**

### Mutable vs Immutable Objects

**Immutable objects** - Cannot be modified after creation - Examples:
integers, floats, strings, tuples - Modifying them creates **new objects
in memory**

**Mutable objects** - Can be modified in place - Examples: lists,
dictionaries, sets - Changes affect the **same object in memory**

### Value vs Reference Behavior

Some variables store **values directly**, while others store
**references to objects in heap memory**.

------------------------------------------------------------------------

## Detailed Explanation

### Stack Memory Behavior

Stack memory handles short-lived variables created during function
calls.

Example:

``` python
def calculate_sum(a, b):
    result = a + b
    return result

total = calculate_sum(5, 10)
```

What happens:

1.  When `calculate_sum()` is called, stack memory is allocated for `a`,
    `b`, and `result`.
2.  The calculation occurs.
3.  When the function returns, the stack frame is removed and those
    variables are cleared.
4.  The variable `total` is stored in the caller's stack space.

Key characteristics: - Fast allocation and removal - Automatically
cleaned after function execution

------------------------------------------------------------------------

### Heap Memory Behavior

Heap memory stores dynamic objects that may live longer than a single
function call.

Example:

``` python
large_list = [1, 2, 3, 4, 5]
model_data = {"weights": [0.5, 0.3], "bias": 0.1}
```

What happens:

1.  Python allocates space in **heap memory** for these objects.
2.  Variables (`large_list`, `model_data`) hold **references to the
    objects in heap memory**.
3.  When objects are no longer referenced, Python's **garbage collector
    eventually frees the memory**.

Heap is used for: - Lists - Dictionaries - Large datasets - Machine
learning models

------------------------------------------------------------------------

### Value vs Reference Example

Value behavior:

``` python
x = 10
y = x

x = 20
print(y)
```

Output:

    10

Explanation: - `y` received a copy of the value stored in `x`. -
Changing `x` later does not affect `y`.

Reference behavior:

``` python
list1 = [1, 2, 3]
list2 = list1

list1.append(4)
print(list2)
```

Output:

    [1, 2, 3, 4]

Explanation: - Both variables reference the **same list in heap
memory**. - Modifying one variable affects the same underlying object.

------------------------------------------------------------------------

## Examples

### Machine Learning Training Example

``` python
training_images = load_images()
model = create_neural_network()

for epoch in range(100):
    loss = calculate_loss(model, training_images)
    update_model(model, loss)
```

Memory behavior:

-   `training_images` → stored in heap (large dataset)
-   `model` → stored in heap (complex object)
-   `loss` → temporary variable stored in stack
-   `loss` is recreated each iteration and removed afterward

If the dataset exceeds available memory, the program will crash.

------------------------------------------------------------------------

### Sensitive Data in Memory

Bad practice:

``` python
user_password = input("Enter password: ")
authenticate(user_password)
```

Issue: - Password remains in memory after use.

Better practice:

``` python
user_password = input("Enter password: ")
authenticate(user_password)
user_password = None
```

Setting the variable to `None` signals that the sensitive data should no
longer be retained.

------------------------------------------------------------------------

### Memory Leak Example

Bad example:

``` python
all_data = []
while True:
    new_data = load_large_dataset()
    all_data.append(new_data)
```

Problem: - Heap memory keeps growing - Program eventually crashes due to
memory exhaustion

Improved version:

``` python
while True:
    new_data = load_large_dataset()
    process(new_data)
```

After processing, the data can be removed from memory.

------------------------------------------------------------------------

## Real-World Relevance

### AI / Machine Learning Engineering

Memory understanding helps with:

-   Training large models
-   Handling large datasets
-   Preventing program crashes from memory exhaustion
-   Improving program efficiency

Examples: - Model weights stored in heap memory - Training batches
loaded temporarily into memory - Efficient memory usage speeds up
training

### AI Security Engineering

Memory knowledge is critical for identifying vulnerabilities:

Common risks: - **Buffer overflows** - **Memory leaks** - **Sensitive
data exposure in memory** - **Model extraction attacks**

Sensitive information like: - passwords - API keys - encryption keys

may reside in memory during execution and must be protected.

------------------------------------------------------------------------

## Important Points

-   Memory stores program data **while the program is running**.
-   RAM is **temporary but fast**, storage is **permanent but slower**.
-   Stack memory stores **short-lived function variables**.
-   Heap memory stores **dynamic objects and large data structures**.
-   Lists, dictionaries, and ML models live in **heap memory**.
-   Mutable objects can change in place; immutable objects cannot.
-   Large AI datasets and models must fit into available memory.
-   Poor memory management can lead to crashes or security
    vulnerabilities.

------------------------------------------------------------------------

## Common Beginner Mistakes

### Confusing References with Copies

``` python
original_list = [1, 2, 3]
backup_list = original_list
```

Both variables reference the same list.

Correct approach:

``` python
backup_list = original_list.copy()
```

------------------------------------------------------------------------

### Accidentally Modifying Data Through Functions

``` python
def modify_data(data):
    data.append(99)
```

This modifies the original list because the reference is passed.

------------------------------------------------------------------------

### Loading Entire Datasets Into Memory

Bad approach:

``` python
all_data = load_entire_dataset()
```

Better approach:

``` python
for batch in load_data_in_batches():
    process(batch)
```

Processing data in batches prevents memory overload.

------------------------------------------------------------------------

### Leaving Sensitive Data in Memory

``` python
password = "secret123"
check_password(password)
```

Better practice:

``` python
password = get_password()
check_password(password)
password = None
```
