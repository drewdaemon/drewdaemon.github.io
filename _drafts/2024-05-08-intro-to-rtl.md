---
layout: post
title: Intro to React Testing Library
date: 2024-05-08 21:30 -0600
---

TL;DR — I made [a presentation](/assets/presentations/intro-to-rtl) to introduce a
group of [Enzyme](https://enzymejs.github.io/enzyme/) users to
[React Testing Library](https://testing-library.com/docs/react-testing-library/intro/).

---

At work I often have side projects — things that aren't exactly
on the roadmap but that I think are worth doing and that
leadership gives a blessing to. Sometimes it's a proof-of-concept, sometimes
it's better described as "glue work" — work that helps us function better as a
software development operation.

One of my latest has been thinking about how to improve our
automated testing.

Kibana is historically super heavy on end-to-end browser-based testing
(the team calls these "functional tests") and it gets expensive. With the upcoming
release of [three distinct
incarnations](https://docs.elastic.co/serverless) of Kibana, each with
its own test suite, the problem has only grown.

Now, I can sympathize with our top-heavy [testing pyramid](https://martinfowler.com/articles/practical-test-pyramid.html).
We're heavy on UI in a way that a project like Elasticsearch
definitely isn't.

But, functional tests are also flaky, especially at scale. This is a bigger
deal than we might be tempted to give it credit for.
And, though indispensible as a high-fidelity validation engine,
functional tests are just inefficient for fine-grained UI 
testing. 

Kibana is a complex analytics platform with miles
of UI views. But these views are composed of smaller user experiences,
each of which can have many possible interaction permutations.
It's much more efficient and robust to test these
small, complicated units in a more isolated environment. 

So, I wondered, _why do we write so many functional tests?_

I started interviewing my teammates and folks
from our sister teams. As I took notes, themes quickly emerged.

One was that Kibana's dependency injection system isn't available
during Jest tests, so it is often impossible to get ahold of key
dependencies. This leads to the need for writing many mocks, a
source of friction.

But another was that [Enzyme](https://enzymejs.github.io/enzyme/) (our usual tool of choice)
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

---

The cool thing about side projects is that they add a hook to your
brain. When you see something relevant to your project as you're
going about your scheduled work, you notice it in a way you
wouldn't if you didn't have an agenda.

I took my next large UI project as an opportunity to try out [React
Testing
Library](https://testing-library.com/docs/react-testing-library/intro/) (RTL)
and quickly found that it solved the fundamental
DX and trust problems of Enzyme.

Async behavior is easy to handle in RTL. It provides [great helpers](https://testing-library.com/docs/dom-testing-library/api-async/) 
very similar to those we use in our browser test runner.
You no longer have to delve into React render cycles to understand
why things are failing... if you know something is supposed to
happen, just wait for it like you do in a functional test.

If the RTL test runner can't find the element matching your query,
it pretty-prints the whole DOM with helpful messages. 

![Pretty-printed DOM](/assets/images/pretty-printed-dom.png)

Visual feedback? Check!

What's more is that RTL builds trust in the test by pushing tests
to rely on DOM interactions instead of React APIs. This makes tests
interact with the very same interface your customers do. (Read more about the philosophy [here](https://testing-library.com/docs/guiding-principles/)).

I was wowed and I quickly wrote an RFC proposing we cease and desist from using
Enzyme and we use RTL for all new Jest UI tests. It was accepted, so
I prepared a presentation to introduce the larger team to the new framework.

[Here](/assets/presentations/intro-to-rtl)'s the presentation I gave (built with
[Big](https://github.com/tmcw/big)).

