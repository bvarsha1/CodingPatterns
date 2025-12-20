In Java, `StringBuffer` is a mutable, thread-safe sequence of characters. While similar to `StringBuilder`, its methods are synchronized, making it suitable for multi-threaded environments, though slightly slower in single-threaded scenarios. 

**1. Initializers (Constructors)**

`StringBuffer` defines four primary constructors: 

- **`StringBuffer()`**: Creates an empty buffer with a default initial capacity of **16 characters**.
- **`StringBuffer(int capacity)`**: Initializes an empty buffer with a specifically defined capacity.
- **`StringBuffer(String str)`**: Creates a buffer containing the specified string. Its initial capacity is the string's length plus 16.
- **`StringBuffer(CharSequence cs)`**: Initializes the buffer with the content of any character sequence, such as a `String` or `StringBuilder`. 

**2. Core Manipulation Methods**

These are critical for coding interviews to modify strings in place: 

- **`append(value)`**: Adds the string representation of various types (int, boolean, String, etc.) to the end.
- **`insert(int offset, value)`**: Inserts data at the specified index.
- **`delete(int start, int end)`**: Removes a substring from the `start` index to `end-1`.
- **`deleteCharAt(int index)`**: Deletes the character at a specific position.
- **`replace(int start, int end, String str)`**: Replaces a specified range with a new string.
- **`reverse()`**: Reverses the character sequence (frequent interview task for palindromes). 

**3. Capacity & Length Methods**

- **`length()`**: Returns the actual number of characters currently in the buffer.
- **`capacity()`**: Returns the total storage available before the buffer must resize.
- **`ensureCapacity(int min)`**: Ensures capacity is at least the specified minimum; if resizing is needed, it typically follows the formula: `(oldCapacity * 2) + 2`.
- **`setLength(int newLength)`**: Sets the buffer length; if reduced, characters are lost; if increased, null characters are appended.
- **`trimToSize()`**: Reduces capacity to match the current length. 

**4. Access & Search Methods**

- **`charAt(int index)`**: Returns the character at a specific index.
- **`setCharAt(int index, char ch)`**: Modifies the character at a specific index.
- **`indexOf(String str)` / `lastIndexOf(String str)`**: Returns the first or last occurrence of a substring.
- **`substring(int start, int end)`**: Returns a standard `String` from the specified range (does not modify the original buffer).
- **`toString()`**: Converts the `StringBuffer` object into a standard immutable `String`. 

**5. Interview Tips**

- **Mutability**: Emphasize that `StringBuffer` does not create new objects for every modification, unlike `String`.
- **Synchronization**: Be ready to explain that `StringBuffer` is **thread-safe** because its methods are `synchronized`, whereas `StringBuilder` is not.
- **New in Java 21**: A `repeat(int count)` method was added to both `StringBuffer` and `StringBuilder` to simplify repeating a character sequence.