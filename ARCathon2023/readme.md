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


## Changes between iteration 6 and iteration 7

Previously I have been using `arc_json_model` as the data model for mining.
Which required converting json data to Image, over and over.
Now I have introduced a new the data model named `arc_work_model`, where the images are preloaded.
I doubt this will impact performance.

Bumped the initial random seed from 1 to 2.

## Iteration 7

[Docker image: 2023-03-18T19-16.tar](2023-03-18T19-16.tar)

This got `score 2`. It didn't change the score.

This confirms that the new model `arc_work_model` hasn't broken things.

Ideas
 - The existing programs solve 2 of the secret puzzles.
 - None of the mutations solve any of the secret puzzles.
 - The initial random seed was unlucky, and didn't find any new solutions. Try pick another initial seed.
 - Start making use of the new `arc_work_model`.

## Changes between iteration 7 and iteration 8

Introduced a `Time limit` for how long a program is allowed to run. If it runs longer, then the program is discarded. The initial time limit is 200ms.
In the past there have been programs that was taking `15` seconds, severely limiting the number of mutation that was possible to explore.

The new `arc_work_model` now makes predictions about `output size` and `output palette`.
However I don't yet expose this data to the `.asm` programs.

I have updated all the `.asm` programs, from using 10 registers per pair, there are now 100 registers per pair.
This is preparation for accessing the data from `arc_work_model` from within the `.asm` program.

The `RunWithProgram.process_computed_images()` now invokes a `postprocess()` function that attempts fixing minor issues.
- If all the images have swapped `width` and `height`, then it rotates the image.
- If all the images creates the correct output but with the wrong palette, then the palette is `recolored`.
- If all the images are flipped, then it tries `flip_x`, `flip_y`, `flip_xy`.

Added 15 solutions.

Bumped the initial random seed from 2 to 3.

## Iteration 8

[Docker image: 2023-03-24T21-24.tar](2023-03-24T21-24.tar)

This got `score 2`. It didn't change the score.

This confirms that my extensive changes to the new model `arc_work_model` hasn't broken things.

This confirms that increasing the gap from 10 to 100 worked out fine. This means that I can proceed inserting metadata into the initial state of the program.

The newer code generates more false positives. Potentially outputting junk for the tasks that otherwise would be solvable.
False positives has been a problem all the way back to the beginning. I think it's particular bad at the moment.
The `postprocessing` made the predictions slightly worse.
One way to mitigate this is to use the predicted output size and the predicted color palette, and reject the results that doesn't satisfy the predicted data.

Ideas
 - Bridge basic data such as `predicted width` + `predicted height` to the `.asm` programs.

## Changes between iteration 8 and iteration 9

The `RunWithProgram` is now providing the programs with the first metadata ever. These are:
- `PredictedOutputWidth`
- `PredictedOutputHeight`
- `OutputImageIsInputImageWithChangesLimitedToPixelsWithColor`

The `OutputImageIsInputImageWithChangesLimitedToPixelsWithColor` is in particular useful. And is being used by the new `repair-mosaic.asm`.

Mask operations with: `xor`, `and`, `or`, is now possible.

Added 5 solutions.

Removed 3 solutions, that was replaced by the new `repair-mosaic.asm`.

The `PopularObjects.find_objects()` is now deterministic. Previously it did a sort on the mass of the object, but didn't consider the scenario where multiple objects have the same mass, and was ordering these objects in random order, leading to programs that behaves correct one moment, and behaving incorrect another moment.

False positives. I have been investigating what is going on, and written a list of things to suppress false positives.
Sometimes a candidate solution would get approved, despite none of the training pairs were correct.
This was partially due to `PopularObjects.find_objects()` being non-deterministic.
The `RunWithProgramResult` now has a `all_train_pairs_are_correct`, so it's no longer up to the caller to determine if a candidate solution is good.

Bumped the initial random seed from 3 to 4.

I did various experiments with identifying cellular automata parameters, but didn't find a reliable approach. So there are no `.asm` programs that does this.

## Iteration 9

[Docker image: 2023-04-02T12-22.tar](2023-04-02T12-22.tar)

Yay, this got `score 4`. This is the same as my previous highest score. My previous highest score was when the code was behaving much more non-deterministic and where I assigned the initial random seed to the unix timestamp. With the current iteration the code is more deterministic, so it should yield the same output when running the code again.

In between iteration 7 and iteration 8, I added a mechanism that rejects candidate solutions that takes too much time. I was concerned that it would reject otherwise good solutions. The rejection time is 200ms. It seems to be a fine time limit.

My thoughts
- Does the program crash, or does it run all the way to the end without crashing.
- Are all the solutions found while traversing the existing solutions. Are some of the solutions found while mutating the existing solutions.

## Changes between iteration 9 and iteration 10

Didn't touch the initial random seed. It's still 4.

In this iteration I wanted to enumerate the objects that was detected. However there are too many grid tasks, that was causing the object detection to mess up. I have to do a grid detector first, and afterwards an object detector. So I abbandoned the object enumeration and instead switched focus to symmetry detection.

Symmetry detection. All the input images are analyzed for these symmetries:
- Horizontal
- Vertical
- Diagonal A
- Diagonal B

The symmetry can be partial. There are several tasks where the pixels have been obscured, and the job is to repair the obscured pixels. If the majority of pixels are symmetric, then it gets detected.

The symmetry can be with an inset. There are several tasks where there is symmetry that isn't perfectly aligned in the center. These now gets identified as being symmetric.

Diagonal symmetry. It doesn't try out different sizes of the square. It's always the `min(width, height)`. In the future I may want to implement this.

The new symmetry code cannot detect rotational symmetry. In the future I may want to implement this.

There is now `pair.input.repair_mask` and `pair.input.repaired_image`, so that the asm programs doesn't have to do the repair.

Added `repair-symmetry.asm` and `repair-symmetry-crop.asm`, that solves a handful of tasks. 

There are several tasks that have some kind of symmetry, that cannot be solved.

Thoughts:
- Detection of rotational symmetry may help solving more tasks of this kind.
- Inset together with diagonal a/b symmetry. There may be a few tasks where this is useful.
- Detection of symmetry in the output for training pairs. Determine if there a correlation between input symmetry and output symmetry.

## Iteration 10

[Docker image: 2023-04-13T13-10.tar](2023-04-13T13-10.tar)

This got `score 4`. It didn't change the score.

Thoughts:
- There seems to be no `easy to repair symmetry` tasks in the private dataset.

## Changes between iteration 10 and iteration 11

Didn't touch the initial random seed. It's still 4.

I have been working on grid detection. In 67 tasks a grid can be detected. I have spotted 1 of the tasks where the detected grid has the wrong properties.
There are edge cases where a grid is detected, where it would be a better fit detecting objects.

- It can detect grids with a few mismatches in the grid structure.
- It can detect grids where the lines thickness varies.
- It can detect grids where the lines have different colors across the input pairs.
- It can only detect grids when there is the same spacing for all cells. It cannot detect grids with weird spacing.
- It cannot detect horizontal/vertically stacked images that are separated by color. Example 5 cells wide and 1 cell tall, with a red separator line.
- The grid must contain 2 or more cells. There are several tasks that have 1 cell but these yield some terrible grid detections, so I avoid those.

Now passing a `GridMask`, `GridColor`, `EnumeratedObjects` to the LODA programs.

I have added 3 new solutions to the repository. With so few new programs, I don't expect this to discover any new solutions.
So if stays on `score 4` then I'm satisfied.

Thoughts:
- Detect horizontal/vertical stacks of images. There are many ARC tasks with this pattern.
- Detect objects and do manipulations with objects. So far there are only a few solutions that does this.
- Send the gathered info to a neural network and see if it helps.
- I need more tools for manipulating objects.

## Iteration 11

[Docker image: 2023-04-25T13-50.tar](2023-04-25T13-50.tar)

This got `score 4`. It didn't change the score.

I'm glad that that I have been able to add several new solutions without worsening the score. Adding new solutions impacts the analytics data used for 
mutating programs, so the same solution may no longer be discovered when using newer analytics data.

I want to detect objects. This has not been possible in the past, because the mosaic patterns and grid patterns was causing more noise than signal.
Now that I have detectors for grid and symmetry. If it's none of those, then I can try detect the objects.

Thoughts:
- There seems to be no `easy to solve grid` tasks in the private dataset.

## Changes between iteration 11 and iteration 12

Didn't touch the initial random seed. It's still 4.

A few new cases where the `EnumeratedObjects` is populated with data.

Added `crop_first_object.asm` that solves a few tasks where there are multiple enumerated objects.
The enumerated objects can be ordered by: mass, symmetry, asymmetry.

With only 1 new solution added to the repository that solves a few simple tasks. I don't expect this to discover solutions for advanced tasks.
So if stays on `score 4` then I'm satisfied.

I did proof of concept with an RNN, but the predictions were too poor. I may try out LSTM or transformer.

Began experiments with an alternative solution using a 3x3 convolution. Work in progress.
I'm not sure how to populate the model, nor how to extract data from the model.

Thoughts:
- Solve more tasks about objects.
- Try out LSTM or Transformer.
- Continue with the 3x3 convolution experiment.

## Iteration 12

[Docker image: 2023-04-30T11-50.tar](2023-04-30T11-50.tar)

Wow, this got `score 6`. This is huge jump from my previous `score 4`. Awesome!

## Changes between iteration 12 and iteration 13

Didn't touch the initial random seed. It's still 4.

Added `SubstituteRule` for determining if a task can be solved by a search/replace with 1 pattern.
It solves several of the tasks in the ARC 1 public dataset. I doubt that it will solve any tasks in the private test set, since it's a simple action. I imagine the private test set to contain advanced tasks that requires object manipulation.

Success criteria. If it continues with `score 6`, then I'm happy.

Thoughts:
- Solve more tasks about objects.
- Try out LSTM or Transformer.
- Continue with the 3x3 convolution experiment.

## Iteration 13

[Docker image: 2023-05-03T23-58.tar](2023-05-03T23-58.tar)

This got `score 6`. Great it says on the same score as previously.

The `SubstituteRule` didn't impact the score. I'm keeping the code, since it solves several tasks in the public dataset.

## Changes between iteration 13 and iteration 14

Didn't touch the initial random seed. It's still 4.

I have lowered the duration that it runs, so it now runs for 10 hours and then stops. Previously it ran for 23h30m. It would take around 2-3 days before I got feedback.
I'm wondering if most of the solutions gets discovered within the first few minutes. This way I can submit before I go to sleep and hopefully see a result next day. 

Success criteria. If it continues with `score 6`, then I'm happy.
- If the score drops, it may be due to the short 10 hour time-duration. I may have to rollback to the 23h30m.
- If the score drops, it may be due to the newly added solutions, that worsens things.

Added `ReverseColorPopularity` that replaces the `most popular color` and swaps with the `least popular color`. This solved 3 tasks.

Extended the `enumerate objects`, so it identifies the case where there is `OutputImageHasSameStructureAsInputImage` and a single `OutputImageIsInputImageWithNoChangesToPixelsWithColor` and a single `InputImageIsOutputImageWithNoChangesToPixelsWithColor`. And they agree on a color. This solved 3 tasks.

Thoughts:
- Solve more tasks about objects.

## Iteration 14

[Docker image: 2023-05-08T00-15.tar](2023-05-08T00-15.tar)

