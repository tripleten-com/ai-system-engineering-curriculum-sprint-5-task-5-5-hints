# Step 1: Define the call and validate every input

## You can't decide where the exception's state comes from

<details><summary>Hint 1</summary>

Who knows the exception's state: the model that wrote the arguments, or the caller that started the run? Which of the two can the tool trust?

</details>

<details><summary>Hint 2</summary>

Can the tool's container reach the database? If it can't, which part of the call must carry the state, and who fills it?

</details>

## The validation order comes out wrong

<details><summary>Hint 1</summary>

Write a call with a misspelled field. A misspelling is one unknown field and one missing field at once. Which code does your tool return first?

</details>

<details><summary>Hint 2</summary>

Does your code look at the field names before it reads any value? Walk through your checks in the order the lesson fixes and stop at the first one that fails.

</details>

## A bad input crashes the tool instead of being rejected

<details><summary>Hint 1</summary>

Where does the error come from: your checks, or the code that converts the arguments into a typed object?

</details>

<details><summary>Hint 2</summary>

What does a caller see when a tool raises, compared with when it returns a rejection? Catch the conversion's failure and turn it into the first rule it broke.

</details>

## You can't find the procedure id's format

<details><summary>Hint 1</summary>

Which file declares what a `procedure_id` looks like? Search the schema for it.

</details>

<details><summary>Hint 2</summary>

Your Task 4 loader reads some of the schema already. Does it expose the part you need here, or does it need one more small reader beside the ones it has?

</details>

## A rejection repeats what the model wrote

<details><summary>Hint 1</summary>

Read one of your rejection codes aloud. Does any part of it come from the input rather than from the contract?

</details>
