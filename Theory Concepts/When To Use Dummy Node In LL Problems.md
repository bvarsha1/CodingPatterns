## 🧠 When do you need a *dummy node* in Linked List problems?

A **dummy node (sentinel node)** is an extra node placed **before the head**, pointing to it.

```text
dummy -> head -> ...
```

You usually need a dummy node when **the head itself might change** during the operation.

---

## 🚩 Key Signal: “Head might be modified”

Ask yourself this **one question**:

> ❓ *Is there any chance the head node could be deleted, reversed, or replaced?*

If the answer is **YES** → **Use a dummy node**.

---

## 🔍 Common Situations Where Dummy Node Is Needed

### 1️⃣ Operations that involve positions (index-based)

Examples:

* Reverse Linked List II (`left = 1`)
* Remove Nth node from end
* Insert / delete at position `1`

👉 Problem:
If `left = 1`, there is **no node before head**.

👉 Dummy solves this:

```text
dummy -> head
```

Now `dummy` safely acts as “node before head”.

✅ **Rule:**
If a problem talks about **positions (1-based indexing)** → strongly consider dummy.

---

### 2️⃣ When deleting nodes (especially including head)

Examples:

* Remove elements with a given value
* Remove duplicates
* Remove Nth node from end

❌ Without dummy:

```java
if (head.val == x) head = head.next; // special case
```

✅ With dummy:

```java
dummy.next = head;
prev.next = curr.next;
```

No special cases needed.

📌 **Rule:**
If deletion logic could target the head → use dummy.

---

### 3️⃣ Partial reversal problems

Examples:

* Reverse Linked List II
* Reverse Nodes in k-Group

Why?

* The **first node of the reversed segment** might be the head
* You need a **stable node before the segment**

Dummy provides:

```text
dummy -> [before reversal] -> [reversed segment]
```

📌 **Rule:**
If reversing part of a list → dummy simplifies reconnection.

---

### 4️⃣ When reconnecting multiple segments

Examples:

* k-group reversal
* Odd-even list
* Merge operations

Dummy acts as:

* A fixed anchor
* A guaranteed “previous node”

📌 **Rule:**
If you're reconnecting chunks of nodes → dummy avoids null checks.

---

## 🧩 Mental Model (Very Important)

Think of dummy as:

> 🪝 **A safety hook before the head**

It ensures:

* Every node has a “previous”
* No `if (head == ...)` special handling
* Clean, uniform pointer logic

---

## ✅ Decision Checklist (Use this before coding)

Use a dummy node if **any** of these are true:

✔ Head **may change**
✔ Operation involves **position 1**
✔ You are **deleting nodes**
✔ You are **reversing a sublist**
✔ You want to **avoid special-case code**

If **all are NO** → dummy likely unnecessary.

---

## 🔁 Example Comparison

### ❌ Without Dummy (messy)

```java
if (left == 1) {
    // special logic
}
```

### ✅ With Dummy (clean)

```java
dummy.next = head;
leftPrev = dummy;
```

---

## 🏁 Final Thumb Rule (Memorize This)

> 🔥 **If you ever write `if (head == ...)`, you probably needed a dummy node.**
