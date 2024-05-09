---
layout: post
title: Intro to RTL
date: 2024-05-08 21:30 -0600
---

At work I often have side projects — things that aren't exactly
on the roadmap but that I think are worth doing and that
leadership gives a blessing to. Sometimes it's a PoC, sometimes
it's better described as "glue work" — work that helps us function better as a
software development operation.

One of my latest has been thinking about how to improve our
automated testing.

Kibana is historically super heavy on end-to-end browser-based testing
(the team calls these "functional tests") and it gets expensive. With the upcoming
release of [three distinct
incarnations](https://docs.elastic.co/serverless) of Kibana, each with
its own test suite, the problem has only grown.

In some ways, a top-heavy testing pyramid makes sense for Kibana.
We're heavy on UI in a way that a project like Elasticsearch
definitely isn't.

But, functional tests are also flaky, especially at scale. This is a bigger
deal than we might be tempted to give it credit for.

If a test
has a 1% chance of a false positive (failing when it shouldn't), 
that may not look like much. Multiply that over the number of PRs 
open in our repo on any given day and it starts to be an issue.

<div style="width: 100%" id="chart1"></div>

Starts to look bad, doesn't it? But it gets much worse...

The graph above assumes that each CI run invokes a single test with a 1% flake rate.
But that isn't true either. Let's say CI runs even an extremely modest number of
100 tests, each with a 1% chance of producing a false positive.

Then, the probability of a false positive for a _single CI run_
grows to a terrifying `1−(1−0.01)^100 ≈ 0.6339`!


Applying this to the first graph, it becomes obvious that the
developer experience for even a _1-person team_ with a tiny test suite
will be absolutely unbearable.

<div style="width: 100%" id="chart3"></div>

And, though indispensible as a high-fidelity validation engine,
functional tests are just inefficient for fine-grained UI 
testing. 

Kibana is a complex analytics platform with miles
of UI views. But these views are composed of smaller user experiences
each of which can have many possible interaction permutations. In theory, 
it's much more efficient and robust to test these
small, complicated units in a more isolated environment. 

So, I wondered, _why do we write so many functional tests?_

I started interviewing my teammates and folks
from our sister teams. Themes quickly emerged.

One was that Kibana's dependency injection system isn't available
during Jest tests, so it is often impossible to get ahold of key
dependencies. This leads to the need for writing many mocks, a
source of friction.

But another was that using Enzyme (our usual tool of choice)
to write Jest tests for UI was just a bad
developer experience. 

Components under test in Enzyme often don't act like they do in
the browser without some serious coaxing. Usually, this is because 
Enzyme does a poor job of handling
asynchronous behavior. I found that developers had all sorts of tricks to
force their React components into the right state and they routinely
sprinkled these hacks all over until the component
started "working properly."

After having to do so much just to get the UI 
to act like it really does in our application, it's hard not to question the
fidelity (and hence, the value) of the tests anyways.

Enzyme also lacks the visual feedback of a browser test. When a
browser test is failing, you can run the test and literally watch
the test runner click through the app until something fails.
Enzyme just tells you it couldn't find a DOM element that matched
your query. Good luck!

The cool thing about side projects is that they add a hook to your
brain. When you see something relevant to your project as you're
going about your scheduled work, you notice it in a way you
wouldn't if you didn't have an agenda.

I took my next large UI project as an opportunity to try out [React
Testing
Library](https://testing-library.com/docs/react-testing-library/intro/)
and quickly found that it solved the fundamental
DX and trust problems of Enzyme.

Async behavior is easy to handle in RTL. It provides `wait`
helpers very similar to those we use in our browser test runner.
You no longer have to delve into React render cycles to understand
why things are failing... if you know something is supposed to
happen, just `wait` for it like you do in a functional test.

If the RTL test runner can't find the element matching your query,
it pretty-prints the whole DOM with helpful messages. Visual
feedback? Check!

What's more is that RTL builds trust in the test by confining
you so that your tests have to interact with the very same
interface your customers do: the DOM. And it does a great job of
simulating a full-DOM render.

I was wowed and I quickly wrote an RFC proposing we cease and desist from using
Enzyme and we write all new Jest UI tests with RTL. The leads
signed off on it and asked me to introduce the larger team to RTL.

Here's the presentation I gave (built with
[Big](https://github.com/tmcw/big)).

<script src="https://cdn.jsdelivr.net/npm/vega@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-embed@6"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite-api@5"></script>

<script>
/*
    const probabilityThatTheTestDoesntFail = .99;

    const probabilities = [];
    for (let i = 1; i< 100; i++) {
        probabilities.push({probability: 1 - Math.pow(probabilityThatTheTestDoesntFail, i), trials: i });
    }

   const spec = {
      $schema: "https://vega.github.io/schema/vega-lite/v5.json",
      description: "Probability of a test flake at least once in N CI runs",
      width: 800,
      height: 400,
      data: { values: probabilities },
      mark: "bar",
      encoding: {
        x: { field: "trials", type: "quantitative", title: "Number of runs" },
        y: {
          field: "probability",
          type: "quantitative",
          title: "Probability of a flake",
          scale: { domain: [0, 1] },
        },
      },
    };

    vegaEmbed("#chart", spec); 
*/

    const trials = 100; // Number of CI runs
    const singleTestFlakeRate = 0.01; // Probability of false positive

    const graphFalsePositivesOverNCIRuns = (mountPoint, singleTestFlakeRatePerRun) => {
        const probabilities = [];

        let cumulativeProbability = 0;
        for (let i = 1; i <= trials; i++) {
          cumulativeProbability += (1 - cumulativeProbability) * singleTestFlakeRatePerRun;
          probabilities.push({ trials: i, probability: cumulativeProbability });
        }

        const spec = {
          $schema: "https://vega.github.io/schema/vega-lite/v5.json",
          title: "Probability of encountering at least one false positive",
          width: 'container',
          data: { values: probabilities },
          mark: "line",
          encoding: {
            x: { field: "trials", type: "quantitative", title: "Number of CI runs" },
            y: { field: "probability", type: "quantitative", title: "Cumulative probability" },
          },
        };

        vegaEmbed(mountPoint, spec);
    }

    const graphFalsePositivesForSingleCIRun = () => {
        const probabilities = [];

        for (let i = 1; i <= trials; i++) {
          probabilities.push({ numTests: i, probability: 1 - Math.pow(1 - singleTestFlakeRate, i) });
        }

        const spec = {
          $schema: "https://vega.github.io/schema/vega-lite/v5.json",
          title: "Probability of a test flake during a single CI run",
          width: 'container',
          data: { values: probabilities },
          mark: "line",
          encoding: {
            x: { field: "numTests", type: "quantitative", title: "Number of tests" },
            y: { field: "probability", type: "quantitative", title: "Probability of test flake" },
          },
        };

        vegaEmbed("#chart2", spec);
    }

    graphFalsePositivesOverNCIRuns("#chart1", singleTestFlakeRate); 
    graphFalsePositivesForSingleCIRun();
    
    const flakeRateOver100Tests = 1 - Math.pow(1 - singleTestFlakeRate, 100);
    graphFalsePositivesOverNCIRuns("#chart3", flakeRateOver100Tests); 
</script>
