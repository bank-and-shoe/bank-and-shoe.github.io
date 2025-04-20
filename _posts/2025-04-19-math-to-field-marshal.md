
## How many games does it take to rank up in KARDS?

We might judge a general by their chance of winning a battle. Yet, as the apocryphal adage goes, “quantity has a quality all its own.” Should we favor a general with only a slight edge but many battles under their belt, or one who’s fought fewer skirmishes but wins far more consistently?

Today, we dive into the online World War II strategy card game KARDS to see what really matters for climbing the ladder: a player’s win rate or the sheer number of games played.

# The Simulation

KARDS uses a ladder system where you earn stars for wins and rank up every five stars. The ultimate goal is to reach *Field Marshal* (Rank 1).

We model rank progression as a series of weighted coin flips:

- You start at 0 points  
- You “rank up” in 5‑point increments  
- Each **heads** flip gives +1 point  
  - **(win‑streak bonus)** after two consecutive wins, each additional win grants 2 points until you lose  
- Each **tails** flip gives −1 point  
  - **(rank‑down guard)** when your score is a multiple of 5, it takes **three** losses in a row to lose a point  
- The simulation ends when you reach 20 points (climbing from Rank 5 to *Field Marshal*) or 25 points (e.g., moving from Rank 25 to Rank 20 or Rank 6 to Rank 10).  
  - In KARDS, you can’t drop below the bottom of each five‑rank bracket (Ranks 25, 20, 15, 10, 5, and 1). That means it only makes sense to model a climb of five ranks at a time (e.g., 15→10) or the final four‑rank jump (5→Field Marshal).  

For each assumed win rate (the coin’s probability of landing heads), we flip until the game ends, repeat 10,000 trials, and average the number of flips required.

For simplicity, I vary the win rate in 2.5% increments, starting from a *(rather abysmal)* 30% win rate *(which is roughly what you experience when you put War Bonds in your deck)*.

![But... but... War Bonds is card neutral!](/assets/images/sad_bonds.jpg)

# Simulation Results: the climb from Rank 5 to Field Marshal

Here’s our first graph, modeling the climb from Rank 5 to Field Marshal:  
![Games played to hit Field Marshal vs. win percentage, in 2.5% increments](/assets/images/5_to_fm_base_30_100.png)

Unsurprisingly, a low win rate barely moves the needle. At 30% wins, reaching Field Marshal takes ~28,000 games on average—technically possible, but practically infinite.  
*(I tried lower win percentages, but even at 25%, the simulation batches took over an hour, so I gave up.)*

Because those low‑win trials depend on improbable streaks, their variance is enormous. Assuming KARDS matches you against players of similar skill with a decent ranking system *(big assumption)*, let’s focus on the smoother 40%+ region:

![Games played to hit Field Marshal vs. win percentage, in 2.5% increments, starting at 40% win rate](/assets/images/5_to_fm_base.png)


In that range, games‑to‑rank‑up and win rate start to form a linear trend. I fitted linear regressions on filtered subsets of the data:

![Games played to hit Field Marshal vs. win percentage, in 2.5% increments, starting at 40% win rate, with a few linear fits](/assets/images/5_to_fm_40_plus_linear_fits.png)

So, past ~60% win rate, the relationship is essentially linear.

**Practical takeaway:**   Let's say you're choosing between the following decks to ladder with:

- **Japan/Germany aggro**: ~60% win rate, with games that average ~5 minutes
- **Soviet/U.S. midrange**: ~75% win rate, with approx. ~10 min games

Even though the midrange deck wins way more often, the faster aggro deck will usually net you a higher rank faster. 

*(Don't believe me? Jump into the end-of-month queues. You'll see more aggro than 90's Nickelodeon kids did on that crag.)*

On the flip side, small gains at lower win rates are **huge**. A 5% win-rate bump from 40% to 45% slashes expected games by two‑thirds. Even if each match takes twice as long, that win‑rate boost is worth it.

## What about the five-rank climb (like rank 10→6)?

Naturally, if we want to model a five-rank climb (like rank 25→20), the simulation is the same, but instead of collecting 20 points (=stars), you have to hit 25 points:

![games played to rank up 5 ranks](/assets/images/6_to_10_base_30_100.png)

At that threshold, lower win rates suffer even more--a 30% win rate means you are grinding hundreds of thousands of games to rank up; even at a 40% win rate, these simulations suggests ~800 games to climb those five grueling ranks.

If we zoom in on just the 40+ range, we see now our linear fits have shifted further: only around 60%+ do the data points become strongly linear:

![games played to rank up 5 ranks](/assets/images/6_to_10_40_plus_linear_fits.png)

In other words, the above analyses apply here, but shifted to the right on the X-axis: you should choose higher win-rate decks over faster-played decks unless you're alreading winning 60%+. 

## Takeways and Tips for Ranking Up!
Alright, data’s in—here’s how to rank up faster:

- **When in doubt, aggro is your out.** More reps trump tiny win‑rate gains (unless you outclass your opponents by huge margins).
  - Also, from a pure gameplay perspective, aggro can beat almost anything, especially if you're on the play. So just play it.
  - Only switch to slower decks *(running things like War Bonds or Compromise)* if you’re very confident in your skill and your meta read.
- **Exploit win‑streak bias.** You can queue during off‑peak hours to farm AI opponents and rack up easy wins.

If you don't care about hitting FM ASAP, play what you enjoy. I am, in fact, one of the sorry War-Bonding saps.

Thanks for reading!
--B&S
<!--  

## Bonus Hypothetical: how much do the bonuses and rank-guards help?

It’s clear that the star‑earning policies of KARDS are designed to help players climb the ranks—but how much do they actually boost your progress?

We can re-run our simulations under three alternative rule sets:
- **No win‑streak bonus:** each win is worth only +1 point.  
- **No rank-guard:** every loss always costs −1 point, even at the bottom of a rank.  
- **No help at all:** wins +1, losses −1 with neither bonus nor guard.  

For ease, I reduce the number of simulations to 1000 per win rate per scenario, and also only chart 40%+:

<graph>

Wowza, yeah, that's a lot of expected games at 40%. Let's zoom in on the 60% to 95% range:

<zoomed>

All this to confim a couple intuitions:
- The win streak bonus is much more useful to the rank-up process
- The rank-guard matters a lot more at lower win-rates

-->




