---
title : Voice activity detection with auditok (eyeballing good enough parameters)
notetype : feed
date : 18-05-2022
---

Like in the [last recipe](vad-auditok-defaults), we're going to learn how to find all the time regions in a recording when someone is speaking and export these time regions to the `default` tier in a new ELAN `.eaf` file. In that recipe, we used the [auditok](https://github.com/amsehili/auditok) Python package, which provides an easy way to detect regions in a sound file that are above a certain threshold of acoutsic energy, using the default threshold of 50 dB. In this recipe, we'll learn how to adjust this parameter.

<p style="text-align:center">
    <img width="450" src="https://user-images.githubusercontent.com/9938298/168442089-92b4c98a-c03c-42db-b9e6-484d66de4643.png">
</p>

## Setup

### Environment

Assuming you have [Docker](environment-setup-with-docker) set up and running and have complered the setup steps in [the first tutorial](vad-auditok-defaults#setup), right-click (Windows: Shift + right-click) the `tutorials-voice-activity-detection` folder and [launch a command-line interface](parledoct-tutorials-environment#launch-command-line-interface-at-folder), then [launch the environment](parledoct-tutorials-environment#launch-environment) with `docker-compose up`. Once the environment has been set up and you see the JupyterLab URL, open the URL in your browser and [create a new Python 3 console](jupyterlab-basics#your-first-python-command).

### Data

Assuming you've [downloaded and unzipped](vad-auditok-defaults#data) the Ihanzu data into the `data` directory.

## Recipe

### Split and plot with auditok

We previously used `auditok.split()` to both load and split a file. This time, we'll seperate out these two steps. First, we'll use `auditok.load()` to load in the data and then `split_and_plot()` to try out various splits on the same file (i.e. so we don't re-load the data unnecessarily each time).

<pre>
audio_data     = auditok.load("data/20180518o.wav")
speech_regions = audio_data.split_and_plot()
</pre>

![download](https://user-images.githubusercontent.com/9938298/169068548-a54a5f96-5fba-4a13-8735-ea2c13599cdd.png)

It seems to be working okay? How can we tell? How do we pick the best threshold for our data? We'll spend the rest of this tutorial trying to answer that question.

### Partial loading

First, `auditok.load` by default loads the whole file. As we can see from the x-axis, it's about 400 seconds in length (6.5 or so minutes). Even though it's a relatively short file (e.g. not an hour long), we really can't tell how well the detected regions correspond to speech regions. So let's use the `max_read` parameter to specify the maximum amount of audio (in seconds) to be loaded and then re-apply `split_and_plot` to try and get a better picture...

<pre>
audio_data     = auditok.load("data/20180518o.wav", <b>max_read=30</b>)
speech_regions = audio_data.split_and_plot()
</pre>

![download-1](https://user-images.githubusercontent.com/9938298/169071706-a20bede0-41aa-4905-9c04-7d2d3ef3bb09.png)

Overall, it looks pretty good! Just eyeballing the splits, it looks like the default energy threshold of 50 dB might be a bit low. We can see that some noise at the very start of the file has been picked up as a speech interval.

### Adjusting the energy threshold

The split functions in auditok (e.g. `split()` and `split_and_plot()`) take an `energy_threshold` parameter which we can specify (defaults to 50 dB if unspecified). Let's try specifying an energy threshold of 60 dB and see how the splits look:

<pre>
speech_regions = audio_data.split_and_plot(<b>energy_threshold=60</b>)
</pre>

![download-2](https://user-images.githubusercontent.com/9938298/169072233-39759cfc-0a14-46db-ae74-8cdf213a2afb.png)

Okay, 60 dB is too high. We can see some clearly speech regions have been partially marked as non-speech. Feel free to try out different energy thresholds to see the difference in the resulting splits (e.g. very low `30`, or something just a bit higher than the default, e.g. `53`).

### Detect and export for all files in folder

Now let's suppose you've tried out a few different energy thresholds and decided `53` is the best for the first 30 seconds of this file. We can update the script from our [last recipe](vad-auditok-defaults#detect-and-export-for-all-files-in-folder) to split all wav files in the `data` folder based on this threshold, with two key assumptions:

- The threshold that worked well for the first 30 seconds of `data/20180518o.wav` should also work well for the rest of this file
- The threshold that worked well for `data/20180518o.wav` should also work well for the other files (`data/20180518p.wav` and `data/20180518q.wav`)

These assumptions probably hold for these files in particular (3 elicitation sessions in the same recording enviroment on the same day, i.e. 2018-05-18).

<pre>
import auditok
import glob
import os
import pympi

for wav_file in glob.glob("data/*.wav"):

    speech_regions = auditok.split(wav_file, <b>energy_threshold=53</b>)

    annot_data = pympi.Elan.Eaf(file_path=None)
    annot_data.add_linked_file(os.path.basename(wav_file))

    for region in speech_regions:
        # auditok stores the start and end times (in seconds)
        # as the metadata of each detected region. Convert to
        # milliseconds to be compatible with the time format
        # the add_annotation function is expecting

        start_ms = int(region.meta.start * 1000)
        end_ms   = int(region.meta.end * 1000)

        # Add a blank annotation (value='') on the 'default' tier
        annot_data.add_annotation(id_tier='default', start=start_ms, end=end_ms, value='')

    # 'data/20180518p.wav' -> 'data/20180518p.eaf'
    eaf_name = wav_file.rsplit('.')[0] + '.eaf'
    annot_data.to_file(eaf_name)
</pre>

Congrats! You've taken the first step in finding optimal parameters for a speech processing task!
