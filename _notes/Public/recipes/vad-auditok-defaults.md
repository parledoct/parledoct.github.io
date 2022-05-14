---
title : Voice activity detection with auditok (default parameters)
notetype : feed
date : 14-05-2022
---

In this recipe we're going to learn how to find all the time regions in a recording when someone is speaking (more technically: [voice activity detection](https://en.wikipedia.org/wiki/Voice_activity_detection)) and export these time regions to the `default` tier in a new ELAN `.eaf` file.

For the detection part, we'll use the [auditok](https://github.com/amsehili/auditok) Python package, which provides an easy way to detect regions in a sound file that are above a certain threshold of acoutsic energy (defaults to `50 dB`). 
We'll then use the [pympi](https://dopefishh.github.io/pympi/index.html) Python package to export the time regions to an ELAN file.

<p style="text-align:center">
    <img width="450" src="https://user-images.githubusercontent.com/9938298/168442089-92b4c98a-c03c-42db-b9e6-484d66de4643.png">
</p>

## Setup

### Environment

Assuming you've set up [Docker](environment-setup-with-docker) and the [Parledoct tutorials environment](http://localhost:4000/notes/parledoct-tutorials-environment), we can run a Docker container with `auditok` and `pympi` installed by running the command below. Once the container is set up, copy the relevant URL and open it up in your browser, then open a new Python 3 Console.

<pre>
docker-compose run --rm --service-ports vad
</pre>

### Data

We'll be using some real fieldwork data on Ihanzu (a Bantu language spoken in Tanzania) collected by [Andrew Harvey](https://www.andrewdtharvey.com/), publicly available with a [CC-BY license](https://creativecommons.org/licenses/by/4.0/) on the [Endangered Languages Archive](https://www.elararchive.org/index.php?name=SO_87014498-be98-4698-82fc-8fac58578d57). For this tutorial, we'll be using versions of the recordings that I've modified (resampled, converted to mono, etc.) and uploaded to Zenodo.

First let's download the data from Zenodo into the `tmp` folder using `wget`:

<pre>
!wget 'https://zenodo.org/record/6519000/files/ihanzu-harvey-0596_20180518opq.zip?download=1' -O tmp/data.zip
</pre>

> Note: The exclamation mark `!` before the commands `wget` and also `unzip` below indicates to the console to interpret what follows as a shell command and not a Python one

Let's unzip the downloaded file into the `data` folder:

<pre>
!unzip tmp/data.zip -d data/
</pre>

### Recipe

#### Import required packages

First let's import the required packages:

<pre>
import auditok
import pympi
</pre>

#### Detect speech regions

Now we can use `auditok.split` to find speech regions:

<pre>
speech_regions = auditok.split("data/20180518q.wav")
</pre>

#### Export speech regions to ELAN

Finally, we can export these speech regions to an ELAN file. First, we'll create a new, empty `Eaf` object by specifying `file_path=None` (if you specify a real file path, it'll load in the data from that file) and, for convenience, also add the wav file as a linked file so when we view the resulting `.eaf` file in ELAN later, the wav file will also be loaded in ELAN.

<pre>
annot_data = pympi.Elan.Eaf(file_path=None)
annot_data.add_linked_file('20180518q.wav')
</pre>

We'll loop through each detected region in `speech_regions` (which we just created using auditok) and add a blank annotation onto the 'default' tier (which has automatically created by `pympi.Elan.Eaf`). The only thing we have to do to make the time regions detected by auditok (specified in seconds) to be compatible is to convert the times into milliseconds (as expected by the `add_annotation` function).

<pre>
for region in speech_regions:
    # auditok stores the start and end times (in seconds)
    # as the metadata of each detected region. Convert to
    # milliseconds to be compatible with the time format
    # the add_annotation function is expecting

    start_ms = int(region.meta.start * 1000)
    end_ms   = int(region.meta.end * 1000)

    # Add a blank annotation (value='') on the 'default' tier
    annot_data.add_annotation(id_tier='default', start=start_ms, end=end_ms, value='')
</pre>

Now we can export the data to an `eaf` file:

<pre>
annot_data.to_file('data/20180518p.eaf')
</pre>

If you browse in your Finder (macOS) or File Explorer (Windows) to the `data` folder inside the `tutorials-main` folder, you will see that there is now an `20180518p.eaf` file which you can open:

<p style="text-align:center">
    <img src="https://user-images.githubusercontent.com/9938298/168444929-c459dd5c-0d1e-4281-aa66-1993aa9f3a34.png">
</p>
