# ARCathon 2022


## Iteration 1

[Docker image: 2022-12-31T19-47.tar](2022-12-31T19-47.tar)

The docker image had the wrong architecture. It was supposed to be `amd64`. I had submitted `arm64`.


## Changes between iteration 1 and iteration 2

Inserted `--platform=linux/amd64` into the `Dockerfile` and resubmitted. This fixed the architecture problem.


## Iteration 2

[Docker image: 2022-12-31T23-56.tar](2022-12-31T23-56.tar)

Yay, I got a [3rd place](https://lab42.global/past-challenges/arcathon-2022/). Score 0.97 and 3% of the 100 secret tasks.

This docker image contains both `amd64` and `arm64`.
