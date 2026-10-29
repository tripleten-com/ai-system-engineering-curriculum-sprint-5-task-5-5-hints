# Step 2: Enforce the authority boundary you wrote in Task 4

## The idea

A well-formed call can still ask for something the boundary forbids:

- a review for an exception that's already settled
- a review for a different exception than the one the run is about
- a next step the schema knows but this workflow never lets the agent propose

Step 1 checked that the input makes sense. This Step checks that it's allowed.

Your Task 4 configuration is where the limits live. Some of its fields are about one call, and the tool can check those. Others are about a whole run, like the review cap or the approval requirement, and the tool can't see a whole run, so the agent enforces them in later Tasks. The prohibited actions have no check in any code at all. They're kept by what the tool's container can't reach.

Three habits keep the enforcement honest:

- **Bind before you judge:** compare the exception id in the arguments with the one in the context first. The state you're about to check belongs to that exception and no other.
- **Read limits at call time:** load the configuration through your loader on every call, so an edited file changes the tool at once.
- **A missing review is normal:** most calls are the first for their exception. Only a packet already marked open means a review exists.

> Covered in **3.3 Authority Boundaries**.

## What's expected

- A call whose exception id differs from the context's is rejected with `value_not_permitted:exception_id`, before any other authority check.
- A context whose state isn't permitted is rejected with `state_not_permitted`.
- A reason code or next step outside your configured lists is rejected with `value_not_permitted` and the field's name.
- With an open packet already in the object store stand-in, the same input is rejected with `review_already_open`.
- After every rejection the stand-ins hold exactly what they held before.
- Changing a list in the configuration file changes the tool's answer on the next call, with no restart.

## When you're stuck

### You're not sure which configuration fields the tool checks

<details><summary>Move 1</summary>

For each of the nine fields, ask: can one call show whether it holds, or do you need the whole run?

</details>

<details><summary>Move 2</summary>

Which fields describe a run? Who sees the run: the tool, or the agent?

</details>

<details><summary>Move 3</summary>

Which field has no check in any code, and what keeps it instead?

</details>

<details><summary>Move 4</summary>

Write down who enforces each of the nine fields. The tool checks the permitted states and the two allowed lists. The agent will handle the run-level ones. The prohibitions are kept by the container's reach.

</details>

### A review opens for the wrong exception

<details><summary>Move 1</summary>

Which check runs first in your tool's authority part?

</details>

<details><summary>Move 2</summary>

The state you check comes from the context. Is that context about the exception the arguments name?

</details>

<details><summary>Move 3</summary>

Compare the two ids before any other authority check, and reject a mismatch with the code the lesson gives.

</details>

### The first call for an exception fails

<details><summary>Move 1</summary>

What does the object store do when you read a key that doesn't exist? Read `docs/fidelity/ObjectStore.md` and the adapter.

</details>

<details><summary>Move 2</summary>

Is a missing packet an error, or the normal case for a first call?

</details>

<details><summary>Move 3</summary>

Catch the store's "not found" and treat it as "no review yet". Reject only when a packet exists with the status open.

</details>

### Editing the configuration file doesn't change the tool

<details><summary>Move 1</summary>

When does your tool read the file: once when it starts, or on every call?

</details>

<details><summary>Move 2</summary>

Where did you put the loaded values: in a module-level variable, or in a local one inside the call?

</details>

<details><summary>Move 3</summary>

Call your Task 4 loader inside the tool on every call. Then decide what the tool does with a broken file, and write that decision down.

</details>
