# Step 4: Serve the tool from its own container and call it with a dev command

## The idea

Where the tool runs is part of the boundary. It's the only code with a side effect, so it gets a container of its own that holds what its two adapters need and nothing else: no database connection string, no model key, nothing about tokens. A mistake in the tool, or a model that talks it into something, then has no door to walk through. Every variable you leave out of its environment is a door that doesn't exist.

MCP is how the agent will call the tool across that container boundary. An MCP server declares its tools, so a client can list them, and runs one when a client calls it. Two details matter here:

- **The declaration shouldn't filter arguments:** a convenient way of declaring tools builds the declaration from your function's signature and silently drops anything else. Your unknown-field check would then never see an unknown field.
- **The context travels beside the arguments, never inside them:** an MCP call can carry extra information next to its arguments, and that's where the caller's context goes.

Your dev command plays the agent's future role by hand. It creates exceptions, builds each call's context, calls the server and prints what happened.

> Covered in **Authorization: RBAC, Scopes, and Least Privilege**, in **Trust Boundaries & Access Control**, and in **3.4 Offering Tools over MCP**.

## What's expected

- A client that lists the server's tools sees one tool, `open_handling_review`, with your contract's inputs.
- An input with an unknown field, sent through the server, comes back rejected as an unknown field.
- The service's resolved configuration shows the object store and queue settings, and no database URL, model or record paths, or token settings.
- `poe start` waits for the service, and `docker compose ps` shows it healthy.
- Your dev command prints one row per input file: the input's name, the status, the code or the review id, and whether the store and the queue changed.
- The well-formed input opens one review, `second-call` is rejected as already open, and every other row matches the rule its input breaks.

## When you're stuck

### `unknown_property` never comes back through the server

<details><summary>Move 1</summary>

Send an input with an extra field through the server and print what your function receives. Is the extra field there?

</details>

<details><summary>Move 2</summary>

What does the way you declared the tool do with an argument the declaration doesn't name?

</details>

<details><summary>Move 3</summary>

The approach: declare the tool through the SDK's lower-level server, which publishes the declared inputs for clients but hands your handler every argument it was sent.

</details>

### You can't find how the pinned MCP package declares a server

<details><summary>Move 1</summary>

Which version is pinned? Read it in the lock file. Does the example you're following match that version?

</details>

<details><summary>Move 2</summary>

Open the installed package's source for its lower-level server class. How does it take the handlers for listing and calling tools?

</details>

<details><summary>Move 3</summary>

In the same source, find what a call handler receives besides the arguments. That's where the caller's extra information arrives.

</details>

### The context gets mixed into the arguments

<details><summary>Move 1</summary>

Besides its arguments, what else can an MCP tool call carry?

</details>

<details><summary>Move 2</summary>

How does the pinned client send that extra part, and where does the server read it?

</details>

<details><summary>Move 3</summary>

The lesson allows two carriers: the request's metadata, or a separate top-level field the server knows is never the model's. Choose one, and decide what the server does when the context is missing or malformed. Then write down why, because how the context travels is a choice you defend.

</details>

### The service's environment holds too much

<details><summary>Move 1</summary>

Print your service's resolved configuration and read its environment as a list of things it can reach. Is a database connection string among them?

</details>

<details><summary>Move 2</summary>

A YAML merge can't leave keys out. What would you need to split so your service merges only part of the worker's block?

</details>

<details><summary>Move 3</summary>

The approach: move the object store and queue lines into a block of their own, merge that block into the worker's block so the worker sees no change, and merge only the new block into your service.

</details>

### Your dev command can't see whether anything changed

<details><summary>Move 1</summary>

Your dev command runs on your machine, not in a container. How does an existing helper reach LocalStack from the host? Read `tests/failure/queue_client.py`.

</details>

<details><summary>Move 2</summary>

What would you compare before and after each call?

</details>

<details><summary>Move 3</summary>

List the keys under the review prefix and read your review queue's depth before and after each row, and print whether either changed.

</details>

### The service is up but `poe ready` doesn't mention it

<details><summary>Move 1</summary>

Which services does `poe ready` check?

</details>

<details><summary>Move 2</summary>

Which check waits for your service, then?

</details>

<details><summary>Move 3</summary>

Give the service a health check and confirm it with `docker compose ps` before you run the dev command.

</details>
