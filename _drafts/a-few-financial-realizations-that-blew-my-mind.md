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

Compound interest is one of the most exciting concepts in personal finance. You put a dollar in an investment. You get a 10% return the first year, meaning you earn 10 cents! Now, you have $1.10 in your investment for the second year. The second year, you get another year of 10% returns. Now, this year, you made 11 cents, one cent more than last year.

When this happens consistently over many years, the growth of your money is not linear, it is exponential. In other words, your money isn't just growing at a steady rate but is rather in a constant state of acceleration. What could be more magical?

<div id="compound-interest"></div>

I felt like death the day I realized that inflation acts the very same way.

You've probably heard that inflation is measured as a percentage. Well, if a pair of shoes costs $100 this year and we have a year of 6% inflation, that pair of shoes will cost $106—six dollars more. But that becomes the new baseline. Let's say the next year is another 6% inflation. That pair of shoes doesn't gain just six dollars, but rather $6.36.

<div id="compound-inflation"></div>

Obviously, this is an oversimplification. The price of a particular good or service may go up or down year-to-year based on myriad factors. But, measured in aggregate, inflation does compound.

### Why is it so?

For the longest time I wondered how this could be. Inflation is just a measurement of how sellers price their products. Why wouldn't price growth be linear? That didn't seem to violate any fundamental principle of mathematics.

I searched all over for an answer and realized that it was there in my own human psychology.

Most of us can remember when $1000 felt like an amazingly huge amount of money. When I was a kid I thought that you could do basically anything with $1000. Why did this amount seem so big (_besides_ the fact that it actually _did_ go way further back then)? Because I had very little money. _Relative_ to what I had at the time, $1000 was very large.

I still think $1000 is a lot of money, but it definitely seems smaller against the backdrop of my adult net worth.

There's an observed financial behavior which is rooted in the same principle of proportionality. It's called the "wealth effect." The observation is that as our perceived wealth increases (e.g. a retirement account grows in value), we become freer with our spending. Again, against the backdrop of eye-watering gains in a 401k, $100 dollars here and there seems so much smaller than it once did.

In other words, as we gain elevated baselines of wealth, fixed units of money feel less valuable.

My best theory to date is that it is this very same relative thinking that supports a compounding pattern for inflation. If I'm selling a piece of candy for $1.00, raising the price by $5.00 (a whopping 500%) feels excessive and I will probably lose customers. But, if I'm selling a cellphone for $100, consumers are much more likely to accept the same fixed price increase of $5.00 because, relative to the current price, it's only 5%.

The next year, I can apply the same reasonable logic and raise the price by 5%. But, here we see the same effect as before. A 5% increase now means not $5.00, but $5.25.

By now you should know what happens next.

### Why it matters

The Federal Reserve targets an inflation rate of approximately 2% per year.<sup>[<a href="#footnote-1">1</a>]</sup>

Even at that modest rate of acceleration (whenever we get back to it 😅), it threatens your future financial wellbeing. It's a mathematical fact that an exponential curve will always catch up with a straight line, no matter its slope. If you are saving at a constant rate without fighting back through compounding investments, you are going to feel the pinch in the long run.

## Losses hurt more

## Footnotes
1. <span id="footnote-1"><a href="https://web.archive.org/web/20240327051648/https://www.federalreserve.gov/faqs/economy_14400.htm">Federal reserve website; retrieved from Wayback Machine</a></span>

<script src="https://cdn.jsdelivr.net/npm/vega@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite@5"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-embed@6"></script>

<script>
const data = [
    {
        "year": 1,
        "totalPaid": 5901.461382269958,
        "remainingBalance": 394098.53861772997
    },
    {
        "year": 2,
        "totalPaid": 12104.852729132608,
        "remainingBalance": 387895.14727086725
    },
    {
        "year": 3,
        "totalPaid": 18625.621350603877,
        "remainingBalance": 381374.3786493959
    },
    {
        "year": 4,
        "totalPaid": 25480.00487039727,
        "remainingBalance": 374519.9951296025
    },
    {
        "year": 5,
        "totalPaid": 32685.07165987257,
        "remainingBalance": 367314.9283401272
    },
    {
        "year": 6,
        "totalPaid": 40258.763340662066,
        "remainingBalance": 359741.2366593377
    },
    {
        "year": 7,
        "totalPaid": 48219.93946181183,
        "remainingBalance": 351780.06053818803
    },
    {
        "year": 8,
        "totalPaid": 56588.42446269037,
        "remainingBalance": 343411.57553730946
    },
    {
        "year": 9,
        "totalPaid": 65385.05703860866,
        "remainingBalance": 334614.9429613911
    },
    {
        "year": 10,
        "totalPaid": 74631.74203207923,
        "remainingBalance": 325368.2579679206
    },
    {
        "year": 11,
        "totalPaid": 84351.50497893029,
        "remainingBalance": 315648.4950210695
    },
    {
        "year": 12,
        "totalPaid": 94568.54944510279,
        "remainingBalance": 305431.450554897
    },
    {
        "year": 13,
        "totalPaid": 105308.31729690674,
        "remainingBalance": 294691.68270309304
    },
    {
        "year": 14,
        "totalPaid": 116597.5520548182,
        "remainingBalance": 283402.44794518163
    },
    {
        "year": 15,
        "totalPaid": 128464.36548857685,
        "remainingBalance": 271535.63451142295
    },
    {
        "year": 16,
        "totalPaid": 140938.30761941502,
        "remainingBalance": 259061.69238058478
    },
    {
        "year": 17,
        "totalPaid": 154050.44030373383,
        "remainingBalance": 245949.55969626596
    },
    {
        "year": 18,
        "totalPaid": 167833.41458145945,
        "remainingBalance": 232166.58541854034
    },
    {
        "year": 19,
        "totalPaid": 182321.55198168862,
        "remainingBalance": 217678.44801831117
    },
    {
        "year": 20,
        "totalPaid": 197550.92998808486,
        "remainingBalance": 202449.07001191494
    },
    {
        "year": 21,
        "totalPaid": 213559.47187684663,
        "remainingBalance": 186440.52812315317
    },
    {
        "year": 22,
        "totalPaid": 230387.04115095673,
        "remainingBalance": 169612.95884904306
    },
    {
        "year": 23,
        "totalPaid": 248075.5408058666,
        "remainingBalance": 151924.4591941332
    },
    {
        "year": 24,
        "totalPaid": 266669.017673802,
        "remainingBalance": 133330.9823261978
    },
    {
        "year": 25,
        "totalPaid": 286213.7721065211,
        "remainingBalance": 113786.22789347869
    },
    {
        "year": 26,
        "totalPaid": 306758.47326965065,
        "remainingBalance": 93241.52673034924
    },
    {
        "year": 27,
        "totalPaid": 328354.28033569886,
        "remainingBalance": 71645.71966430102
    },
    {
        "year": 28,
        "totalPaid": 351054.96987753373,
        "remainingBalance": 48945.03012246602
    },
    {
        "year": 29,
        "totalPaid": 374917.069779553,
        "remainingBalance": 25082.930220446753
    },
    {
        "year": 30,
        "totalPaid": 400000,
        "remainingBalance": 0
    }
];

const spec = {
  $schema: 'https://vega.github.io/schema/vega-lite/v5.json',
  data,
  width: 'container',
  mark: 'line',
  encoding: {
    x: { field: 'year', type: 'quantitative', title: 'X Axis' },
    y: { field: 'totalPaid', type: 'quantitative', title: 'Y Axis' }
  }
};

// Render the chart in the container element
vegaEmbed('#chart', spec);
</script>


