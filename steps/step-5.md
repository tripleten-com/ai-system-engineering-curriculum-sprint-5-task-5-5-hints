# Step 5: Prove it with tests and write the evidence note

## `review_already_open` shows up in a test that didn't ask for it

<details><summary>Hint 1</summary>

Which ports does that test pass to the tool? Could something from an earlier run still be in them?

</details>

## A broken tool still passes the rejection tests

<details><summary>Hint 1</summary>

What does each test assert about the result: only that it isn't opened, or its exact status and reason?

</details>

<details><summary>Hint 2</summary>

After each rejected call, do you check that the stand-ins hold what they held before?

</details>

## Your tests mostly test the stand-ins

<details><summary>Hint 1</summary>

Does a test write to a stand-in and read the same thing back? What part of your tool did that test run?

</details>

## The demo fails after you add the service

<details><summary>Hint 1</summary>

Does the stack start? Read your service's logs and its health in `docker compose ps`. An import error shows up there before any call.

</details>

## Your boundary document no longer matches the tool

<details><summary>Hint 1</summary>

Where does your Task 4 document say the tool gets the exception's state? Is that still true?

</details>
