---
layout: post
title: Always Pick C
comments: true
---

**"You're taking a test and you don't know the answer to the last 5 questions. Do you choose C for all of them or do you guess randomly?"**

Statistically, this line never seems to work to start conversations at bars. Luckily, your chances of getting the problems on the test correct are a little better. Does always picking C (equivalently, always picking A/B/D) vs guessing randomly even make a difference? It's a simple enough question. In discussions with several friends, most people fall into one of two camps. Imagine you had 8 questions left, with choices A,B,C, and D. Consider the following two viewpoints.
	
1. It doesn't matter either way -- your chance of getting any problem right in either case is just 25% for each problem. Both strategies have exactly the same results.

2. While you have a 25% chance of getting any problem right in both cases, the strategy of picking C every single time has lower variance. That is, if you picked randomly, you'll have a chance to get a 50% on the test (though it's exactly the same chance that you'll get a 0% on the test). If you get really lucky, you have a chance to get 100%! Meanwhile, putting down C always you can expect to get around 25% of the questions right pretty consistently. In that sense, always picking C is less risky.

I <a href="https://github.com/sandeepba/stats-busters/blob/master/always-pick-c/always-pick-c.py"> simulated </a> trying both the "always pick C" and the "guess randomly" approaches just to confirm. Over 100000 tests, with 100 questions each and 4 choices, it turns out that camp #1 is absolutely right -- it doesn't matter. The distributions are shown below (the purple is the overlap). Since the whole graph is purple, it appears that the distributions are actually identical. 

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sandeepba/sandeepba.github.com/master/assets/always-c-sim.gif" /></div>

The reasoning is that if the teacher's answer key is truly picked randomly, [C,C,C,C,C] is just as likely as any other specific order of choices like [D,B,B,C,A]. Your chance of getting 100% with putting down all C is exactly the same as the chance of getting 100% when you pick random answers -- it just "feels" less likely that a teacher would give an exam with a 100 C's in a row rather than any specific other combination of letters. It's similar to the  question, which is more likely from 3 coin flips: HHH or HTH? All 8 possible outcomes are equally likely (HHH, THH, HTH, HHT, HTT, THT, TTH, TTT). While it's true that there is only one with 3 heads but 3 outcomes with 2 heads and 1 tails, any specific arrangement of those 2 heads and 1 tails (HHT, HTH, THH) has an equal chance of coming up as (HHH).

So theoretically, with a teacher effectively randomizing answer choices, your strategy for dealing with questions you don't know doesn't matter (between guessing and always choosing the same letter). Realistically, most teachers won't ever make all answers C. Someone designing a test might glance over the answer key, see 7 C's in a row, and think to themselves, *hmm, this isn't random enough. Let's mix things up by making a few of them different letters*. Does this change things? 

Let's make our teacher generate answer keys using "realistic" randomness assumptions. Suppose the answers are random but with a twist -- the teacher will never make more than 3 answers in a row as the same choice. So what's your best play now? I re-ran a similar simulation. This time, I had tried strategies:

* **Always C** - self explanatory 
* **Pseudorandom** - guessing randomly, but with the same rules as the teacher -- only 3 of the same answers in a row ever
* **Random** - guessing totally randomly

results below.

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sandeepba/sandeepba.github.com/master/assets/4q2rep.png" /></div>

 Neat! All 3 strategies have the same mean performance, so you can expect to get 25% right in all 3 cases. However, the "always guess C" strategy clearly has a skinnier distribution. In this sense, it's "safer" than the other two strategies, as you are "guaranteed" to get pretty close to 25%. In comparison, the other two random strategies have more variation in the scores you could get -- you might get a 20% or 30% more often than you would get with just guessing C always. The two random strategies are pretty close to each other -- are they actually the same? 

 To get some separation, we can push the test to a sort of extreme case -- 2 choices (for example, true / false), with a maximum of 2 of the same answer in a row. While a max of 2 of the same answer in a row might be a little unrealistic, it does lead to some more clarity in the way the strategies differ. Note here -- I'm keeping the "Always C" out of convenience as a description of the constant strategy, but with 2 choices there isn't necessarily a C to pick! 

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sandeepba/sandeepba.github.com/master/assets/2q2rep.png" /></div>

As you can see, you can expect to do equally well with any of these strategies. However, if you're feeling lucky, guessing randomly or guessing pseudorandomly gives you a better chance to get a higher score than always guessing C would. Unfortunately, you're also just as likely to get an equally low score if luck's not going your way. So what's the verdict? **In the real (pseudorandom) world, always guessing C is the safest option. In the perfectly random world, it doesn't matter which strategy you use.**  If you want to expose yourself to a little more risk (a chance for a slightly higher/lower score than 25%), you can guess randomly. However, the more answer choices you have and the more actually random the test answer key is, the less it ends up mattering, as the answer key will look more and more like the perfectly random case where the distributions are the same. 
