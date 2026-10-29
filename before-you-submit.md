# Before you submit

The basic checks confirm that your work is there. They don't judge whether it's right. Your instructor reads your pull requests and evidence notes with four questions. Ask them of your own work first.

## Did you meet the Task's requirements?

- Does the tool reject in the fixed order, with codes from the guardrail's table and nothing the model wrote?
- Does it bind the call to the exception before any other authority check, and load the configuration on every call?
- Does a rejection change nothing, and does a failed publish leave a packet a later call can finish?
- Does your service's environment hold only what the two adapters and your queue setup read, with no new value typed in?
- Does `poe demo` still pass with the new service in the stack?

## Did you explain your design choices?

- Does your note say how the context travels beside the arguments, and why there?
- Does it say how a review id is made and what the tool does with a broken configuration file?
- Does it say what a failed result after a publish does and doesn't tell the caller?

## Can your tests fail when the code is wrong?

- Does every rejection test assert the exact code and that nothing changed?
- Did you remove the binding check, accept an unknown field and hide a publish failure once each, and see a test fail?

## Did you state the limits of what your checks show?

- Does your note say what your checks don't prove, such as two calls arriving at the same moment?
- Does your boundary document from Task 4 still match what the tool does?
