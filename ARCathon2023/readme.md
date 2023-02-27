# ARCathon 2023


## Changes between ARCathon 2022 iteration 2 and ARCathon iteration 1

Cleaned up all the hardcoded messy code that I put together days before the 31-dec-2022 deadline.
It took around 1 month to clean up. Lesson technical debt takes ages to clean up.

40 hardcoded mutations was tried out of each existing solution. It would not continue after that.
Now the executable continues to accumulate mutations until CTRL-C is pressed.


## Iteration 1

[Docker image: 2023-02-13T21-40.tar](2023-02-13T21-40.tar)

The docker image had the right architecture.
However the score was lower than my previous score. 
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

The docker image had the wrong architecture. It was supposed to be amd64. I had submitted arm64.
I guess it's because I rewrote my notes about regenerating the docker image.

Score 0.98 and 2% of the secret tasks.

Idea: If it has been running with the wrong architecture, it may have been doing emulation, and thus running terrible slow.
This could explain why few mutations got explored.

