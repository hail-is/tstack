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
`commit`, and `rollback`. The `begin` operation conceptually stores the state of the stack so that
it can be returned to at a later time. The `rollback` operation returns to the state of the most
recent `begin`.

The `begin` and `rollback` operations themselves form a stack-like structure. After calling
`rollback`, the most recent `begin` becomes inaccessible. Calling `rollback` again would restore the
stack to the state at next most recent `begin`. For example:

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

The `commit` operation allows us to skip over a `begin` without restoring its state. For example:

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

We do not expect every candidate to implement a completely correct and maximally efficient
stack. Instead, we endeavor to probe your ability to reason about a modern computer and the programs
executing upon it.
