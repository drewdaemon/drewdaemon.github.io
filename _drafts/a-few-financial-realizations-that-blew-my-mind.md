---
layout: post
title: Some financial truths that blew my mind
date: 2024-03-31 15:41 -0600
---


I like learning about personal finance. I would even call it a hobby.

Most of the time my learning is a gradual process but, once in awhile, I have a realization that really expands my worldview. Sometimes it's an exciting experience and other times it's terrifying.

Here are a few of those realizations.

## "Home equity" is a lie

This is a bit of an exaggeration. But you'll soon see that it's close to the truth. 

"Equity" is most often used to denote a share in something—that is, co-ownership. For example, if I have equity in a business, I _own_ part of the business.

For a long time, my subconscious self applied this to mortgages. I thought of it like this: you start with a 20% down payment so you own 20% of your home. Your mortgage payments gradually increase this number over 15-30 years until you finally own 100% of the property.

<div style="width: 100%" id="chart"></div>

So, the bank starts with 80% and slowly sells you its share.

How wrong I was.

### The big reveal

The bank owns _0%_ of your home.

Let me restate that in case you missed it: **the bank doesn't own any of your home**.

So how much of your home do you own? 100%. Starting on day one.

### Why it matters

Isn't this splitting hairs? After all, if you owe the bank a sum of money equal to 80% of the value of your home, it's not like selling your home will net you more than 20% of the home's value. The end result is the same as if the bank actually owned 80%.

Well, that's true during a sale. But this distinction _definitely_ matters when it comes to changes in the market.

The value of your home isn't static. It can go up and down, just like a stock. But the startling truth is that you will (respectively) reap the rewards and suffer the consequences _alone_.

The first time I was misled about the nature of home "equity" was when I read an article which claimed that making a lower down payment was a good way to take less risk in buying a property. The author seemed to imply that making a 3% down payment exposed you to less risk than making a 20% down payment.

If home "equity" meant co-ownership with the bank, this would be true. In that (imaginary) scenario, a smaller down payment would mean that you would own a smaller share of the home. Owning a smaller share of the home would mean you would take a smaller hit if the home became less valuable.

But remember, that isn't how this works. You own 100% of the property on day one, regardless of the size of your down payment.

In other words, you don't owe the bank less money if your home becomes worthless. And you don't owe the bank more money if your home becomes worth millions. 

Folks who bought property before the 2008 financial crisis suffered 100% of the losses. On the other hand, those who bought property in Utah before COVID-19 reaped 100% of the unbelievable gains.

The bank is just a lender. When it comes to swings in the housing market—for better or for worse—you're on your own.

## Inflation compounds

## Losses hurt more

<script src="https://cdn.jsdelivr.net/npm/vega@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-embed@6"></script>

<script>

// Define the data for the chart
const data = {
  values: [
    { x: 1, y: 10 },
    { x: 2, y: 20 },
    { x: 3, y: 15 },
    { x: 4, y: 25 },
    { x: 5, y: 30 }
  ]
};

const spec = {
  $schema: 'https://vega.github.io/schema/vega-lite/v5.json',
  data,
  width: 'container',
  mark: 'line',
  encoding: {
    x: { field: 'x', type: 'quantitative', title: 'X Axis' },
    y: { field: 'y', type: 'quantitative', title: 'Y Axis' }
  }
};

// Render the chart in the container element
vegaEmbed('#chart', spec);
</script>


