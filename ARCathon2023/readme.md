# ARCathon 2023


## Changes between ARCathon 2022 iteration 2 and ARCathon 2023 iteration 1

Cleaned up all the hardcoded messy code that I put together days before the 31-dec-2022 deadline.
It took around 1 month to clean up. Lesson technical debt takes ages to clean up.

40 hardcoded mutations was tried out of each existing solution. It would not continue after that.
Now the executable continues to accumulate mutations until CTRL-C is pressed.


## Iteration 1

[Docker image: 2023-02-13T21-40.tar](2023-02-13T21-40.tar)

The docker image had the wrong architecture. It was supposed to be `amd64`. I had submitted `arm64`.

The score was lower than my previous score. 
At the end of ARCathon 2022 where I got 3rd place with score 0.97 and 3% of the 100 secret tasks.
However now I'm getting the score 0.98 and 2% of the secret tasks.

Idea: The initial seed for the random generator was leading to more discoveries.

Idea: My code improvements that I have been doing throughout january 2023 has worsened the code.
I have measured time, and some of the solutions are indeed slower, taking around 90 seconds to run.
It could only explore few mutations.


## Changes between iteration 1 and iteration 2

The docker image now stops running after 23h30m, so it doesn't cross the 24 hour mark.

I have optimized the slowest programs, so more mutations can be explored.
The worst solutions are now taking around 20 seconds.

Rewrote my notes about how to generate the docker image, and I deleted the docker container, docker images, buildx instances,
and regenerated it from scratch.


## Iteration 2

[Docker image: 2023-02-26T13-03.tar](2023-02-26T13-03.tar)

The docker image had the wrong architecture. It was supposed to be `amd64`. I had submitted `arm64`.
I guess it's because I rewrote my notes about regenerating the docker image, and thus messed up the docker image's architecture.

Score 0.98 and 2% of the secret tasks.

Idea: If it has been running with the wrong architecture, it may have been doing emulation, and thus running terrible slow.
This could explain why few mutations got explored.


## Changes between iteration 2 and iteration 3

I have removed the `arm64` platform entirely from the scripts that creates the docker image. 
Now the docker image should only contain this platform: `amd64`. 
The `loda-rust arc-competition` now prints out the architecture, so it's possible to verify what configuration is running.

Immediately after I had submitted I realized that I had forgotten to run the important `rake payload` command.
The `/root/loda-arc-challenge/programs` dir contains the same files as the repository `loda-arc-challenge`.
The `/root/loda-arc-challenge/solutions.csv` file is identical to the same file in the repository `loda-arc-challenge`.
The `/root/.loda-rust` may be slightly different than my `~/.loda-rust` dir.
Next time I must remember to run the `rake payload` command.


## Iteration 3

[Docker image: 2023-02-27T21-26.tar](2023-02-27T21-26.tar)

Finally, the docker image now has the right architecture. It's `amd64`.

The score has jumped from `score 2` to `score 4`. This is my new high.


## Changes between iteration 3 and iteration 4

I have added these solutions: `0dfd9992`, `29ec7d0e`.


## Iteration 4

[Docker image: 2023-03-02T22-37.tar](2023-03-02T22-37.tar)

It's still running the right architecture. It's `amd64`.

The score has dropped from `score 4` to `score 2`. 

Ideas what may be causing the score to drop.
 - Starting with a different random seed.
 - Applying mutations over and over. If a new solutin has been added. This impacts what mutations gets applied.
 - My newest code are causing the miner to run slower, so it explores fewer mutations.
 - My newest code does terrible predictions, and finding false-positive solutions.


## Changes between iteration 4 and iteration 5

Replaced the fuzzy `ImageRepairTrigram` algorithm that didn't do well with bigger repeating patterns.
Replace with the new `ImageRepairPattern` that better deals with big the mosaics.


## Iteration 5

[Docker image: 2023-03-05T16-31.tar](2023-03-05T16-31.tar)

This got `score 2`. It didn't change the score.

Ideas why `score=2`.
 - The existing programs solve 2 of the secret puzzles.
 - None of the mutations solve any of the secret puzzles.
 - The initial random seed may be lucky and a few puzzles may get solved.
 - The new repair algorithm is slow, that a smaller search space is explored.


## Changes between iteration 5 and iteration 6

Now it's always the same random seed that gets assigned in each mutation.
Running the docker container under the same conditions, should now generate the same output over and over.
It's still non-deterministic. Change to the solutions repository impacts the analytics files, and impacts the mutations.
Previously the initial random seed was based on datetime, thus the results could not be reproduced.

Improved performance. 
Added `AssertFunction` that throws an exception if a precondition isn't satisfied.
Modified the slowest of the existing solutions to make use of the `AssertFunction`, so that less time is wasted on.

On my mac running `loda-rust arc-competition` for 24 hours, then 480 mutations gets tried out.
This is higher than previously. In ARCathon 2022 my code only did 40 mutations.


## Iteration 6

[Docker image: 2023-03-09T17-09.tar](2023-03-09T17-09.tar)

This got `score 2`. It didn't change the score.

Ideas
 - The existing programs solve 2 of the secret puzzles.
 - None of the mutations solve any of the secret puzzles.
 - The initial random seed was unlucky, and didn't find any new solutions. Try pick another initial seed.
 - Due to the new assertions in solutions, it may reject otherwise good mutations by accident.

My thoughts about initial random seed. It shouldn't depend on getting lucky with picking a random seed. It should be able to solve puzzles even with the worst initial random seed.

