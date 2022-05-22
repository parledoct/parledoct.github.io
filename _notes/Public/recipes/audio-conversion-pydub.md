---
title : Audio conversion with pydub and ffmpeg
notetype : feed
date : 18-05-2022
---

As we learned in the [audio data basics](#to-do) tutorial, many speech processing tools will require your data to be 16 kHz mono wav files. In this recipe we're going to learn how to convert audio files into this format using the [pydub](https://github.com/jiaaro/pydub) Python package, which provides a Python interface to the very popular ffmpeg library for audio/video processing.

For more intermediate/advanced readers who want to directly use ffmpeg, I also provide a handy snippet at the [end of this tutorial](#shell-advanced).

## Setup

### Environment

Assuming you have [Docker](environment-setup-with-docker) set up and running, and that you've familiarised yourself with the process for setting up[Parledoct tutorials environments](parledoct-tutorials-environment), download the `tutorials-pydub` zip file:

- [https://github.com/parledoct/tutorials/archive/refs/heads/pydub.zip](https://github.com/parledoct/tutorials/archive/refs/heads/pydub.zip)

Once unzipped, right-click the `tutorials-pydub` folder and [launch a command-line interface](parledoct-tutorials-environment#launch-command-line-interface-at-folder), then [launch the environment](parledoct-tutorials-environment#launch-environment) with `docker-compse up`. Once the environment has been set up and you see the JupyterLab URL, open the URL in your browser and [create a new Python 3 console](jupyterlab-basics#your-first-python-command).

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

> Note: The exclamation mark `!` before the commands `wget` and `unzip` indicates to the JupyterLab console to interpret what follows as a shell command and not a Python one (see [JupyterLab basics](#jupyterlab-basics)).

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

If you navigate to the `tutorials-pydub/data` folder on your computer (e.g. in Finder or File Explorer), you will see the exported `20180518o_16khz-mono.wav` file there.

#### Convert all files in a folder

##### Python

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

##### Shell (advanced)

The following 1-line shell script performs the same task as the Python script above (remember to prefix `!` if you're using the Python 3 Console to execute a shell command).

<pre>
find data -name "*.mp3" | parallel ffmpeg -y -i {} -ar 16000 -ac 1 {.}_16khz-mono.wav
</pre>

While it is more concise, it assumes some familiarity with how the `find`, `parallel` and `ffmpeg` shell commands work. Let's break it down.

- `find data -name "*.mp3"` Creates a list of all mp3 files in the data folder which is then passed to `parallel` via the pipe `|`
- `parallel` will execute the command that follows (e.g. `ffmpeg`) in parallel, which means for example if you have a multi-core CPU it can process multiple files simultaneously (you can limit how many simultaneous jobs happen with `-j`, e.g. `parallel -j 2`)
- `ffmpeg` takes an input file specified by `-i`, the output sample rate is specified by `-ar`, the number of channels by `-ac`, and `-y` specifies that existing files should be overwritten. Without parallel, you would typically write for example

    <pre>
    ffmpeg -i data/20180518o.mp3 -ar 16000 -ac 1 data/20180518o_16khz-mono.wav
    </pre>

    - The `{}` and `{.}` are represent replacement strings, which is a functionality provided by `parallel` (see [documentation](https://www.gnu.org/software/parallel/parallel_tutorial.html#replacement-strings)).
        - `{}`: the original input path, e.g. `data/20180518o.mp3`
        - `{.}`: the path without the extension, e.g. `data/20180518o`
