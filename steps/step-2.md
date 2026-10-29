# Step 2: Enforce the authority boundary you wrote in Task 4

## You're not sure which configuration fields the tool checks

<details><summary>Hint 1</summary>

For each field in your configuration, ask: can one call show whether it holds, or do you need the whole run?

</details>

<details><summary>Hint 2</summary>

Which fields describe a run rather than a call? Who sees the run? Write down who enforces each of the nine fields.

</details>

## Editing the configuration file doesn't change the tool

<details><summary>Hint 1</summary>

When does your tool read the file? Once when it starts, or on every call?

</details>

## The first call for an exception fails

<details><summary>Hint 1</summary>

What does the object store do when you read a key that doesn't exist? Read `docs/fidelity/ObjectStore.md` and the adapter.

</details>

<details><summary>Hint 2</summary>

Is a missing packet an error, or the normal case for a first call? Which status means a review is already open?

</details>

## A review opens for the wrong exception

<details><summary>Hint 1</summary>

Which check runs first in your tool? The state you check belongs to one exception. Is that the exception the arguments name?

</details>
