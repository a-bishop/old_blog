---
layout: post
title: Wrapping Mocha Tests With A Custom Retry Handler
tags: mocha chai sinon javascript testing
---

When writing integration tests for interactions with external services, it can be difficult to predict the amount of time that these processes will take. As a result, it is tempting to add large timeouts to ensure that the side effects of each network call have completed before moving on to the next step of the test.

For instance, a request sent to an endpoint that responds via webhook and triggers a database update could take longer than expected to complete. The naive solution would be to add a lengthy `setTimeout` function after the external call.

## Retry handlers

While writing integration tests in Mocha for the payment platform at Change.org, I helped develop a method that would allow us to avoid the use of these heavy-handed timeouts. We wanted to be able to retry the test if it failed, using increasing timeouts calculated on each run.

The technique involved wrapping the test in a function that takes in the number of attempts desired, and returns a test handler with a function argument that when called calculates an adjusted timeout based on the attempt count and a multiplication factor (which can be configured). This function knows the attempt count on each test run because it has been bound via closure in the handler.

```js
// test_utils.js

// the test wrapper
function withAttempts(totalAttempts, testFunc) {
  return async function() {
    // grab the retry count from mocha's test instance
    const retryCount = this.runnable().currentRetry();
    const attemptCount = retryCount + 1;

    // if it's the first run of the test, set the number of desired retries.
    if (!retryCount) this.retries(totalAttempts - 1);
    else debug(`retry: ${this.test.title}`);

    // define a weightedSleep function, with the attemptCount bound via closure
    function weightedSleep(milliseconds, options) {
      return sleep(
        getWeightedValue(milliseconds, { attemptCount, ...options })
      );
    }

    try {
      // run the test and pass in an object with the timeout function and the attempt count
      return await testFunc({ weightedSleep, attemptCount });
    } catch (e) {
      debug(`error: ${e}`);
      // throwing the caught error will retry the test
      throw e;
    }
  };
}

function sleep(milliseconds) {
  return new Promise(resolve => setTimeout(resolve, milliseconds));
}

// Increase a value by the given factor based on the attempt count.
// For example, if the initial value is 1000, with a default increment
// factor of 0.5 (50%), if the attempt count is 1 the return value will be 1000,
// if the attempt count is 2 the return value will be 1500, and if the attempt
// count is 3 the return value will be 2000.
function getWeightedValue(value, { attemptCount = 1, incrementFactor = 0.5 }) {
  return attemptCount > 1
    ? value + value * attemptCount * incrementFactor
    : value;
}
```

And here's how the wrapper would be used in the context of a test:

```js
// my_integration.test.js

describe('My Integration', function() {
    context('when I make a network request with side effects', () => {
        it('the correct value is eventually recorded in my database', withAttempts(3, async ({ weightedSleep }) => {
                // make a time-consuming network call with side effects
                await fetchSomethingTimeConsuming();
                // set a timeout of 5 seconds on initial run, with steadily
                // increasing timeouts on subsequent runs
                await weightedSleep(5000);
                // assert on the value of something that happened as a side effect
                // of the network call. If this assertion fails
                // the test will retry with a new weightedSleep function
                expect(await getValueInDatabase()).to.equal({foo: 'bar'})
            })
        )
    });
}
```

The nice thing about this pattern is that we can start by writing tests with many retries and low timeouts, and analyze the runs to determine at what timeout threshold the tests become stable.
