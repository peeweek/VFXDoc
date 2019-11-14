# Random

Randomness is a way to generate non-humanly predictable behavior based on a mathematical computation. It is often mistaken with a shuffle pattern that is one way of randomizing data.

## General Concepts

Random functions are mostly determined by a seed, a polynomial function, an amplitude and a frequency.

### Seed

The seed is a parameter of the random function that will influence the degree of uniqueness of the random. 

Simple randoms based on an integer seeds are made to be reproducible and every random sequence for a given seed will be the same every time the random function is called.

### Sequences and distribution

Sequences of random numbers are the result of a series of queries on the random function. Every time the random function is queried it will return one data (for instance a number).

For example, querying 44 times a random function that returns a number between 0 and 9 can return:

`04714639195729083463719501001475934282957482`

This sequence is supposed to return the same proportion of 0's, 1's, 2's. However, in our case we have the following counts:

| 0     | 1     | 2    | 3    | 4     | 5    | 6    | 7    | 8    | 9     |
| ----- | ----- | ---- | ---- | ----- | ---- | ---- | ---- | ---- | ----- |
| 5     | 5     | 3    | 4    | 6     | 4    | 2    | 5    | 3    | 6     |
| 11.3% | 11.3% | 6.8% | 9%   | 13.6% | 9%   | 4.5% | 11.3 | 6.8% | 13.6% |

If we did expect a count per value it should be 44 items divides by the amount of different items : 10 so `44/10 = 4.4` that would lead to a 10% chance for every item.



<u>Now if we query this same sequence, but 440 times instead of 44:</u>

`04714639195729083463719501001475934282957482509971420504698123371404605992928069829629973179976943453937628771182332958458932138312133335111746117354244028576025492652086387071912620487365247800801886304683691622758132580181295524331111133770987612698983506007917757921360848563977877676311624278342846246992361296948422263311018303524652676824418509528129597896602548139511391896808330591148621357603557481094342007970316073151562650088303`

Now if we redo the counting of occurrences of every number we obtain the following:

| 0    | 1    | 2     | 3     | 4    | 5    | 6    | 7    | 8     | 9     |
| ---- | ---- | ----- | ----- | ---- | ---- | ---- | ---- | ----- | ----- |
| 41   | 53   | 48    | 50    | 37   | 37   | 42   | 41   | 45    | 46    |
| 9.3% | 12%  | 10.9% | 11.3% | 8.4% | 8.4% | 9.5% | 9.3% | 10.2% | 10.4% |

While this is still not perfect, we can see the proportion of every number has slightly evolved so every proportion tends to be closer to 10% this time.

This is actually because any random distribution will ***tend*** to any kind of percentage as the amount of data increases.

#### Determinism

Determinism is the way of having a random pattern to be reproduced identically. It is mostly determined by the seed of the function and its parameters. 

We call a function *deterministic* when it returns an absolute exact sequence every time it is queried. Most of the time, it involves 