# Step 3: Open the review through the two ports

## The queue port doesn't accept your job

<details><summary>Hint 1</summary>

What type does the queue port's publish method declare? What does the adapter actually read from the job it's given?

</details>

<details><summary>Hint 2</summary>

The lesson describes one way through that doesn't change the port. What does the type checker need to hear, and what does your evidence note need to say?

</details>

## Your review jobs end up in the worker's queue

<details><summary>Hint 1</summary>

What does the worker do with a message it can't decode as its own job? Which queue does your tool publish to?

</details>

<details><summary>Hint 2</summary>

How does the initializer create the worker's queue? Read `src/api/initialize.py`, including where its queue policy values come from.

</details>

## The new queue's policy values are typed into your code

<details><summary>Hint 1</summary>

Where do the visibility timeout and the receive count come from for the worker's queue? Can your service read the same values?

</details>

<details><summary>Hint 2</summary>

Does your service's environment carry those two values? If it doesn't, which block in `compose.yaml` would have to hold them for both your service and the worker to see the same ones?

</details>

## A failed publish loses the review

<details><summary>Hint 1</summary>

After a failed publish, what's left in the object store, and with which status? What does the next call do when it finds that packet?

</details>

<details><summary>Hint 2</summary>

Does the next call make a new review id, or keep the one that's there? What about the opening time?

</details>

## You can't make a port fail on request

<details><summary>Hint 1</summary>

Does LocalStack fail when you ask it to? Where else can a failure come from in a test?

</details>

<details><summary>Hint 2</summary>

What could your stand-in ports remember that makes their next call raise, before they apply the effect?

</details>
