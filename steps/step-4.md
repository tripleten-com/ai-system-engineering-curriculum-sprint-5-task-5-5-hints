# Step 4: Serve the tool from its own container and call it with a dev command

## `unknown_property` never comes back through the server

<details><summary>Hint 1</summary>

What does the server do with an argument the tool's declaration doesn't name? Does your function ever see it?

</details>

<details><summary>Hint 2</summary>

The lesson warns about one way of declaring tools. Which other way still lists the contract's inputs for a client and hands your function every argument it was sent?

</details>

## You can't find how the pinned MCP package declares a server

<details><summary>Hint 1</summary>

Which version is pinned? Read it in the lock file. Does the example you're following match that version?

</details>

<details><summary>Hint 2</summary>

Read the installed package's source for the lower-level server class. How does it take its handlers, and where does a handler find what the request carried beside its arguments?

</details>

## The context gets mixed into the arguments

<details><summary>Hint 1</summary>

Besides its arguments, what else can an MCP tool call carry? Where does the server's handler read it from?

</details>

## The service's environment holds too much

<details><summary>Hint 1</summary>

Print your service's resolved configuration and read its environment as a list of things it can reach. Is a database connection string among them?

</details>

<details><summary>Hint 2</summary>

A YAML merge can't leave keys out. How could you split the worker's block so your service merges only the part it needs, and the worker still sees everything it had?

</details>

## Your dev command can't see whether anything changed

<details><summary>Hint 1</summary>

Your dev command runs on your machine, not in a container. How does an existing helper reach LocalStack from the host? Read `tests/failure/queue_client.py`.

</details>

<details><summary>Hint 2</summary>

What would you compare before and after each call: the keys under the review prefix, and the depth of which queue?

</details>

## The service is up but `poe ready` doesn't mention it

<details><summary>Hint 1</summary>

Which check waits for your service to be healthy? Is it `poe ready`, or something `poe start` waits on?

</details>
