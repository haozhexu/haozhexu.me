---
title: "A Concise Method of Lottery Prediction"
date: 2019-09-15T15:39:00+11:00
draft: false
ads: true
categories:
  - Article
tags:
  - lottery
  - powerball
  - mathematics
  - numbers
---

# A Concise Method of Lottery Prediction

> Q: The balls are randomly picked, how could you predict?  
> A: If so, why don't people usually buy number combinations like 1, 2, 3, 4, 5, 6?  
> Q: That does not look random at all!  

In fact, the chance that the next lottery winning numbers are 1, 2, 3, 4, 5, 6, is exactly the same as every other combination of numbers, but still, people wouldn't usually buy number combinations which _look like following certain pattern_, and this is precisely how the algorithm works for prediting future winning numbers.

In the world of computing, we often hear the term "random number", surprisingly, it is extremely difficult to make a real random number generator, they are all deterministic in certain ways. There are alternative methods for generating random numbers, for example, Random.org claims to "offer true random numbers", whose randomness "comes from atmospheric noise". However, in the context of lottery winning numbers, sampling history winning numbers, we could see certain implicit rules, patterns, or __trends__, that loosely drive winning numbers in the future.

So, let's answer the simple question, why doesn't 1, 2, 3, 4, 5, 6 look random, despite its equal chance of being next winning numbers as any other number combination? To answer this question in a technically sound way, let's look at the history of PowerBall winning numbers.

```
10, 11, 20, 27, 28, 30, 31,  2
 2,  4, 14, 16, 19, 27, 29, 13
 8,  9, 11, 20, 22, 27, 32, 20
 3,  7, 10, 11, 17, 26, 35, 19
 7, 14, 21, 24, 27, 30, 35, 13
 1,  5, 11, 16, 17, 29, 30,  8
 9, 15, 22, 23, 25, 30, 35,  6
 1,  3,  9, 11, 22, 23, 24,  4
 1,  6, 11, 13, 16, 23, 27, 11
 6,  7,  8, 21, 27, 28, 34, 16
```

For the sake of this artical, above are only recent 10 draws of Australian Powerball winning numbers, ideally we should sample as many draws as possible. To figure out the implicit pattern, we need to introduce several ways of _analytical quantification_ for each winning draw:

- Sequential Pairs (SP): denotes how many pairs of sequential numbers are there in a particular draw, e.g. the draw (10, 11, 20, 27, 28, 30, 31) has an SP of 3, being (10, 11), (27, 28) and (30, 31).
- Odd Even Ratio (OE): denotes the ratio of odd and even numbers in a draw, e.g. the draw (10, 11, 20, 27, 28, 30, 31) has an OE of 3:4, there are 3 odd numbers and 4 even numbers.
- Big Small Ratio (BS): denotes the ratio of numbers that belong to upper half and lower half, within the possible number range. e.g. PowerBall has a range of 1 to 35 for its main matrix, so let's say Small range is 1-17 and Big range is 18-35, thus the draw (10, 11, 20, 27, 28, 30, 31) has a BS of 5:2
- Three Partition Ratio (3P): dividing the range of possible numbers into three partitions, for PowerBall, they are 1-12, 13-24 and 25-35, 3P denotes the number of balls that belong to each partition. For the draw (10, 11, 20, 27, 28, 30, 31), its 3P is 214.
- Head Tail Distance (HTD): the distance between the smallest number and the largest number in a single draw, for the draw (10, 11, 20, 27, 28, 30, 31), its HTD is 21.
- Arithmetic Complexity (AC): the complexity of a draw, it's calculated in such a way:

 1. Calculate the absolute difference between every two numbers in a draw.
 2. Eliminate the same numbers from result of 1, let D be the remaining number of differences.
 3. AC = D - (r - 1), r is the number of numbers in a draw

For example, every two numbers in the draw (10, 11, 20, 27, 28, 30, 31) has absolute differences below:

abs(10 - 11) = 1
abs(10 - 20) = 10
abs(10 - 27) = 17
abs(10 - 28) = 18
abs(10 - 30) = 20
abs(10 - 31) = 21
abs(11 - 20) = 9
abs(11 - 27) = 16
abs(11 - 28) = 17 *
abs(11 - 30) = 19
abs(11 - 31) = 20 *
abs(20 - 27) = 7
abs(20 - 28) = 8
abs(20 - 30) = 10 *
abs(20 - 31) = 11
abs(27 - 28) = 1 *
abs(27 - 30) = 3
abs(27 - 31) = 4
abs(28 - 30) = 2
abs(28 - 31) = 3 *
abs(30 - 31) = 1 *

By calculating the differences between every two numbers in the draw, we got 21 results in total, then we eliminate duplicate results, and end up with 15 distinct numbers, so D = 15; Since there are 7 numbers in a draw, AC = D - (7 - 1) = 15 - 6 = 9.

Apart from the methods above, there are lots of ways to analyse lottery winning numbers. The point is, by turning history winning numbers into several analytical figures, we could apply certain levels of __logical inductions__, for example:

(with a large sample of history winning numbers)

- the maximum SP a draw ever had in history is 3.
- there isn't any draw in history that has two 0 components in 3P.
- the minimum HTD in history is 16.
- the history has an AC within certain range.
- the same BS or OE has repeated in at most 3 consecutive draws in history.

Now let's look at the numbers (1, 2, 3, 4, 5, 6, 7), it has:

- 3P, 3 Partition Ratio: 100, meaning all numbers of this draw fall into the first partition.
- BS, Big Small Ratio: 0:7, meaning all numbers of this draw falls into Small range.
- HTD, Head Tail Distance: 6

Now, (1, 2, 3, 4, 5, 6, 7) does look __evidently__ not random, it is too far away from the implicit patterns of history winning draws, just like someone picked it by hand!

And using the rule of __logical induction__, we could potentially filter out number combinations that are _unlikely to be the next winning draw_. For example, if the previous three winning draws all have Odd Even Ratio (OE) of 2:5, given that "the same BS or OE has repeated in at most 3 consecutive draws in history", we could say that the next draw is less likely to maintain the same OE. After applying each induction rule, we could effectively eliminate a large number of combinations which do not follow history trend.

Unfortunately, even after elimination of less likely numbers, permutating the remaining numbers can still get you a long list of draws, the cost of buying all the draws is usually higher than the 1st prize. In this case, we could apply something call Rotation Matrix, to further filter out numbers that are less likely to be winning number. I'll talk about this when I have time in the future.
