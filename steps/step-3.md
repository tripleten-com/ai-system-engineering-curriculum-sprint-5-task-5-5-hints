# Step 3: Open the review through the two ports

## The idea

A call that passes every check has earned its one effect, and that effect lands in two places with no shared transaction:

- **A packet in the object store:** the review itself.
- **A job on the queue:** what tells dispatch the review exists.

If either one can fail independently, the order decides what a failure leaves behind.

The dispatcher's view sets the order. A job that arrives before its packet sends someone to a review that isn't there. So the packet comes first, marked as not yet announced. Then the job goes out. Then the packet is marked open. If the publish fails, the packet is marked so, and the tool reports the failure honestly instead of claiming success.

The second idea is that a repeated call must be safe. A retrying agent, a replayed run or a curious engineer can call the tool again. The packet's path holds one review per exception, so the existing packet decides:

- **An open packet:** the call is rejected.
- **A packet that was started but never announced:** the call finishes it, keeping its id and its opening time, and the review is delayed rather than lost.

One more practical point: the worker reads everything on its queue as its own kind of job. Your job needs a queue of its own, created with the same settled policy the platform uses.

> Covered in **Idempotency Keys and Deduplication** and **The Dual-Write Problem**, in **Idempotency & the Outbox Pattern**.

## What's expected

- A well-formed call against fresh stand-ins returns `opened` with a review id, leaves one packet with the status open at the review's key, and publishes one `handling_review_opened` job with the same review id.
- The same call again is rejected with `review_already_open`, and nothing new is written.
- With the store set to fail its next write, the call returns `failed` with `object_store_unavailable`, with no packet and no job.
- With the queue set to fail its next publish, the call returns `failed` with `queue_unavailable`, and the packet is marked as a failed publish.
- The next call after a failed publish returns `opened` with the same review id and opening time, and one job.
- The review queue's settings come from the same place the platform's queue settings come from.

## When you're stuck

### You can't decide what the packet holds

<details><summary>Move 1</summary>

List the fields the lesson names for the packet. Which of them came from the model, and were they validated?

</details>

<details><summary>Move 2</summary>

Does the dispatcher need anything in the packet that the exception record already holds?

</details>

<details><summary>Move 3</summary>

How is a review id made so it's unique and never comes from the model? Write that rule beside the tool.

</details>

### The queue port doesn't accept your job

<details><summary>Move 1</summary>

What type does the queue port's publish method declare?

</details>

<details><summary>Move 2</summary>

What does the queue adapter actually read from the job it's given? Read the adapter's publish method.

</details>

<details><summary>Move 3</summary>

The approach: a small model of your own holding the exception id and the review id, published through the same port. Mark that one call for the type checker with a comment that says why, and record the choice in your note.

</details>

### Your review jobs end up in the worker's queue

<details><summary>Move 1</summary>

What does the worker do with a message it can't decode as its own job?

</details>

<details><summary>Move 2</summary>

How does the initializer create the worker's queue? Read `src/api/initialize.py`.

</details>

<details><summary>Move 3</summary>

Create a queue of your own when your server starts, the same way, and publish only there.

</details>

### The new queue's policy values are typed into your code

<details><summary>Move 1</summary>

Where do the worker queue's visibility timeout and receive count come from?

</details>

<details><summary>Move 2</summary>

Does your service's environment carry those two values?

</details>

<details><summary>Move 3</summary>

Which block in `compose.yaml` would have to hold them so your service and the worker read the same values, without your service getting anything else?

</details>

<details><summary>Move 4</summary>

The approach: read both values from the environment, the way the initializer's settings read them, and make sure the environment block your service merges includes them.

</details>

### A failed publish loses the review

<details><summary>Move 1</summary>

After a failed publish, what's left in the object store, and with which status?

</details>

<details><summary>Move 2</summary>

What does the next call do when it finds that packet: reject it, start over, or finish it?

</details>

<details><summary>Move 3</summary>

Does the next call make a new review id, or keep the one that's there? What about the opening time?

</details>

<details><summary>Move 4</summary>

The approach: when the existing packet isn't open, treat it as a review started and never announced. Keep its id and its time, take this call's validated values, then publish and mark it open.

</details>

### You can't make a port fail on request

<details><summary>Move 1</summary>

Can LocalStack fail when you ask it to? Where else could a failure come from?

</details>

<details><summary>Move 2</summary>

What could a stand-in port remember that makes its next call raise?

</details>

<details><summary>Move 3</summary>

Give each stand-in a switch for "fail on the next write" or "fail on the next publish". It raises before it applies the effect, then resets.

</details>
