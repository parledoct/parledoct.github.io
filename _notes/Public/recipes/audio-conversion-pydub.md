---
title : Audio conversion with pydub
notetype : feed
date : 18-05-2022
---

As we learned in the [audio data basics](#to-do) tutorial, many speech processing tools will require your data to be 16 kHz mono wav files. In this recipe we're going to learn how to use the [pydub](https://github.com/jiaaro/pydub) Python package to convert audio files into this format.

## Setup

### Environment

Assuming you've set up [Docker](environment-setup-with-docker) and the [Parledoct tutorials environment](parledoct-tutorials-environment), we can run a Docker container with `pydub` installed by running the command below. Once the container is set up, copy the relevant URL and open it up in your browser, then open a new Python 3 Console.

<pre>
docker-compose run --rm --service-ports pydub
</pre>

### Data

We'll be using some real fieldwork data on Ihanzu (a Bantu language spoken in Tanzania) collected by [Andrew Harvey](https://www.andrewdtharvey.com/), publicly available with a [CC-BY license](https://creativecommons.org/licenses/by/4.0/) on the [Endangered Languages Archive](https://www.elararchive.org/index.php?name=SO_87014498-be98-4698-82fc-8fac58578d57). For this tutorial, I've uploaded a small subset (3 files) to Zenodo, though keeping the original format from the archive (44.1 kHz stereo mp3).

First let's download the data from Zenodo into the `tmp` folder using `wget`:

<pre>
!wget 'https://zenodo.org/record/6562254/files/ihanzu-harvey-0596_20180518opq_mp3.zip?download=1' -O tmp/data_mp3.zip
</pre>

Let's unzip the downloaded file into the `data` folder:

<pre>
!unzip tmp/data_mp3.zip -d data/
</pre>

> Note: The exclamation mark `!` before the commands `wget` and `unzip` indicates to the JupyterLab console to interpret what follows as a shell command and not a Python one (see [JupyterLab basics](#to-do)).

### Recipe

#### Single file

##### Import required packages

First let's import the pydub package:

<pre>
import pydub
</pre>

##### Read in data from audio file

Now we can use `pydub.AudioSegment.from_file` to read the audio data:

<pre>
audio_data = pydub.AudioSegment.from_file("data/20180518o.mp3")
</pre>

##### Convert to 16 kHz mono

The `audio_data` can be converted to 16 kHz (=16,000 Hz) and to mono (stereo = 2 channels; mono = 1 channel). 

<pre>
audio_data = audio_data.set_frame_rate(16000)
audio_data = audio_data.set_channels(1)
</pre>

##### Export converted data to a wav file

<pre>
audio_data.export("data/20180518o_16khz-mono.wav", format="wav")
</pre>

#### Convert all files in a folder

The following script uses the `glob` package to find all files ending with `mp3` in the `data` folder then applies the recipe above to each of those files. For this tutorial, I've intentionally named the exported files with a `_16khz-mono` suffix, as not to unintentionally over-write any .wav files that may be present (so those who do want this behaviour can modify the script as you need).

<pre>
import glob
import pydub

for input_file in glob.glob("data/*.mp3"):

    # Read in audio data from file
    audio_data = pydub.AudioSegment.from_file(input_file)

    # Convert to 16 kHz mono
    audio_data = audio_data.set_frame_rate(16000)
    audio_data = audio_data.set_channels(1)

    # Derive new file name: "data/20180518o.mp3" => "data/20180518o_16khz-mono.wav"
    #   str.rsplit(".")[0]: return the first part after splitting str on the first ".", starting from the right
    path_no_ext = input_file.rsplit(".")[0]
    output_file = path_no_ext + "_16khz-mono.wav"

    # Export audio to file
    audio_data.export(output_file, format="wav")
</pre>
