# Step 1: Define the call and validate every input

## The idea

The tool you're building is the only code in the agent that changes anything. Its arguments will be written by a model, so treat them like input from a stranger: check everything, assume nothing, and refuse cleanly.

Two kinds of information reach the tool, and keeping them apart is the heart of this Step:

- **The arguments:** what the model wrote, such as which exception, why, which procedure and what next step to propose. The tool trusts none of it.
- **The call context:** what the caller knows and the model never writes, such as which exception this run is about, what state it's in, and which run is calling. The tool's container can't reach the database, so it can't look these up. The caller supplies them, and the tool checks the arguments against them.

Validation runs in a fixed order, and the first broken rule wins:

1. an unknown field
2. a missing field
3. a wrong type
4. a value outside its format or its closed list

The order matters. A misspelled field is both an unknown field and a missing one, and the model's invention should be the first thing reported.

A bad input produces a rejection: an ordinary result with a code from the guardrail's own table. It never raises an error. An error means the tool itself failed, which is a different outcome with a different meaning for the caller.

## What's expected

- The tool is one function that takes the model's arguments, a typed call context and the two ports.
- Its result type has three outcomes a caller can tell apart: `opened` with a review id, and `rejected` or `failed` with a code. A result that mixes them can't be built.
- In the shell, each of your input files gets the code its comment names, and the stand-in ports hold nothing afterwards.
- No rejection contains anything the model wrote. Codes are spelled as the guardrail's table spells them, with a contract field name after the colon where the code takes one.
- A misspelled field name is reported as an unknown field.

## When you're stuck

### You can't decide where the exception's state comes from

<details><summary>Move 1</summary>

Who knows the exception's state: the model that wrote the arguments, or the caller that started the run?

</details>

<details><summary>Move 2</summary>

Can the tool's container reach the database?

</details>

<details><summary>Move 3</summary>

If the model could write the state, what could it unlock?

</details>

<details><summary>Move 4</summary>

The approach: a small typed context object with the exception's id, its state and the run's id, passed to the tool beside the arguments. Only the caller fills it, and the tool never reads the state from the arguments.

</details>

### The validation order comes out wrong

<details><summary>Move 1</summary>

Write a call with a misspelled field. Which code does your tool return first?

</details>

<details><summary>Move 2</summary>

Does your code look at the field names before it reads any value?

</details>

<details><summary>Move 3</summary>

Walk your checks in the lesson's order and stop at the first one that fails. Is any check out of place?

</details>

<details><summary>Move 4</summary>

The approach: four passes over the arguments in order. Each pass returns its code the moment it finds a problem, and only a clean pass moves on to the next.

</details>

### A bad input crashes the tool instead of being rejected

<details><summary>Move 1</summary>

Where does the error come from: one of your checks, or the code that turns the arguments into a typed object?

</details>

<details><summary>Move 2</summary>

What does a caller see when a tool raises, compared with a returned rejection?

</details>

<details><summary>Move 3</summary>

Run your checks on the plain mapping before anything converts it, or catch the conversion's failure and turn it into the first rule it broke.

</details>

### Your result can mix two outcomes

<details><summary>Move 1</summary>

Can you build a result that says `opened` and also carries a reason?

</details>

<details><summary>Move 2</summary>

How did your Task 1 contract stop a tool input on the wrong kind? The same kind of check works here.

</details>

<details><summary>Move 3</summary>

Write the rules down: opened has a review id and no reason, and the other two have a reason and no review id. Then enforce them when the result is built.

</details>

### A rejection repeats what the model wrote

<details><summary>Move 1</summary>

Read one of your rejection codes aloud. Does any part of it come from the input?

</details>

<details><summary>Move 2</summary>

Which part of an unknown-field rejection would tempt you to include the field's name? Who wrote that name?

</details>

<details><summary>Move 3</summary>

Use only the table's code and, where the code takes one, a field name from your contract. Never the input's own text.

</details>

### You can't find the procedure id's format

<details><summary>Move 1</summary>

Which file declares what a procedure id looks like? Search the exception summary schema.

</details>

<details><summary>Move 2</summary>

Does your Task 4 loader already read that schema? Which parts does it expose?

</details>

<details><summary>Move 3</summary>

Add one small reader beside the ones your loader has, so the tool checks every closed form from its source.

</details>

### You don't have ports to call the tool with

<details><summary>Move 1</summary>

Open `tests/doubles/retrieval.py`. How does it stand in for a port?

</details>

<details><summary>Move 2</summary>

Which methods do the object store port and the queue port declare?

</details>

<details><summary>Move 3</summary>

Write one small in-memory stand-in per port under `tests/doubles/`. Each holds what it was given, so you can look inside after a call.

</details>
