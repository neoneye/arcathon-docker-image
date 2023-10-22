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
- If the score drops, it may be due to the short 10 hour time-limit. I may have to rollback to the 23h30m.
- If the score drops, it may be due to the newly added solutions, that worsens things.

Added `ReverseColorPopularity` that replaces the `most popular color` and swaps with the `least popular color`. This solved 3 tasks.

Extended the `enumerate objects`, so it identifies the case where there is `OutputImageHasSameStructureAsInputImage` and a single `OutputImageIsInputImageWithNoChangesToPixelsWithColor` and a single `InputImageIsOutputImageWithNoChangesToPixelsWithColor`. And they agree on a color. This solved 3 tasks.

Thoughts:
- Solve more tasks about objects.

## Iteration 14

[Docker image: 2023-05-08T00-15.tar](2023-05-08T00-15.tar)

This got `score 6`. Great it stays on the same score as previously.

Thoughts about time-limit:
- Positive: The shorter time-limit didn't impact the score in a negative way.
- Negative: The shorter time-limit didn't shorten the feedback cycle. It still takes +2 days until the result is there. I had hoped it would reduce the response by 1 day.
- I'm going to keep the new 10 hour time-limit.
- I can try halve the time-limit and see if it still yields the same score.

## Changes between iteration 14 and iteration 15

Didn't touch the initial random seed. It's still 4.

I have lowered the duration to 4 hours. I still think that most of the solutions gets discovered within the first few minutes.
- If the score drops, it may be due to the short 4 hour time-limit. I may have to rollback to the 10 hour time-limit.
- If the score drops, it may be due to the newly added solutions, that worsens things.

Solved around 10 tasks:
- Tasks where the job is to identify the biggest, medium, smallest objects, and color them differently.
- Tasks about cropping out the object, and reverse it's colors by popularity.
- Tasks where the job is to objects with a precise mass, and color all other sized objects with a different color.

Success criteria. If it continues with `score 6`, then I'm happy.
- It will mean that the new time limit is good, and that I haven't broken things.

Thoughts:
- My solutions can only do basic operations with objects. My weakness is lack of object operations.
- I guess the private dataset has tasks with object operations. It may be the majority of tasks. I cannot process these `object-tasks`.
- What is an object?

## Iteration 15

[Docker image: 2023-05-10T14-42.tar](2023-05-10T14-42.tar)

This got `score 6`. Great it stays on the same score as previously.

This was incredibly fast. I submitted earlier today, and I got a response in the evening.

Thoughts about time-limit:
- Positive: The shorter time-limit didn't impact the score in a negative way.
- Positive: I got feedback the same day as I submitted.
- I'm going to keep the new 4 hour time-limit.
- This seems to confirm that the discoveries happens in the beginning using the already discovered solutions.
- Can the time-limit can be halved again?

## Changes between iteration 15 and iteration 16

Didn't touch the initial random seed. It's still 4.

Didn't touch the time limit. It's still 4 hours.

I have manually solved 16 tasks.

Now passing on more fields to the LODA programs.
- `InputMostPopularColor` is the most popular color across the inputs.
- `InputSinglePixelNoiseColor` is the color that is the most popular single pixel noise.

Success criteria. If it continues with `score 6`, then I'm happy.
- It will mean that the new time limit is good, and that I haven't broken things.

Thoughts:
- My solutions can only do basic operations with objects. My weakness is lack of object operations.
- I have picked all the low hanging fruit and things have gotten difficult. Now I want to try out machine learning approaches.

## Iteration 16

[Docker image: 2023-05-22T20-22.tar](2023-05-22T20-22.tar)

This got `score 6`. Great it stays on the same score as previously.

Thoughts:
- I'm passing several parameters to the asm programs, this could easily have broken the existing programs. Great that it didn't impact the score negatively.
- I added `most popular color` and the `noise color`, however these didn't lead to new discoveries. I will keep these parameters around.
- I added several asm programs. However none of those lead to new discoveries.
- Try out: Can the time limit be cut in half again.

## Changes between iteration 16 and iteration 17

Didn't touch the initial random seed. It's still 4.

Experiments with logistic regression.

- 800 tasks in total in the ARC1 dataset.
- 268 tasks where the input size is different than the output size. I cannot process with these with logistic regression, so I ignore these tasks.
- 545 tasks where the input size is the same as the output size. These are the tasks that I'm processing with logistic regression.
- Around 40 tasks can be solved with logistic regression.
- Around 22 tasks are that I had no prior solution for.

I don't run mutations, so it stops immediately after logistic regression.

Success criteria. If it improves the so it goes beyond `score 6`, then I'm happy.
- It will mean that the logistic regression did make an improvement.
- If it drops below `score 6`, then the mutations that I disabled may have found solutions to the secret dataset, but since it's disabled then these are no longer solved.
- If it continues with `score 6`, then logistic regression didn't solve any tasks in the secret dataset.

Thoughts:
- Reject a predicted output from logistic regression, if it's obviously junk or too different than the training pairs.
- Do logistic regression with tasks where the input size is different than the output size.
- Experiment with more input data for the logistic regression.
- How can I do mutations after the logistic regression has been run? Currently I have disabled mutations. If bad predictions can be rejected, then the remaining tasks can be mutated.
- Principal component analysis (PCA) with the logistic regression data. Can insignificant data be eliminated, so it's only the significant stuff that remain?

## Iteration 17

[Docker image: 2023-05-26T10-14.tar](2023-05-26T10-14.tar)

This got `score 5`. I'm surprised that it dropped. Previous it has been `score 6`. It was supposed to use logistic regression for cases where no other solution could be found.

Thoughts:
- Scenario A: my check for `task.occur_in_solutions_csv` doesn't work. So it did compute logistic regression for all the tasks, and thus overwriting what was found by the existing solutions. This is the same as running only logistic regression and not running any of the existing solutions. So this would be 5% of the programs that can be solved with logistic regression. If this is the case, then make sure I don't overwrite the existing solution.
- Scenario B: some of the solutions are discovered while mutating the existing programs. And the tasks in the hidden dataset are so tricky, that logistic regression cannot solve any of those. Try submit an executable where the time limit is 0, so that it only runs the existign solutions and none of the mutations.
- Scenario C: other ideas?

## Changes between iteration 17 and iteration 18

Didn't touch the initial random seed. It's still 4.

Experiments with logistic regression.

Scenario A seems to be what I ended up in. I inspected the code and fixed it. I did traverse all tasks, and thus overwriting the already found solutions with the result of logistic regression.
Now I only traverse the unsolved tasks and try doing logistic regression on it.

Success criteria. The success criteria is identical to my previous success criteria. If it improves the so it goes beyond `score 6`, then I'm happy.
- It will mean that the logistic regression did make an improvement.
- If it drops below `score 6`, then the mutations that I disabled may have found solutions to the secret dataset, but since it's disabled then these are no longer solved.
- If it continues with `score 6`, then logistic regression didn't solve any tasks in the secret dataset.

## Iteration 18

[Docker image: 2023-05-28T21-50.tar](2023-05-28T21-50.tar)

This got `score 5`.
I fixed a problem with the logistic regression, that was overwriting already found solutions.
So now I'm pretty confident that the logistic regression hasn't messed up previous found solutions.

Thoughts about why the score has dropped. I have disabled the 4 hours of mutations. There is a chance that the solutions has been found during the mutations.
I should try out run both logistic regression together with the mutations.

Idea A: Safe route. Disable logistic regression, reenable mutation. Verify that `score 6` still can be reached.
I also changed the time limit, so I may have to increase it to 4 hours again.
Afterwards submit again, but with the logistic regression enabled + mutations.

Idea B: Less safe. Continue with logistic regression enabled, and get the mutation code working together with logistic regression.
It's impossible to determine if the `score 6` is only due to the LODA programs, or some of them was found with logistic regression.

## Changes between iteration 18 and iteration 19

Didn't touch the initial random seed. It's still 4.

Restored time limit to 4 hours again.

Disabled logistic regression.

Reenabled mutations of existing solutions.

Success criteria.
- In the past this combo 2 times gotten `score 6`. So if I can get `score 6` again, then it's good.
- I don't expect the score to go higher, since I have not added any new solutions.

Thoughts:
- Investigate can the time limit be halfed to 2 hours, and still yield the same score.
- Afterwards get mutations to work together with logistic regression.
- Reminder to self: Only change 1 thing at a time.

## Iteration 19

[Docker image: 2023-05-30T21-15.tar](2023-05-30T21-15.tar)

This got `score 5`. This is lower than what I had expected, I had expected `score=6`. 

My logistic regression code is entirely disabled. It usees the existing solutions and mutates the existing solutions.

There are still issues about the mutation process that are non-deterministic.

The way that a program gets rejected when it's too time consuming.
It may be that a solution was found in the past, but now the mutation takes slightly longer time to run and the program gets rejected.
So time limit may be the reason.

The non-deterministic can also arise from a trashed random-seed.
If a program is rejected due to exeeding the time limit, and the random seed has been impacted by that program, then all the following mutations 
will be using another random-seed. I think I have solve this issue.

It may be extracting data from `HashMap` or `HashSet` causes data ends up in a random order. I have fixed it many places, by sorting the data
before further processing is taking place. However there may still be places where no sorting is happening.

Thoughts:
- Halving the run time from 4 hours to 2 hours, will it still yield `score=5`?
- Doubling the max time limit for each solution, will it solve programs during the mutations, that it wouldn't otherwise solve?

## Changes between iteration 19 and iteration 20

Disabled the existing solutions.

Disabled mutation of the existing solutions.

Now it's only the logistic regression that runs.

Maybe my initial impression that logistic regression works is flawed. If so, then the score should be zero.
Thinking that logistic regression works, and it doesn't work that may be a lot of time wasted on nothing.
So with this docker image I hope to clarify, does it work or not.

Outcomes:
- Score is zero, then it means that my logistic regression code doesn't work.
- Score is non-zero, then it means that my logistic regression does work. I hope for this scenario.

Thoughts:
- If it works, then it's weird that it's solves the same tasks that are being solved with the existing solutions.

## Iteration 20

[Docker image: 2023-06-01T15-56.tar](2023-06-01T15-56.tar)

This got `score 0`. This means that my logistic regression code doesn't work.
I had mistakenly believed that my logistic regression approach was solving tasks, but it was instead the tasks that were using the existing solutions.
When testing logistic regression in isolation, then zero tasks are solved.

Thoughts:
- Pause my logistic regression approach. In it's current form it doesn't solve anything.
- Try out another approach.

## Changes between iteration 20 and iteration 21

I have been experimenting converting images into a graph. For this I use [petgraph](https://docs.rs/petgraph/latest/petgraph/).

I have been experimenting with gravity of objects.

Disabled logistic regression.

Reenabled the existing solutions. Reenabled mutation of the existing solutions.

Didn't touch the initial random seed. It's still 4.

The time limit is 4 hours.

I have added 23 new solutions. And removed 1 redundant solution.

I have replaced existing code that used `overlay` several times, with a single function that deals with overlaying multiple layers on top of each other. Z-stack.

I have made a split image function into X parts. It simplifies several exising solutions that did splitting by kludgy ways.

Thoughts:
- My changes to existing solutions may brake things, so that previous solutions are no longer found.
- My changes to existing solutions may also lead to new discoveries, while preserving the previously found solutions.
- The 23 new solutions, may help find new solutions.

Success criteria.
- In the past the highest I have gotten was `score 6`. So if I can get `score 6` again, then it's good.
- I doubt it will go higher, since the new solutions are not doing any object manipulation, and I consider the object manipulation tasks more advanced than the pixel manipulation tasks.
- If the score drops below `score 5`, then my `improvements` may have broken something.

## Iteration 21

[Docker image: 2023-06-04T23-35.tar](2023-06-04T23-35.tar)

This got `score 5`. So I didn't break everything by making changes to several existing solutions. That's good.

Thoughts:
- I should do more object manipulation.

## Changes between iteration 21 and iteration 22

The initial random seed is 4.

The time limit is 4 hours.

Major rework of the `arc_work_model`. Extracted big parts of `arc_work_model::Input` to a new struct `ImageMeta`, so that the same image processing now also is applied to the output images.

I'm experimenting with gravity of objects.

Added 4 solutions. Probably not going to solve any of the hidden tasks.

Success criteria.
- I doubt that I again get `score=6`. If a program takes too long to execute, it gets killed. A program that previously have been killed, maybe it completes successfully.
- If the score stays the same `score=5`, then I know I haven't broken too many things while doing the major rework.
- If the score drops, then I should investigate what I may have broken.

Thoughts:
- I have experienced stackoverflows recently, and I'm considering moving the data from the stack to the heap.
- There are fewer tasks in the hidden dataset (100 tasks), so it's less likely that to crash due to stackoverflow.

## Iteration 22

[Docker image: 2023-06-11T23-59.tar](2023-06-11T23-59.tar)

This got `score 5`. So I didn't break everything by making changes to several existing solutions. That's good.

## Changes between iteration 22 and iteration 23

The initial random seed is 4.

The time limit is 4 hours.

I have investigated the source of the stackoverflow and it was due to my `flood_fill` algorithm, that didn't check if the color was already assigned. Causing infinite recursion.
It was hard to track down. The crash happened after 1 hour of running in RELEASE mode.
```
thread 'tokio-runtime-worker' has overflowed its stack
fatal runtime error: stack overflow
zsh: abort      loda-rust arc-competition
```
I have been toying with some other ARC datasets, and I realized that with the `1D-ARC` dataset, I could get the program to crash sooner. So I started removing as many ARC tasks as possible, 
and finally were down to a single ARC task. Similarly with the solutions, I removed everything until there were just 1 solution remaining.
Now the program was crashing instantly, and I could run it with `rust-lldb` and see where the crash happened.

```
PROMPT> cargo build -p loda-rust-cli
PROMPT> rust-lldb ./target/debug/loda-rust arc-competition
(lldb) r
2023-06-12T18:19:11Z - Start of program
lots of output
Process 48594 stopped
* thread #12, name = 'tokio-runtime-worker', stop reason = EXC_BAD_ACCESS (code=2, address=0x17127bfc0)
    frame #0: 0x00000001011a4c5c loda-rust`core::ptr::mut_ptr::_$LT$impl$u20$$BP$mut$u20$T$GT$::is_null::h1a435efb678a5fe5(self=<unavailable>) at mut_ptr.rs:35
Target 0: (loda-rust) stopped.
(lldb) bt -c 10
* thread #12, name = 'tokio-runtime-worker', stop reason = EXC_BAD_ACCESS (code=2, address=0x17127bfc0)
  * frame #0: 0x00000001011a4c5c loda-rust`core::ptr::mut_ptr::_$LT$impl$u20$$BP$mut$u20$T$GT$::is_null::h1a435efb678a5fe5(self=<unavailable>) at mut_ptr.rs:35
    frame #1: 0x0000000101189c48 loda-rust`_$LT$alloc..vec..Vec$LT$T$C$A$GT$$u20$as$u20$core..ops..deref..Deref$GT$::deref::h6ac3d357652fed5a at mod.rs:1173:21
    frame #2: 0x0000000101189c28 loda-rust`_$LT$alloc..vec..Vec$LT$T$C$A$GT$$u20$as$u20$core..ops..deref..Deref$GT$::deref::h6ac3d357652fed5a(self=0x0000000171474ab0) at mod.rs:2533:40
    frame #3: 0x00000001010ee184 loda-rust`_$LT$alloc..vec..Vec$LT$T$C$A$GT$$u20$as$u20$core..ops..index..Index$LT$I$GT$$GT$::index::h53e56d010093d6b8(self=0x0000000171474ab0, index=16) at mod.rs:2628:23
    frame #4: 0x00000001000201c0 loda-rust`loda_rust::arc::image::Image::get::h84b709382fb13d8b(self=0x0000000171474ab0, x=16, y=0) at image.rs:87:14
    frame #5: 0x00000001003c5488 loda-rust`loda_rust::arc::image_fill::FloodFill::flood_fill4::he8ac8be579fa3bb9(image=0x0000000171474ab0, x=16, y=0, from_color='\0', to_color='\0') at image_fill.rs:61:25
    frame #6: 0x00000001003c5524 loda-rust`loda_rust::arc::image_fill::FloodFill::flood_fill4::he8ac8be579fa3bb9(image=0x0000000171474ab0, x=17, y=0, from_color='\0', to_color='\0') at image_fill.rs:66:9
    frame #7: 0x00000001003c5574 loda-rust`loda_rust::arc::image_fill::FloodFill::flood_fill4::he8ac8be579fa3bb9(image=0x0000000171474ab0, x=16, y=0, from_color='\0', to_color='\0') at image_fill.rs:67:9
    frame #8: 0x00000001003c5524 loda-rust`loda_rust::arc::image_fill::FloodFill::flood_fill4::he8ac8be579fa3bb9(image=0x0000000171474ab0, x=17, y=0, from_color='\0', to_color='\0') at image_fill.rs:66:9
    frame #9: 0x00000001003c5574 loda-rust`loda_rust::arc::image_fill::FloodFill::flood_fill4::he8ac8be579fa3bb9(image=0x0000000171474ab0, x=16, y=0, from_color='\0', to_color='\0') at image_fill.rs:67:9
```

This revealed that `flood_fill4()` function was calling itself forever. Now I have fixed the flood fill algorithm.

Added a few more solutions.

Success criteria.
- I doubt that I again get `score=6`. It may be due to random chance.
- If the score stays the same `score=5`. I have not made any major reworks, except fixing the flood fill and adding a few solutions, probably not going to impact the score.

Thoughts:
- I'm glad that I solved the stackoverflow problem. It has been haunting me for months.

## Iteration 23

[Docker image: 2023-06-12T22-49.tar](2023-06-12T22-49.tar)

I didn't receive a mail with the score. So I'm not sure how the stackoverflow fix will impact the score, probably not.

## Changes between iteration 23 and iteration 24

I have focused on `arc-size`, it's a new subcommand for predicting the output size of a task.
The way to invoke it: `loda-rust arc-size path/to/task.json`

## Iteration 24

I have merged it to `develop`. So I'm on iteration 25 now.

## Changes between iteration 24 and iteration 25

I didn't get feedback on iteration 23, and I haven't submitted iteration 24. So now I'm submitting what I have.

Changes since iteration 22:
- Fixed stackoverflow that caused crash while running.
- Added several programs.
- The output size prediction has been slightly improved.

Success criteria.
- I doubt that I again get `score=6`. It may be due to random chance.
- It will probably be `score=5` as it has been for several iterations.

## Iteration 25

[Docker image: 2023-06-18T23-40.tar](2023-06-18T23-40.tar)

This got `score 5`. Great.

## Changes between iteration 25 and iteration 26

I have focused on computing the output size for more tasks.

Solved a few tasks from the Concept ARC dataset.

Success criteria.
- I doubt that I again get `score=6`. It may be due to random chance.
- It will probably be `score=5` as it has been for several iterations.

Thoughts:
- Ensemble of existing solutions + mine, is that something I should try out?
- A graph representation of a task.

## Iteration 26

[Docker image: 2023-06-22T15-17.tar](2023-06-22T15-17.tar)

This got `score 5`. Great.

## Changes between iteration 26 and iteration 27

I have made a few tasks to the ARC 2 dataset, and it's takes quite some time to come up with a challenging ARC task, that differs from what I have previously seen.

I'm making a UI for interacting with a graph representation of a single ARC task. However it's still in the early stages.

What did I do when I got the score 6. Can I recreate the score consistently?
I'm resubmitting my programs from iteration 12 with date `2023-04-30T11-50`.

Success criteria.
- I doubt that I again get `score=6`. It may be due to random chance, but the chance may be higher when I'm using the same programs, that in the past have yielded score=6.
- It will probably be `score=5` as it has been for several iterations.

## Iteration 27

[Docker image: 2023-06-30T11-01.tar](2023-06-30T11-01.tar)

This got `score 6`. Great. This confirms that the solutions within iteration 12 are still working and is yielding the same score.

I wonder if all the solutions are found without doing any mutations?

Things to try out:
- Can the number of mutations be lowered to zero, and still yield the same score? Currently the time limit is 4 hours.
- Can the time limit be lowered to 30 minutes, and still yield the same score? Currently the time limit is 4 hours.
- Can the number of solutions be reduced to a minimum, and still yield the same score?
- How should I organize the solutions, so that I still keep the crappy solutions. A comment in the top of the program, is this good/bad.
- Run a few mutations with the good solutions, when no progress is made, then start mutating the bad solutions.
- There was another time I got `score=6`. Was that the exact same tasks? If it was different, I may be able to do a crossover.

## Changes between iteration 27 and iteration 28

I'm making a UI for interacting with a graph representation of a single ARC task. However it's still in the early stages.

I have disabled mutations of existing programs. I wonder if all the solutions are found, without doing any mutations.

Scenario A: If it yields the same (`score 6`), then it means that no mutations are needed.

If it yields a lower score, then there can be multiple meanings:
- Scenario B: The score 6 is due to random chance, and the solutions still works, but this time around the 6th solution wasn't found.
- Scenario C: The lower score is due to some of the solutions being found during the mutation phase.

Success criteria.
- Best case is scenario A.
- If it's scenario B or scenario C, then I can try tweak the max number of mutations or the time limit. And check how that impacts the score.

## Iteration 28

[Docker image: 2023-07-02T12-03.tar](2023-07-02T12-03.tar)

This got `score 5`. Ok, it's either scenario B or scenario C.

The 5 solutions are found while trying out the existing solutions.

The 6th solution may be found while mutating the existing solutions.
Eventually while trying out the existing solutions due to randomness where `HashSet` and `HashMap` are being used.
This means that it's highly sensitive to the content of the analytics dir.
I suspect this 6th solution is found within the first few minutes while mutating.
I can try stopping after 1 mutation, to verify this.

## Changes between iteration 28 and iteration 29

I'm making a UI for interacting with a graph representation of a single ARC task. However it's still in the early stages.

I have set the max number of mutations to 1 mutation.

Probing for a sweet spot in between these extremes:
- Infinity: In the past I have continued for during as many iterations as possible within 4 hours. This is a long time to wait for a response. If I can get the response faster that would be nice.
- Zero: In the previous iteration I entirely disabled the mutations. However this didn't discover the 6th solution. So 1 or more mutations are needed. I don't know how many iterations are needed.

The 5 solutions are found using the existing programs without doing mutations.

Scenario A: If it yields the `score 6`, then the 6th solution is found within the 1st mutation. This is best case.

Scenario B: If it yields the `score 5`, then the 6th solution must require 2 or more mutations, and more probing is needed.
If this is the scenario, then I can try 4 or 8 mutations.
There is still the possibility that the 6th solution is found by random chance. I have confirmed it 2 times that the 6th solution can be found, (iteration 12 and iteration 27).

Success criteria.
- Best case is scenario A.
- If it's scenario B, then more probing is needed to find the sweet spot between 0 and infinity.

## Iteration 29

[Docker image: 2023-07-03T12-45.tar](2023-07-03T12-45.tar)

This got `score 5`. Ok, it's scenario B.

What do I know now:
- 5 solutions are found using the existing programs, without applying any mutations.
- the 6th solution is found after 2 or more mutations. I have ruled out 0 mutations and 1 mutations.
- the 6th solution is found before 4 hours of mutations.

How many mutations does it take to find the 6th solution?

## Changes between iteration 29 and iteration 30

I'm making a UI for interacting with a graph representation of a single ARC task. However it's still in the early stages.

Probing for how many mutations is needed to find the 6th solution. 

I'm searching for a sweet spot between 2 and infinity, but shorter than 4 hours. Let's try 64 mutations, and see if it discovers the 6th solution.
- I have already ruled out doing 0 mutation and 1 mutation. These doesn't discover the 6th solution.
- Running the miner for 4 hours, this does find the 6th solution.
- The hidden ARC dataset has 100 programs. How long time does it take mutating 100 programs. 
- Lower bound 10 seconds. Then it takes around 10 minutes to complete 64 mutations.
- Upper bound 120 seconds. Then it takes around 2 hours to complete 64 mutations.

Scenario A: If it yields the `score 6`, then the 6th solution was found within the mutations. 
There is a chance that the max number of iterations can be lowered further, maybe divide by 4.

Scenario B: If it yields the `score 5`, then the 6th solution was not found, and may be require even more mutations.
Here the max number of iterations can be raised further, maybe multiply by 4.

## Iteration 30

[Docker image: 2023-07-04T12-21.tar](2023-07-04T12-21.tar)

This got `score 5`. Ok, it's scenario B.

What do I know now:
- Using solutions from iteration 12.
- 5 tasks are solved are found while running the existing solutions.
- The 6th task is solved while mutating the existing solutions.
- It takes 65 mutations or more, and less than 4 hours. To find the 6th solution.
- Eventually the 6th task is solved by random chance.

## Changes between iteration 30 and iteration 31

I'm making a UI for interacting with a graph representation of a single ARC task. However it's still in the early stages.

Probing for how many mutations is needed to find the 6th solution. 

I'm searching for a sweet spot between 65 and infinity, but shorter than 4 hours. 
I have seen `score=5` 3 times in a row.
I wonder if I can find a upper bound, that yields the 6th solution.
Let's try 8192 mutations, which I imagine will be way overkill. But what if it's not overkill, then I can go on probing forever.

Scenario A: If it yields the `score 6`, then the 6th solution was found within the mutations. 
The number of mutations is so unimaginable high, so I seriously hope that this is the scenario.
There is a chance that the max number of iterations can be lowered further, maybe divide by 4.

Scenario B: If it yields the `score 5`, then the 6th solution was not found, and may be require even more mutations.
Here the max number of iterations can be raised further, maybe multiply by 4.
If this is the scenario, then multiple things can cause the 6th solution to not be found.
- Random chance is impacting the mutations. I sort things and try to avoid ambiguous cases, but there are definitely several places that deal with `HashSet`, `HashMap` where I may have forgotten to sort the items.
- If it's a hefty machine that lab42 is running my program on, then it may reach 8192 much sooner than 4 hours, so it never finds the program.
I can try set the max number of mutations to 1048576. This is way more than what I can run in 4 hours on my own computer.

## Iteration 31

[Docker image: 2023-07-05T14-34.tar](2023-07-05T14-34.tar)

This got `score 6`. Ok, it's scenario A. This is what I hoped for.

Previously the unknown upper bound was infinity. Now it's reduced to 8192 mutations.

What do I know now:
- Using solutions from iteration 12.
- 5 tasks are solved are found while running the existing solutions.
- The 6th task is solved while mutating the existing solutions.
- It takes in the range 65..8192 mutations, and less than 4 hours. To find the 6th solution.
- Eventually the 6th task is solved by random chance.

## Changes between iteration 31 and iteration 32

I'm making a UI for interacting with a graph representation of a single ARC task. However it's still in the early stages.

Probing for how many mutations is needed to find the 6th solution. 

I'm searching for a sweet spot between 65 and 8192, and shorter than 4 hours. 

Let's try 4096 mutations, and see if it still generates the 6th solution.

Scenario A: If it yields the `score 6`, then the 6th solution was found within the mutations. 
There is a chance that the max number of iterations can be lowered further, maybe divide by 2.
a place halfway between 65 and 4096.

Scenario B: If it yields the `score 5`, then the 6th solution was not found, and may be require even more mutations.
a place halfway between 4096 and 8192.

## Iteration 32

[Docker image: 2023-07-06T12-42.tar](2023-07-06T12-42.tar)

This got `score 6`. Ok, it's scenario A. This is what I hoped for.

Previously the unknown upper bound was 8192. Now it's reduced to 4096 mutations.

## Changes between iteration 32 and iteration 33

I'm making a UI for interacting with a graph representation of a single ARC task. However it's still in the early stages.

Probing for how many mutations is needed to find the 6th solution. 

I'm searching for a sweet spot between 65 and 4096, and shorter than 4 hours. 

Let's try 2048 mutations, and see if it still generates the 6th solution.

Scenario A: If it yields the `score 6`, then the 6th solution was found within the mutations. 
There is a chance that the max number of iterations can be lowered further, maybe divide by 2.
a place halfway between 65 and 2048.

Scenario B: If it yields the `score 5`, then the 6th solution was not found, and may be require even more mutations.
a place halfway between 2048 and 4096.

## Iteration 33

[Docker image: 2023-07-10T16-24.tar](2023-07-10T16-24.tar)

This got `score 6`. Ok, it's scenario A. This is what I hoped for.

Previously the unknown upper bound was 4096. Now it's reduced to 2048 mutations.

## Changes between iteration 33 and iteration 34

I'm making a UI for interacting with a graph representation of a single ARC task. However it's still in the early stages.

Probing for how many mutations is needed to find the 6th solution. 

I'm searching for a sweet spot between 65 and 2048, and shorter than 4 hours. 

Let's try 1024 mutations, and see if it still generates the 6th solution.

Scenario A: If it yields the `score 6`, then the 6th solution was found within the mutations. 
There is a chance that the max number of iterations can be lowered further, maybe divide by 2.
a place halfway between 65 and 1024.

Scenario B: If it yields the `score 5`, then the 6th solution was not found, and may be require even more mutations.
a place halfway between 1024 and 2048.

## Iteration 34

[Docker image: 2023-07-11T14-50.tar](2023-07-11T14-50.tar)

This got `score 6`. Ok, it's scenario A. This is what I hoped for.

Previously the unknown upper bound was 2048. Now it's reduced to 1024 mutations.

## Changes between iteration 34 and iteration 35

I'm making a UI for interacting with a graph representation of a single ARC task. However it's still in the early stages.

Probing for how many mutations is needed to find the 6th solution. 

I'm searching for a sweet spot between 65 and 1024, and shorter than 4 hours. 

Let's try 512 mutations, and see if it still generates the 6th solution.

Scenario A: If it yields the `score 6`, then the 6th solution was found within the mutations. 
There is a chance that the max number of iterations can be lowered further, maybe divide by 2.
a place halfway between 65 and 512.

Scenario B: If it yields the `score 5`, then the 6th solution was not found, and may be require even more mutations.
a place halfway between 512 and 1024.

## Iteration 35

[Docker image: 2023-07-12T12-21.tar](2023-07-12T12-21.tar)

This got `score 6`. Ok, it's scenario A. This is what I hoped for.

Previously the unknown upper bound was 1024. Now it's reduced to 512 mutations.

## Changes between iteration 35 and iteration 36

I have made a UI for interacting with a graph representation of a single ARC task. [Here is a demo](https://youtu.be/GDQoVlyfAZQ).
I'm not yet making use of the graph representation. This is what I will be coding in the near future.

Probing for how many mutations is needed to find the 6th solution. 

I'm searching for a sweet spot between 65 and 512, and shorter than 4 hours. 

Let's try 256 mutations, and see if it still generates the 6th solution.

Scenario A: If it yields the `score 6`, then the 6th solution was found within the mutations. 
There is a chance that the max number of iterations can be lowered further, maybe divide by 2.
a place halfway between 65 and 256.

Scenario B: If it yields the `score 5`, then the 6th solution was not found, and may be require even more mutations.
a place halfway between 256 and 512.

## Iteration 36

Github rejected this commit. Github has a 100 mb limit, so I had to make a zip file instead. It's inconsistent. Typically compressed `tar` files is named `tar.gz` or `tgz`, and doesn't use zip for compression.

[Docker image: 2023-07-18T00-23.tar.zip](2023-07-18T00-23.tar.zip)

This got `score 6`. Ok, it's scenario A. This is what I hoped for. 

Previously the unknown upper bound was 512. Now it's reduced to 256 mutations.

Also it contains the new graph representation code that I have been working on for the last month, so I know that I haven't broken things too much.

What do I know now:
- While running the existing program without mutations, it solves 5 of the hidden tasks.
- While mutating the existing programs between 65 mutations and 256 mutations, this solves 1 of the hidden tasks. And it takes less than 4 hours.
- My logistic regression does not solve any of the hidden tasks. It solves around 50 tasks of the public dataset. So I doubt it's a way forward.
- Adding more solutions, makes the predictions worse, causing the 6th solution to be unsolved.
- The graph representation correctly identifies similarities between input and output shapes.

## Changes between iteration 36 and iteration 37

I'm working on the graph representation.

Github doesn't like files bigger than 100mb, so i have reworked the `docker save` code, so it now a `tgz` file.  Previously it saved `tar` files. 
Docker's documentation seems to prefer `tar` for saving and piping through gzip. However `docker load` can load `tgz` files just fine.

I'm searching for a sweet spot between 65 and 256, and shorter than 4 hours. 

Let's try 128 mutations, and see if it still generates the 6th solution.

Scenario A: If it yields the `score 6`, then the 6th solution was found within the mutations. 
There is a chance that the max number of iterations can be lowered further, maybe divide by 2.
a place halfway between 65 and 128.

Scenario B: If it yields the `score 5`, then the 6th solution was not found, and may be require even more mutations.
a place halfway between 128 and 256.

## Iteration 37

[Docker image: 2023-07-19T15-56.tgz](2023-07-19T15-56.tgz)

This got `score 6`. Ok, it's scenario A. This is what I hoped for. 

Previously the unknown upper bound was 256. Now it's reduced to 128 mutations.

Migration from using `.tar` files, to using `.tgz` files. This confirms that it works.

Thoughts:
- The graph representation has taken much longer than I anticipated. While waiting I have been probing.
- I began probing between iteration 26 and iteration 27, a month ago, and it has taken much longer than I imagined to narrow down how many mutations it takes.
- Can the `score=6` be recreated again and again. Yes. Finding 5 solutions requires no mutations. It takes between 65 mutations and 128 mutations to find the 6th solution.
- How much can I shorten the run time to find a solution. I have set it to 4 hours. I have not tried halving it.
- Further probing can be done to narrow down the exact number of mutations, somewhere in between 65 and 128. 
- Probing can be done to narrow down what solutions contributes to the `score=6`. What solutions can be removed without harming the score? I have no idea what combo of programs are used.
- Adding more solutions to the pool of existing solutions, and the score drops to `score=5`.

## Changes between iteration 37 and iteration 38

Since last I have been trying out several things:
- Experiments with encoder / decoder with ARC tasks. Terrible results.
- Investigating Q-learning with ARC tasks.
- Made an interactive web interface for interacting the ARC tasks.

I realized that I hadn't any proper detection mechanism for split views. So I have added this.
I have added a `SolveSplit`, that solves 17 of 800 tasks in the public ARC 1 dataset.

It can only deal with splitviews where there is a separator line.
Known problem. If there is no separator line, then `SolveSplit` will ignore the task.

I wonder how `SolveSplit` does with the hidden ARC 1 dataset.
In this docker image, I'm trying out only the `SolveSplit`, so it will perform worse than my previous high `score=6`.

Scenario A: If it solves 1 or more tasks. That means that the `SolveSplit` code is useful.
I can try combine it with my previous code, and if it improves the score.

Scenario B: If it solves 0 tasks.

No matter if it's scenario A or B.
I have to deal with the scenario where there is zero-size separator in the splitview.

In the logistic regression code. I have made a terrible mistake in the json format, where the predictions ids are garbage.
this may explain why LR was unable to solve any tasks. I will have to rework the json format, and resubmit to
determine if logistic regression is working or not.

## Iteration 38

[Docker image: 2023-08-14T01-00.tgz](2023-08-14T01-00.tgz)

This got `score 0`. Ok, it's scenario B.

The `SolveSplit` doesn't solve any tasks from the hidden dataset.

## Changes between iteration 38 and iteration 39

Corrected the flawed json outputted by the logistic regression solver.
The corrected json format may change the outcome.

Upgraded to Rust from 1.65.0 to 1.71.1.

Scenario A: If it solves 0 or more tasks, that means logistic regression is still as useless as before.
Previously the logistic regression has been unable to solve any tasks from the hidden ARC dataset.
This is such a tiny code change, so I doubt that this solves any of the tasks in the hidden ARC dataset.

Scenario B: If it solves 1 or more tasks, that means the logistic regression is useful after all.
I will be surprised that this happens.

## Iteration 39

[Docker image: 2023-08-15T15-56.tgz](2023-08-15T15-56.tgz)

This got `score 0`. Ok, it's scenario A.

The `SolveLogisticRegression` doesn't solve any tasks from the hidden dataset.

## Changes between iteration 39 and iteration 40

Improved the `SolveSplit` so it now solves 27 of 800 ARC tasks. This is an improvement of 10 tasks.
Let's see if it can solve one of the tasks in the hidden ARC dataset.

I have experimented with ViT (vision transformers) and made an image generator.
The ViT is very data hungry and I'm not yet seeing any progress. Too soon to determine if it will work or not.

Scenario A: If the scores is 1 or greater, then the `SolveSplit` solves 1 or more tasks.
Afterwards in next iteration, enable the LODA solver and see if it works together.
It may be there is overlap and both solvers solve the same task.
It may be that there is no overlap, this is best case scenario.

Scenario B: If it scores zero, then the `SolveSplit` is still as useless as before.

## Iteration 40

[Docker image: 2023-09-10T23-43.tgz](2023-09-10T23-43.tgz)

This got `score 0`. Ok, it's scenario B.

## Changes between iteration 40 and iteration 41

The `ShapeIdentification` code, now rotates the images by 45 degrees, so it better can recognize diagonal `ShapeType`s.
Huge thanks to Douglas Miles for this 45 degree rotation idea.

I have tweaked logistic regression, so it now solves 3 more tasks.
- Now solves 54 tasks of 800 tasks.
- Previously solved 51 tasks of 800 tasks.

The tweaks I have done to logistic regression:
- Making use of the 45 degree rotated shape types, so it may be improve recognition of diagonal shapes.
- Now running 5 iterations where the previous prediction is used as input for the next prediction.

Scenario A: If the score is 1 or greater, then the `SolveLogisticRegression` solves 1 or more tasks.

Scenario B: If the score is 0, then the `SolveLogisticRegression` is still useless.

## Iteration 41

[Docker image: 2023-09-13T14-04.tgz](2023-09-13T14-04.tgz)

This got `score 1`. Scenario A. The first confirmation that my logistic regression approach is working.

## Changes between iteration 41 and iteration 42

I have tweaked logistic regression, so it now solves 2 more tasks.
- Now solves 56 tasks of 800 tasks.
- Previously solved 54 tasks of 800 tasks.

Scenario A: If the score is 2 or greater, then the `SolveLogisticRegression` solves 2 or more tasks. Which will be an improvement.

Scenario B: If the score is 1, then the `SolveLogisticRegression` may be solving the same task as in previous iteration.
Or it may be a different task it is solving.

Scenario C: If the score is 0, then I may have messed up something in the `SolveLogisticRegression` code so it no longer solves any tasks.

## Iteration 42

[Docker image: 2023-09-15T01-15.tgz](2023-09-15T01-15.tgz)

This got `score 1`. Scenario B. The logistic regression approach is still working. No improvement, no worsening.

Thought:
If there is overlap between the logistic regression and the genetic algorithm, then the the score be 6.
If there is no overlap, then solved tasks are different, and the score will be 6+1, so score 7.

Thought:
How many more tasks do I have to solve on the public 800 ARC tasks in order to solve 2 tasks from the 100 hidden ARC tasks?
Currently I'm solving 56 tasks (7% of 800). Will I have to solve twice as many?

## Changes between iteration 42 and iteration 43

Made it possible to accumulate predictions from multiple solvers. So now it's running both logistic regression and genetic algorithm. An ensemble of solvers.
Until now only been 1 solver type has been able to run.

Improved the logistic regression so it solves 57 tasks of 800 tasks.

Scenario A: If the score is 7, then the logistic regression still solves 1 task. And the genetic algorithm solves 6 tasks.
This is the sunshine scenario.

Scenario B: If the score is 6, then the there is overlap between the set of tasks solved by logistic regression and tasks solved by genetic algorithm.

Scenario C: If the score is 6, and if I have broken the logistic regression while improving it, then the genetic algorithm solves 6 tasks.
Then I will have to unwind my improvements to the logistic regression code (the commits that broke it).

Scenario D: If the score is 5 or less, then I have messed up in some way. I have made cherry-picking mechanism that takes the best 3 predictions.
If I have messed it up, then the score may be impacted.

## Iteration 43

[Docker image: 2023-09-18T21-31.tgz](2023-09-18T21-31.tgz)

This got `score 1`. Scenario D. I have broken something. It does solve 1 task, I guess via the logistic regression solver. However it doesn't seem like 
there is any contribution of solved tasks from the genetic algorithm.

## Changes between iteration 43 and iteration 44

Investigated what was causing havoc. It seems like only the logistic regression has been running and the genetic algorithm has been disabled.

I have now entirely disabled the `resume_from_last_snapshot` feature flag, so I'm sure this isn't the culprit.

The `SolveLogisticRegression` works when it's not being emulated.
However running a `x86_64` docker image on a `M1` chip, then `SolveLogisticRegression` takes forever to run, that I have to abort it before it solves a single task.
I cannot be certain it's able to solve anything on an `x86_64` chip.

Scenario A: If the score is 5 or less, then I have messed up in some way.
And introducing the `resume_from_last_snapshot` feature flag wasn't sufficient to fix it.
I guess the chance is 60% for this scenario.

Scenario B: If the score is 7, then the logistic regression still solves 1 task. And the genetic algorithm solves 6 tasks.
This is the sunshine scenario.

Scenario C: If the score is 6, then the there is overlap between the set of tasks solved by logistic regression and tasks solved by genetic algorithm.

Scenario D: If the score is 6, and if I have broken the logistic regression while improving it, then the genetic algorithm solves 6 tasks.
Then I will have to unwind my improvements to the logistic regression code (the commits that broke it).
Since it was able to solve 1 task possible by logistic regression, then I doubt this is the case.

## Iteration 44

[Docker image: 2023-09-19T16-19.tgz](2023-09-19T16-19.tgz)

This got `score 7`. Scenario B. This is a new high for me. Yay.

My previous best `score 6` was 5 months ago in iteration 12.

What solves what:
- The logistic regression solves 1 task.
- The genetic algorithm solves 5 tasks using the existing programs.
- The genetic algorithm solves 1 tasks with a mutated program.
- The `SolveSplit` does not solve anything.

## Changes between iteration 44 and iteration 45

Improved the logistic regression so it solves 61 tasks of 800 tasks. And partially solves 3 tasks.

SolveLogisticRegression can now deal with tasks that have 2 or more training pairs, this helped solves a few more tasks.
Previously it could only handle 1 test pair.

Scenario A: If the score is 8, then it's an improvement. Sunshine scenario.

Scenario B: If the score is 7, then it's status quo.

Scenario C: If the score is 6, then I have worsened the logistic regression solver so much that it solves nothing.

Scenario D: If the score is 5 or less, then I have seriously messed up.

## Iteration 45

[Docker image: 2023-09-21T23-25.tgz](2023-09-21T23-25.tgz)

This got `score 7`. Scenario B. This is status quo.

It's quite a big code change to how the logistic regression code is working, so I'm just happy that it's still working.

## Changes between iteration 45 and iteration 46

Experiments training Llama2 7B with `alpaca` formatted examples of basic image manipulations.
I have been using [Axolotl](https://github.com/OpenAccess-AI-Collective/axolotl) for automated training.

This Rust project does not yet include any LLM code.

Tweaked logistic regression so it solves 1 more task.

Improved the logistic regression so it solves 1 more task. Now it solves 62 tasks of 800 tasks. And partially solves 3 tasks.
Let's see if this makes a difference.

Scenario A: If the score is 8, then it's sunshine scenario.

Scenario B: If the score is 7, then it's status quo.
I may have broken things while tweaking the logistic regression, so this scenario may not happen.

Scenario C: If the score is 6, then I have broken something with the logistic regression code.
And I may have to roll back.

## Iteration 46

[Docker image: 2023-10-06T00-20.tgz](2023-10-06T00-20.tgz)

This got `score 8`. Scenario A. This is sunshine scenario.

The code for this version is in the loda-rust repo: [commit `430f3d4b1182a40058230e54564b8e6c482e1509`](https://github.com/loda-lang/loda-rust/commit/430f3d4b1182a40058230e54564b8e6c482e1509).

## Changes between iteration 46 and iteration 47

Improved the logistic regression so it solves a few more task. Now it solves 69 tasks of 800 tasks. And partially solves 2 tasks.
Let's see if this makes a difference.

Scenario A: If the score is 8, then it's sunshine scenario.

Scenario B: If the score is 7, then it's status quo.
I may have broken things while tweaking the logistic regression, so this scenario may not happen.

Scenario C: If the score is 6, then I have broken something with the logistic regression code.
And I may have to roll back.

Scenario D: If feedback on my last submission turns out to have score=8 (I doubt it. Oh, it did happen, yay). And my current submission gets score=7.
Then I must have broken something.

## Iteration 47

[Docker image: 2023-10-09T01-12.tgz](2023-10-09T01-12.tgz)

This got `score 7`. Scenario D. I have broken something.

I will continue with the logistic regression as it is, despite this version performing worse.
It solves more tasks on the public ARC tasks. I will try to get it to solve even more.
With more improvements, if it gets stuck on `score 7`, then I may roll back to iteration 46 that scored 8.

## Changes between iteration 47 and iteration 48

I have improved on the logistic regression so it now solves 1 more task.
Out of the 800 tasks in the ARC 1 dataset.
It fully solves 70 tasks. It partially solves 2 tasks.

Scenario A: If the score is 8, then it's sunshine scenario.

Scenario B: If the score is 7, then it's status quo.

Scenario C: If the score is 6, then I have broken something with the logistic regression code.

## Iteration 48

[Docker image: 2023-10-11T01-08.tgz](2023-10-11T01-08.tgz)

This got `score 6`. Scenario C. My improvements have worsened the logistic regression code, so it no longer solves any tasks.

I assumed the more tasks that my logistic regression code solved on the public ARC 1 dataset, the more it would be able to solve on the hidden ARC 1 dataset.
I'm forgetting the public dataset have one distribution and the hidden dataset have another distributions. It's two different distributions.
Optimizing for one distribution may harm the other distribution.

Thoughts:
I'm considering random feature flipping. Maybe doing 10 logistic regression per tasks, with random features enabled/disabled.
For this I need a ranking system that discards the worst predictions, and keeps the best candidates.

## Changes between iteration 48 and iteration 49

I have improved on the logistic regression so it now also can deal with tasks where input size and output size is different.
Until now logistic regression has only been able to process tasks where the input and output size had the same sizes.

Out of the 800 tasks in the ARC 1 dataset.
It fully solves 70 tasks. It partially solves 6 tasks.

I have fixed a typo in the way `center_row_right` was computed, where x and y was swapped.
Interestingly this mistake caused a higher number of tasks to get solved in the public ARC dataset.
Fixing the mistake and the number of tasks solve changed from 72 to 69.
Having fixed the typo where x and y was swapped. Hopefully this prevents the bias for the public ARC dataset, 
so it's able to solve more than 1 task in the hidden dataset.

Scenario A: If the score is 8, then 2 tasks gets solved with logistic regression.

Scenario B: If the score is 7, then 1 task gets solved with logistic regression.

Scenario C: If the score is 6, then something is still broken with the logistic regression code.

## Iteration 49

[Docker image: 2023-10-16T02-07.tgz](2023-10-16T02-07.tgz)

This got `score 6`. Scenario C. The logistic regression solver, doesn't solve any tasks.

## Changes between iteration 49 and iteration 50

Upgraded to a newer `linfa` package.

Out of the 800 ARC tasks, 108 task was aborted due to the error: `MoreThuenteLineSearch`.
I have made a retry mechanism that tries invoking the fit() function, with a lower `max_iterations`.
So whenever it encounters the error: `MoreThuenteLineSearch: Search direction must be a descent direction.`
then it tries again. This solves 1 more task than previously.

Added gravity images. This solves 1 more task than previously.

Tiny improvements to the logistic regression code.
It fully solves 72 tasks. It partially solves 6 tasks.

Scenario A: If the score is 8, then 2 tasks gets solved with logistic regression.

Scenario B: If the score is 7, then 1 task gets solved with logistic regression.

Scenario C: If the score is 6, then something is still broken with the logistic regression code.

## Iteration 50

[Docker image: 2023-10-18T02-14.tgz](2023-10-18T02-14.tgz)

This got `score 6`. Scenario C. The logistic regression solver, doesn't solve any tasks.

Thoughts:
- Maybe I provide too many features for logistic regression. What if I provide fewer?

## Changes between iteration 50 and iteration 51

Made the logistic regression solver run in parallel, so it finishes faster depending on the number of cores on the server.

Enabled lots of features related to diagonal histograms. Hopefully it will be able to solve 1 of the hidden ARC tasks that contains diagonal structures.

I have on purpose focused on solving much different tasks than I have solved the logistic regression code in the past.
It fully solves 59 tasks. It partially solves 6 tasks.
Big overlap with what it has solved in the past. There is a few new ones that it hasn't solved before.

Scenario A: If the score is 8, then 2 tasks gets solved with logistic regression.
I doubt that this scenario happens.

Scenario B: If the score is 7, then 1 task gets solved with logistic regression.
My primary goal. I hope to get a confirmation that the logistic regression code is running. 
If it solves just one task, then it will confirm that the logistic regression code is working.

Scenario C: If the score is 6, then something is still broken with the logistic regression code.

## Iteration 51

[Docker image: 2023-10-20T01-22.tgz](2023-10-20T01-22.tgz)

This got `score 6`. Scenario C. The logistic regression solver, doesn't solve any tasks.

Since I got score 8, I have changed lots of things in the logistic regression solver. It would be nice to see if I can reproduce that score.
This would confirm that the many changes hasn't entirely broken the logistic regression solver.

## Changes between iteration 51 and iteration 52

Not getting anywhere with the logistic regression. So I have partially rolled back to old commit a049260e29723dad027aa89658a95b5e2aa70e70,
this got `score=8`.

I have taken the best parts from my newer code and reapplied it to the old code.

Unlike the old code, this new code: 
- Can process tasks where `input_size` is different than `output_size`.
- Runs uses `rayon` for running the solver in parallel.
- Deals with the error: `MoreThuenteLineSearch: Search direction must be a descent direction.`.
- Made `linfa` non-optional

I'm hesitant about migrating more functionality for now, since it may be what is causing the score to drop to zero.
It will be nice to get a confirmation that things still works.

Newer things that I have not migrated over:
- Bugfix where I had made a typo with the x and y axis in the line `let center_row_right: Image = match center_row.right_columns(y_reverse)`. This may be what is solving tasks from the hidden ARC dataset, so I have not applied the bugfix.
- Support for 11 colors. This extra color is used for the outside area.
- Feature flipping. All the places that had `enable_` prefix. I don't have them.

When I have gotten a confirmation, then I can migrate more of the newer functionality.

Scenario A: If the score is 8, then 2 tasks gets solved with logistic regression.
It should end up in this scenario, since the code is very close to the old code, that did score 8.
This will confirm that my changes so far hasn't broken things, and that I can proceed with the migration of newer code.

Scenario B: If the score is 7, then 1 task gets solved with logistic regression.
If this scenario happens, then I have not done a good job of rolling back to the old code.
It's a mix of old and new code, so it's not an exact rollback.
So this scenario is also likely.

Scenario C: If the score is 6, then something is still broken with the logistic regression code.
I have been getting score 6 for several iterations. Maybe it's something very basic that I have messed up.

## Iteration 52

[Docker image: 2023-10-22T12-30.tgz](2023-10-22T12-30.tgz)


