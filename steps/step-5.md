# Step 5: Prove it with tests and write the evidence note

## The idea

The tool's tests answer three questions:

- **The success path:** does a well-formed call open exactly one review?
- **The rejections:** does each rule reject its input with the right code, and change nothing?
- **The failures:** does each failure return its code and leave behind what the requirements say?

The tests call the function directly, with your stand-in ports and a context the test builds. No stack, no server. That keeps them fast and repeatable, and it keeps them independent of whatever an earlier run left in LocalStack.

Two traps make tool tests look stronger than they are:

- **Checking only that a result isn't `opened`:** a broken tool that fails for the wrong reason passes that check. Assert the exact status and reason, and that the stand-ins hold what they held before.
- **Testing the stand-ins instead of the tool:** a test that writes to a stand-in and reads it back proves the stand-in works. Change the stand-ins only through the tool, and read them only to check what the tool did.

Load every input from its file, bound to the context's exception id as its comment says, so the tests and the dev command use the same inputs.

> Covered in **CI Gates and Release Verification**.

## What's expected

- One test per input file asserts the exact code and that no packet and no job were added.
- The success test asserts these:
  - one open packet with your contract's fields
  - one job with the same review id
- The failure tests cover four cases:
  - a failed write
  - a failed publish, with the packet marked
  - the call after a failed publish, with the same id and time
  - a failed final rewrite
- You broke each group once and saw tests fail:
  - removed the binding check
  - accepted an unknown field
  - reported a failed publish as success
- `poe demo` still runs with the new service in the stack.
- Your Task 4 boundary document says the tool receives the exception's state from its caller.
- `poe verify` passes after your last edit.

## When you're stuck

### `review_already_open` shows up in a test that didn't ask for it

<details><summary>Move 1</summary>

Which ports does that test pass to the tool?

</details>

<details><summary>Move 2</summary>

Could something from an earlier test, or an earlier run, still be in them?

</details>

<details><summary>Move 3</summary>

Give every test fresh stand-ins from a fixture, and never pass the live adapters in tests.

</details>

### A broken tool still passes the rejection tests

<details><summary>Move 1</summary>

What does each rejection test assert: only that the result isn't opened, or its exact status and reason?

</details>

<details><summary>Move 2</summary>

After each rejected call, do you check that the stand-ins hold what they held before?

</details>

<details><summary>Move 3</summary>

Remove the binding check and run your tests. If none fails, the mismatch test isn't checking the code it should.

</details>

### Your tests mostly test the stand-ins

<details><summary>Move 1</summary>

Does any test write to a stand-in and read the same thing back?

</details>

<details><summary>Move 2</summary>

Which part of your tool did that test run?

</details>

<details><summary>Move 3</summary>

Set up the stand-ins only through the tool, or with the minimum a scenario needs, such as an existing open packet for `second-call`. Then read them only to check the tool's effect.

</details>

### The demo fails after you add the service

<details><summary>Move 1</summary>

Does the stack start? Read your service's log and its health in `docker compose ps`.

</details>

<details><summary>Move 2</summary>

Does the demo depend on anything your new service changed, such as a shared environment block?

</details>

<details><summary>Move 3</summary>

Fix the service until the stack is healthy, then run `poe demo` again. The agent still ends a tool call as not enabled in this Task, so no review opens in the demo.

</details>

### Your boundary document no longer matches the tool

<details><summary>Move 1</summary>

Where does your Task 4 document say the tool gets the exception's state?

</details>

<details><summary>Move 2</summary>

Is that still true now that the context carries it?

</details>

<details><summary>Move 3</summary>

Update that sentence so the document and the code say the same thing, and mention the change in your note.

</details>
