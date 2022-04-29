# TStack

A Stack data structure is the conceptual equivalent of a stack of books. We can `push` a new element
onto the stack. We can also `pop` the top-most element off the stack. One mutable API for a Stack is:

    def new() -> Stack
    def push(int)
    def pop() -> int

If you prefer safe & immutable APIs, you might prefer this one:

    def empty() -> Stack
    def push(int) -> Stack
    def pop() -> Option<Pair<Stack, int>>

The TStack or Transactional Stack is a Stack data structure with three new operations: `begin`,
`commit`, and `rollback`. The `begin` operation starts a "transaction". All subsequent `push` and
`pop` operations continue to work as described above. However, the user may now call the `rollback`
operation which changes the stack so that it appears as if the operations in the transaction never
happened.

The user may also call the `commit` operation which ends the current transaction. Calling `rollback`
when there is no active transaction is an error.

Transactions may be nested. This occurs if `begin` is called while a transaction is still active.

Here are some examples:

    s = new()
    s.push(1)
    s.begin()
    assert s.pop() == 1
    s.push(2)
    s.begin()
    s.push(3)
    assert s.pop() == 3
    assert s.pop() == 2

Example two:

    s = new()
    s.push(1)
    s.begin()
    s.push(2)
    s.begin()
    s.push(3)
    s.rollback()
    assert s.pop() == 2
    assert s.pop() == 1
    s.rollback()
    assert s.pop() == 1

Example three:

    s = new()
    s.push(1)
    s.begin()
    s.push(2)
    s.begin()
    s.push(3)
    s.commit()
    assert s.pop() == 3
    s.rollback()
    assert s.pop() == 1

Your task is to implement a Transactional Stack in your favorite language. Your API should have at
least `push`, `pop`, `commit`, `rollback`, and `begin`. Your API may be mutable or immutable. When
we meet, we will discuss the correctness, asymptotic computational complexity, asymptotic memory
complexity, and the practical performance on a modern computer.

This problem is hard. We do not expect every candidate to implement a completely correct and maximally efficient
stack on the first try. Instead, we endeavor to probe your ability to reason about a modern computer and the programs
executing upon it.
