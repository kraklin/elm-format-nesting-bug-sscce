# Elm-format nesting SSCCE

This repsitory contains SSCCE for a bug found in `elm-review`.
Install everything with 

```
npm install
```

## Problem description

We have found out that once you have a deeply nested lists (cca > 10), elm-format starts to be exponentialy slow. It was discovered by one of our engineers who put deeply nested HTML structure to our code and then `prettier-plugin-elm` throws errors because `elm-format` was taking longer than 15s.

I have created two files to demonstrate the problem.

The problematic part is in `src/Problem.elm`. If you ran

```
time npx elm-format --validate src/Problem.elm
```

it will take `elm-format` roughly 20s to finish. See times below for different nesting levels.

```
nesting depth - time
----------------
4 - 0.36s
5 - 0.36s
6 - 0.36s
7 - 0.40s
8 - 0.40s
9 - 0.43s
10 - 0.50s
11 - 0.66s
12 - 0.94s
13 - 1.56s
14 - 2.73s
15 - 5.11s
16 - 10.21s
17 - 19.79s
18 - 43.57s
```

## Possible way around

To get out of the problem, split your structure to separate functions to prevent deep nesting. 

There is second file `src/SplitProblem.elm` which has the same exact structure, but it is split in half to different function call, so the nesing is not that deep. This works quite nicely and is checked roughly in 0.56s.

```
time npx elm-format --validate src/SplitProblem.elm
```
