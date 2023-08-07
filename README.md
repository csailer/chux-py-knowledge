# chux-py-knowledge
Just fun facts about Python

1. **What is the difference between a list and a tuple in Python?**
    The primary difference between tuples and lists is that **tuples are immutable as opposed to lists which are mutable**.
    Therefore, it is possible to change  a list but not a tuple. The contents of a tuple cannot change once they have been
    created in Python due to the immutability of tuples.
2. How does Python's garbage collection work? What is the role of reference counting in it?
   Python garbage collection algorithm is very useful to open up space in the memory. Garbage collection is implemented in Python in two ways:
   reference counting and generational.  
   **When the reference count of an object reaches 0, reference counting garbage collection algorithm cleans up the object immediately**.
3. Can you explain how list comprehensions are beneficial and provide an example?
   List comprehensions in Python provide a concise and readable way to create lists. They can often replace multi-line loops with a single line of code.

    Benefits of List Comprehensions:

    - Conciseness: You can generate lists with less code, making your script or program shorter and more maintainable.
    - Readability: Once you're familiar with them, list comprehensions can be more readable than equivalent loops because their intent is clear: constructing                     a list.
    - Performance: List comprehensions are often faster than equivalent for loops, mainly because they are optimized in the Python interpreter.

    **Example:**

     Suppose you want to generate a list of squares for numbers from 0 to 9.
     Using a traditional loop:

     ```python
        squares = []
        for i in range(10):
          squares.append(i**2)
     ```
     **Using a list comprehension:**
     ```python
        squares = [i**2 for i in range(10)]
     ```
     Both approaches give you the same result, [0, 1, 4, 9, 16, 25, 36, 49, 64, 81], but the list comprehension does
     so in a more concise and readable manner. Furthermore, list comprehensions can also incorporate conditions.

   **Using a list comprehension:** 
   ```python
     even_squares = [i**2 for i in range(10) if i % 2 == 0]    
   ```
4. Describe the GIL (Global Interpreter Lock) and its implications in multi-threaded Python applications.
   
The Global Interpreter Lock, or GIL, is a mutex (a synchronization primitive) that protects access to Python objects, preventing multiple native threads  from executing Python bytecodes concurrently in a single process. The GIL exists mainly because CPython (Python's primary implementation) is not thread-safe at the level of its memory management.

**Implications of the GIL in multi-threaded Python applications:**

1. **Single-threaded Execution**: Despite the presence of multiple threads in a multi-threaded application, only one thread can execute Python bytecode at a time because of the GIL. This makes pure-Python programs using threading unable to fully utilize the capabilities of modern multi-core processors.

2. **CPU-bound Programs**: In CPU-bound Python programs (those that do intensive calculations rather than waiting for I/O), the use of threading can actually result in a performance hit. This is because threads frequently have to stop and acquire or release the GIL, incurring overhead.

3. **I/O-bound Programs**: For I/O-bound programs (those that spend time waiting for input/output from external resources, like disk, network, etc.), Python threads can still be effective. When one thread is waiting for I/O, it can release the GIL so another thread can run.

4. **Extensions**: It's possible to bypass the GIL when writing extensions in C or C++. If an extension doesn't need to interact with Python objects for a while, it can release the GIL, allowing other threads to run.

5. **Alternative Implementations**: Some other implementations of Python, such as Jython (for the JVM) and IronPython (for .NET), do not have a GIL and therefore do not suffer from its multi-threading limitations. PyPy, an alternative Python implementation, has a GIL in its default configuration, but its software transactional memory (STM) version aims to get rid of it.

6. **Concurrency vs. Parallelism**: While the GIL affects parallelism (the simultaneous execution of multiple threads), it does not impact concurrency (the ability to deal with multiple tasks). For this reason, Python provides other concurrency mechanisms, like multiprocessing, asyncio, and concurrent.futures, which allow for efficient concurrent execution without the constraints of the GIL.

7. **Complexity in Developing Extensions**: For developers writing C extensions for Python, the GIL adds complexity. They need to carefully manage the GIL when their extension calls back into Python code or when working with Python objects to ensure thread safety.

In summary, while the GIL does limit the parallel execution capabilities of multi-threaded Python programs, it's essential to note that not all applications need multi-threaded parallelism. In cases where it's essential, developers often use multi-process architectures (via Python's `multiprocessing` module) or other Python implementations to achieve true parallelism.

5. What is the difference between deepcopy and copy in Python?
   In Python, both `copy` and `deepcopy` are methods provided by the `copy` module to create duplicates of objects. However, they differ in how they handle compound objects (objects that contain other objects, like lists or class instances).

1. **Shallow Copy (`copy`):**
   - A shallow copy creates a new compound object and then inserts references into it to the original objects.
   - This means that while the compound object itself is distinct, its contents are still references to the same objects as the contents of the original.
   - In essence, a shallow copy is one level deep. The "outer" container is new, but the contents are not.

2. **Deep Copy (`deepcopy`):**
   - A deep copy creates a new compound object, and then, recursively, inserts copies of the original objects into it.
   - This creates a true "copy" of the original object and all its contents, ensuring that none of the objects (or their objects, and so forth) are shared between the original and its copy.
   - A deep copy is "fully" independent of the original object.

**Example:**

Consider a list of lists:

```python
import copy

original = [[1, 2, 3], [4, 5, 6]]

shallow_copied = copy.copy(original)
deep_copied = copy.deepcopy(original)
```

If you modify an inner list of `original`:

```python
original[0][0] = 99
```

The change will be reflected in `shallow_copied` but not in `deep_copied`:

```python
print(shallow_copied)  # Outputs: [[99, 2, 3], [4, 5, 6]]
print(deep_copied)    # Outputs: [[1, 2, 3], [4, 5, 6]]
```

In the case of the shallow copy, the outer list is different, but the inner lists are shared/referenced between `original` and `shallow_copied`. On the other hand, with the deep copy, everything is independent.

It's important to choose the appropriate type of copy based on the requirements of your application. If you only need a duplicate of the outer object, a shallow copy might suffice. However, if you need a completely independent object that shares no references with the original, a deep copy is the way to go.

# Pytest Specific Questions:

1. Explain the main advantages of using Pytest over other testing frameworks like unittest.
   `Pytest` is a popular testing framework in the Python community, known for its user-friendly and flexible approach to writing tests. While both `unittest` (a standard library module in Python) and `pytest` serve the purpose of testing, there are several advantages of using `pytest` over `unittest`:

1. **Concise Syntax**: 
    - `Pytest` allows you to write tests using plain `assert` statements without needing to remember the various `self.assert*` methods that `unittest` requires. This makes the test cases more readable.
    - For example, instead of `self.assertEqual(a, b)`, you can simply write `assert a == b`.

2. **Powerful and Flexible Fixture System**:
    - Fixtures in `pytest` replace the traditional setup and teardown methods. They are more powerful and can be modular and reused across different test functions, modules, or even projects.
    - They can also use dependency injection, where the test functions can directly accept fixtures as arguments.

3. **Parameterized Testing**:
    - With `pytest`, you can easily parameterize a single test function, allowing it to run with different arguments, reducing repetition in your tests.
    - This is done using the `@pytest.mark.parametrize` decorator.

4. **Auto-discovery of Test Cases**:
    - `Pytest` automatically recognizes test cases without needing a specific class or method naming convention (though sticking to the convention `test_*` is recommended). 
    - You don't need to write boilerplate code like `if __name__ == '__main__': unittest.main()`.

5. **Richer Command Line Options**:
    - `Pytest` offers various command-line options, such as running tests that failed in the last run (`--lf`), running tests in a random order, or generating coverage reports.

6. **Enhanced Reporting**:
    - `Pytest` provides more detailed feedback on test failures compared to `unittest`. This includes a clear distinction of each assertion in a test function and shows local variables in tracebacks.

7. **Built-in Parallel Execution**:
    - Using plugins like `pytest-xdist`, you can run tests in parallel, distributing test execution across multiple CPUs or even remote machines.

8. **Extensibility**:
    - `Pytest` supports a vast ecosystem of plugins, and you can also easily write your own.

9. **Compatibility with unittest**:
    - `Pytest` can run tests written using the `unittest` framework out-of-the-box, which means transitioning to `pytest` doesn't necessitate rewriting existing tests.

10. **Advanced Filtering**:
    - It's straightforward to run a subset of tests, such as specific modules, specific test functions, or tests that match a particular keyword expression.

11. **Custom Markers**:
    - You can define your markers (e.g., `@pytest.mark.smoke`) to categorize tests and use them to run a specific subset of tests.

Despite these advantages, the choice between `pytest` and `unittest` (or other testing frameworks) depends on the specific needs of a project and the preferences of its developers.

2. How do you mark a test function as expected to fail in Pytest?
   In `pytest`, if you expect a test function to fail, you can mark it with the `xfail` marker. This allows you to indicate that a particular test is known to fail, and it shouldn't be considered a surprise or a complete test failure if it does fail.

Here's how to mark a test as expected to fail:

```python
import pytest

@pytest.mark.xfail
def test_function():
    assert 1 == 2
```

When this test is run, `pytest` will indicate that the test was expected to fail, and if it does fail, it won't be counted as a normal failure. 

Additionally, if a test marked with `xfail` actually passes, `pytest` will report it as "unexpectedly passing" or "xpassed."

You can also provide a reason for the expected failure:

```python
@pytest.mark.xfail(reason="This test is known to fail with the current implementation.")
def test_function():
    assert 1 == 2
```

If you want to see the reasons in the test report, you can use the `-r` flag followed by the characters representing the results you are interested in. For instance, using `-rx` would show the reasons for all xfail tests.

Moreover, `xfail` can be made conditional:

```python
@pytest.mark.xfail(condition=sys.version_info < (3, 8), reason="Fails on Python versions below 3.8")
def test_function():
    ...
```

Here, the test would be expected to fail only for Python versions below 3.8. If the condition is `True`, the test is expected to fail; otherwise, it's expected to pass.

3. Describe how fixtures in Pytest are different from setup and teardown in unittest.
   `pytest` fixtures and `unittest`'s setup/teardown methods are both mechanisms to set up preconditions and clean up after tests. However, they differ in their implementation, flexibility, and capabilities.

1. **Implementation and Syntax**:
    - **unittest**:
        - Uses traditional object-oriented setup and teardown methods.
        - A typical `unittest` test class might have `setUp()` and `tearDown()` methods that run before and after each test method, respectively. There are also `setUpClass()` and `tearDownClass()` methods that run before and after all test methods in the class.
    - **pytest**:
        - Uses a declarative approach with the `@pytest.fixture` decorator.
        - Instead of class methods, you define standalone functions decorated with `@pytest.fixture`.

2. **Scope and Granularity**:
    - **unittest**:
        - Offers two primary scopes: "method" (with `setUp` and `tearDown`) and "class" (with `setUpClass` and `tearDownClass`).
    - **pytest**:
        - Provides more scopes: "function" (default), "class", "module", "package", and "session".
        - This flexibility allows setup and teardown operations at various levels, from individual tests to entire test sessions.

3. **Dependency Injection**:
    - **unittest**:
        - `setUp` and `tearDown` methods are implicitly called for tests in the same class, and state is usually managed via instance variables.
    - **pytest**:
        - Uses a form of dependency injection. Test functions that need a fixture explicitly accept it as a parameter. This approach makes dependencies more explicit and provides better modularity.

4. **Fixture Reusability and Modularity**:
    - **unittest**:
        - Setup and teardown methods are tied to the class they're defined in.
    - **pytest**:
        - Fixtures can be defined in separate modules and reused across multiple test modules or even different projects. This is particularly useful for commonly used setup code.

5. **Fixture Composition**:
    - **pytest** allows fixtures to use other fixtures, making it easy to set up layered or hierarchical preconditions.

6. **Parameterized Fixtures**:
    - **pytest**:
        - Supports parameterized fixtures, allowing them to run tests multiple times with different arguments. This feature can replace the loop-based tests in `unittest` with a more declarative style.

7. **Finalization**:
    - **pytest**:
        - Allows using a `yield` statement in fixtures. Everything after the `yield` acts like a teardown phase, ensuring resources are finalized or cleaned up, even if a test fails.

8. **Explicit Naming**:
    - **pytest**:
        - Fixtures can be given descriptive names, making test functions more readable as the setup and cleanup steps are explicitly named in function parameters.

In summary, while `unittest`'s setup and teardown methods provide a basic way to manage test preconditions and cleanup, `pytest` fixtures offer a more flexible, modular, and declarative approach. The increased granularity, explicitness, and reusability of `pytest` fixtures make them a powerful tool, especially for larger or more complex test suites.

4. What is the purpose of the pytest.mark.parametrize decorator?
   
   
