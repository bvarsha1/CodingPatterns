The Java `TreeMap` class provides numerous methods for working with sorted key-value pairs. Key functions can be categorized into **basic operations** inherited from the `Map` interface and **navigation operations** from the `NavigableMap` interface. 

### Basic Map Operations

Common `Map` methods available in `TreeMap` include:

- **`put(K key, V value)`**: Adds or updates a key-value mapping.
- **`get(Object key)`**: Retrieves the value associated with a key.
- **`remove(Object key)`**: Deletes a key-value mapping.
- **`containsKey(Object key)`**: Checks if a key is present.
- **`containsValue(Object value)`**: Checks if a value is present.
- **`size()`**: Returns the number of entries.
- **`clear()`**: Removes all entries.
- **`keySet()`**: Provides a sorted `Set` of all keys.
- **`values()`**: Provides a `Collection` of all values, ordered by keys.
- **`entrySet()`**: Provides a sorted `Set` of key-value entries. 


### Navigation Operations

These methods take advantage of the sorted keys for efficient access (O(log n)): 

- **`firstKey()` / `lastKey()`**: Get the lowest/highest key.
- **`firstEntry()` / `lastEntry()`**: Get the entry with the lowest/highest key.
- **`pollFirstEntry()` / `pollLastEntry()`**: Get and remove the entry with the lowest/highest key.
- **`lowerKey(K key)` / `lowerEntry(K key)`**: Get the key/entry strictly less than the given key.
- **`floorKey(K key)` / `floorEntry(K key)`**: Get the key/entry less than or equal to the given key.
- **`ceilingKey(K key)` / `ceilingEntry(K key)`**: Get the key/entry greater than or equal to the given key.
- **`higherKey(K key)` / `higherEntry(K key)`**: Get the key/entry strictly greater than the given key.
- Methods like `headMap()`, `tailMap()`, and `subMap()` provide views of specific ranges within the map. 


### HeadMap, TailMap, SubMap

These methods leverage the sorted nature of `TreeMap` to return a _view_ (a subset) of the original map whose keys fall within a specified range. These views are **backed** by the original `TreeMap`, meaning that changes made to the view are reflected in the original map, and vice versa.

The methods have overloaded versions that accept an optional `boolean` argument, `inclusive`, to control whether the specified start/end boundary keys are part of the returned view.

Here is a breakdown of how they work:

`headMap()`

This method provides a view of all entries whose keys are _less than_ (or less than or equal to) a specified `toKey`.

|Method Signature|Description|
|---|---|
|`headMap(K toKey)`|Returns a view of the portion of the map whose keys are strictly less than `toKey`.|
|`headMap(K toKey, boolean inclusive)`|If `inclusive` is `true`, the view includes entries with a key _equal to_ `toKey`; otherwise, it behaves as the single-argument version.|

**Example:**  
If a `TreeMap` has keys `[1, 2, 3, 4, 5, 6]`:

- `map.headMap(4)` returns a view with keys `[1, 2, 3]`.
- `map.headMap(4, true)` returns a view with keys `[1, 2, 3, 4]`.

`tailMap()`

This method provides a view of all entries whose keys are _greater than_ (or greater than or equal to) a specified `fromKey`.

|Method Signature|Description|
|---|---|
|`tailMap(K fromKey)`|Returns a view of the portion of the map whose keys are greater than or equal to `fromKey`.|
|`tailMap(K fromKey, boolean inclusive)`|If `inclusive` is `false`, the view excludes entries with a key _equal to_ `fromKey`; otherwise, it behaves as the single-argument version.|

**Example:**  
If a `TreeMap` has keys `[1, 2, 3, 4, 5, 6]`:

- `map.tailMap(4)` returns a view with keys `[4, 5, 6]`.
- `map.tailMap(4, false)` returns a view with keys `[5, 6]`.

`subMap()`

This is the most comprehensive method, providing a view of entries within an arbitrary range, defined by both a starting `fromKey` and an ending `toKey`.

|Method Signature|Description|
|---|---|
|`subMap(K fromKey, K toKey)`|Returns a view of the portion of the map whose keys range from `fromKey`, inclusive, to `toKey`, exclusive.|
|`subMap(K fromKey, boolean fromInclusive, K toKey, boolean toInclusive)`|Provides full control over whether both boundary keys are inclusive or exclusive.|

**Example:**  
If a `TreeMap` has keys `[1, 2, 3, 4, 5, 6]`:

- `map.subMap(2, 5)` returns a view with keys `[2, 3, 4]` (2 is included, 5 is excluded by default).
- `map.subMap(2, true, 5, true)` returns a view with keys `[2, 3, 4, 5]`.
- `map.subMap(2, false, 5, false)` returns a view with keys `[3, 4]`.

Key Characteristics of These Views:

- **Live Views:** The returned object is not a static copy. If you add an element to the main `TreeMap` that falls within the `subMap`'s range, it immediately appears in the `subMap` view. If you add an element via the view, it appears in the original map.
- **Sorted Order:** The views maintain the same sorted order as the original `TreeMap`.
- **`SortedMap` / `NavigableMap` Types:** These methods return instances of `SortedMap` or `NavigableMap`, allowing you to use further map operations on the limited range of data.