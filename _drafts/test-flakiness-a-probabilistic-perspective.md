---
layout: post
title: 'Test flakiness: a probabilistic perspective'
date: 2024-05-14 20:46 -0600
---


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
