---
title : Voice activity detection with auditok (searching for optimal parameters)
notetype : feed
date : 18-05-2022
---

Like in the [last recipe](vad-auditok-eyeball), we're going to learn how to find an optimal energy threshold that can be used to classify time regions in a recording when someone is speaking. In that recipe, we used the `split_and_plot` function from the [auditok](https://github.com/amsehili/auditok) Python package to eyeball a seemingly good enough energy threshold. In this recipe, we'll learn how to formally evaluate how good splits resulting from a given energy threshold when compared to human annotations.

<p style="text-align:center">
    <img width="450" src="https://user-images.githubusercontent.com/9938298/168442089-92b4c98a-c03c-42db-b9e6-484d66de4643.png">
</p>

## Background: frame error rate

When we create annotations, we're basically labelling each time frame in a recording as being speech (and, implicitly, labelling unannotated regions as non-speech).
We can compare these labels with those predicted by a machine and ask measure as a percentage of the total number of time frames how many were wrongly predicted (False alarm = non-speech frames predicted as speech; False rejection = speech frames predicted as non-speech).
This measure is called the Frame Error Rate (FER) and what we want to do is find an energy threshold that minimises FER for a given recording.

![Screen Shot 2022-05-04 at 16 28 48](https://user-images.githubusercontent.com/9938298/166841438-0b56e510-0001-436d-afb7-7dd05e44bf2c.png)

## Setup

### Environment

Assuming you have [Docker](environment-setup-with-docker) set up and running and have complered the setup steps in [the first tutorial](vad-auditok-defaults#setup), right-click (Windows: Shift + right-click) the `tutorials-voice-activity-detection` folder and [launch a command-line interface](parledoct-tutorials-environment#launch-command-line-interface-at-folder), then [launch the environment](parledoct-tutorials-environment#launch-environment) with `docker-compose up`. Once the environment has been set up and you see the JupyterLab URL, open the URL in your browser and [create a new Python 3 console](jupyterlab-basics#your-first-python-command).

### Data

Assuming you've [downloaded and unzipped](vad-auditok-defaults#data) the Ihanzu data into the `data` directory. We'll also need some human annotations to evaluate how good the predictions are. Let's download from Zenodo an ELAN file where I've completed the first minute of `20180518p.wav` for you:

<pre>
!wget 'https://zenodo.org/record/6519235/files/20180518p.eaf?download=1' -O 'data/20180518p_NS.eaf'
</pre>

You can see that I've annotated time intervals up to the first minute on the `default` tier:

![Screen Shot 2022-05-18 at 08 59 31](https://user-images.githubusercontent.com/9938298/169088659-76df8d0e-7974-4b69-814a-1d3adbe3597a.png)

## Recipe

### Read human annotations from ELAN file

We can use the `pympi` package to read in annotations from an ELAN eaf file. Since only the first 60 seconds are defined, we'll use the `get_annotation_data_between_times` function to grab the annotations on the `default` tier up the the fisrt minute (0 to 60,000 milliseconds).

<pre>
import pympi

human_data   = pympi.Elan.Eaf(file_path="data/20180518p_NS.eaf")

# Convert 60 seconds to 60,000 milliseconds, since that's the time format the function expects
true_regions = human_data.get_annotation_data_between_times('default', 0, 60 * 1000)
</pre>

You can inspect what the `true_regions` variable contains:

<pre>
true_regions
</pre>

It is a list of annotations in which each annotation is a [tuple](https://www.learnbyexample.org/python-tuple/) such as `(1730, 5190, '')` consisting of a start time and end time (both in milliseconds) and an annotation value (all blank = `''`).

<pre>
# [(1730, 5190, ''),
#  (5210, 7740, ''),
#  (7990, 9635, ''),
#  (9675, 11275, ''),
#  (11440, 12650, ''),
#  (12870, 13800, ''),
#  (13940, 14950, ''),
#  (16960, 18280, ''),
#  (19245, 22345, ''),
#  (26460, 28310, ''),
#  (29000, 31490, ''),
#  (32040, 33530, ''),
#  (34320, 35670, ''),
#  (36420, 38440, ''),
#  (39675, 41925, ''),
#  (42305, 44645, ''),
#  (45580, 46530, ''),
#  (46790, 48350, ''),
#  (49195, 50155, ''),
#  (50650, 52710, ''),
#  (53605, 54585, ''),
#  (55180, 56030, ''),
#  (56200, 57010, ''),
#  (57200, 59610, '')]
</pre>

#### Create true labels list

Based on the regions above, let's create a list of 0s and 1s where it's 0 everywhere except time frames in the regions listed above.

<pre>
import numpy as np

# Create a list of zeros with 60,000 positions (60 seconds * 1000)
true_labels = np.zeros(60000, dtype=np.int8)

# Set time frames annotated as speech to 1
for start_ms, end_ms, _ in true_regions:
    true_labels[start_ms:end_ms] = 1
</pre>

We expect the `true_labels` to be a list of ones and zeros. Since the first annotation is `(1730, 5190)` let's just double check that the 1s start around then:

<pre>
# Show labels for 1.5 - 2 seconds (1s should start about half way in at 1730)
true_labels[1500:2000]
</pre>

<pre>
# array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
#        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
#        1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1], dtype=int8)
</pre>

## Evaluate annotations generated by different thresholds

We can now try out different energy thresholds and find out what the frame error rate is by comparing the splits returned by auditok to the ones that I've manually created:

<pre>
import auditok

for threshold in range(45, 55):

    # Create a list of 0s
    predicted_labels = np.zeros(60 * 1000, dtype=np.int8)
    
    # Get predicted regions for first minute of audio for a given threshold
    predicted_regions = auditok.split("data/20180518p.wav", max_read=60, energy_threshold=threshold)
    
    for region in predicted_regions:
        # Convert auditok regions to milliseconds
        start_ms = int(region.meta.start * 1000)
        end_ms   = int(region.meta.end * 1000)

        # Set time frames detected as speech to 1
        predicted_labels[start_ms:end_ms] = 1
        
    # Calcuate frame error rate
    # xor = 'Exclusive Or' (1 if predicted and true frames disagree, 0 if they agree)
    fer = 100 * sum(np.logical_xor(predicted_labels, true_labels)/len(true_labels))
    
    # Report threshold and FER, rounded to 2 decimal places
    print(f"Threshold: {threshold} dB, FER: {round(fer, 2)}%")
</pre>

<pre>
# Threshold: 45 dB, FER: 15.07%
# Threshold: 46 dB, FER: 13.99%
# Threshold: 47 dB, FER: 12.06%
# Threshold: 48 dB, FER: 12.01%
# Threshold: 49 dB, FER: 12.17%
# <b>Threshold: 50 dB, FER: 11.56%</b>
# Threshold: 51 dB, FER: 12.64%
# Threshold: 52 dB, FER: 12.81%
# Threshold: 53 dB, FER: 14.01%
# Threshold: 54 dB, FER: 14.76%
</pre>

Well it looks like the auditok default of 50 dB is actually the best threshold to use (note lowest frame error rate of 11.56%).

Congrats! You've taken the first step in programatically finding optimal parameters for a speech processing task!
