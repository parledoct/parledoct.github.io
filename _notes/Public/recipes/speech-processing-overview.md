---
title : 'Speech processing: an overview'
notetype : feed
date : 22-05-2022
---

If you have done any speech data annotation in Praat or ELAN, congrats — you're already an expert in speech processing! The tutorials on this website will provide an introduction to how some of the more tedious parts of annotation work can be offloaded to machines.

Let's break down what's typically involved in annotating speech. Let's suppose like in the example below we're interested in American English vowels, and so we want to annotate the type of vowel uttered at various time regions of interest. Thus, this task will involve only vowel labels such as 'a' or 'æ' and not consonantal labels such as 'p' or 'b'. Then, to complete the task, you will listen to various speech excerpts of interest and decide which of the vowel labels apply to that excerpt.

<p style="text-align:center">
    <img src="https://user-images.githubusercontent.com/9938298/169721571-6e36f177-9047-4397-8dc4-3e44c95dbb09.png">
</p>

This notion of what labels we're trying to associate with a given region of speech can help us conceptualise different speech processing tasks. Essentially, we want the machine to 'listen' to a given speech region and decide on what label(s) should be associated with that speech region.

The table below provides some example labels for various tasks. In voice activity detection (when is speech happening?), we want to label whether someone is speaking in a given speech region. In speaker diarisation (who is speaking?), we want to label the identity of the speaker in a given speech region. In spoken language identification (what language is being spoken?), we want to label the language of the speech in a given speech region. In automatic speech recognition (what is being said?), we want to derive the orthographic representation of a given speech region.

| Speech processing task       | Example labels    |
|------------------------------|-----------------------|
| Voice activity detection     | Speech (S), Non-speech (N)   |
| Speaker diarisation          | SpeakerA (A), SpeakerB (B) |
| Spoken language identification      | English (E), French (F)  |
| Automatic speech recognition (English) | a, b, c, d, ..., x, y, z |
| Automatic speech recognition (French) | a, à, â, b, ..., x, y, z |

What are these speech regions? Typically, we divide an utterance into smaller windows and for each region we predict a relevant label. Suppose we have an utterance like below where there are two speakers (A, B) with speaker A asking in English (E) what the French (F) word for 'busy' is and speaker B responds that it is 'occupé'. We can see from the table below that each speech processing task (e.g. voice activity detection) essentially boils down to dividing the speech signal into small time windows (1-20) and classifying each window according to the vocabulary of the task at hand (N = non-speech, S = speech). 

<p style="text-align:center">
    <img width="800" src="https://user-images.githubusercontent.com/9938298/169712377-d9c4380d-ce3b-4873-93e6-ca302532182a.png">
</p>

## Feature extraction

The speech signal contains all sorts of rich information but for a given task we may not need all that detail. Feature extraction is the process of distilling from the raw speech signal only the relevant information for the task at hand. Let's say all we want to do is classify vowel regions into 4 labels: high-back, high-front, low-back, low-front. For this task, all we might need from the speech signal is the mean formant values of each speech region.

| Speech region | Mean F1 | Mean F2 |
|-----------------------------------|
| 1 | 512 | 2200 |
| 2 | 831 | 1073 |
| ... | ... | ... |
| 19 | 489 | 2100 |
| 20 | 789 | 1050 |

## Classification

Once we have distilled the speech signal down to only relevant information, we can ask a classifier to predict a relevant label based on that information. For our simplistic task of classifying vowels based on mean first and second formant values into 4 labels, a relatively simple algorithm might suffice:

<table>
<tr>
<td>
<pre>
def predict_label(mean_f1, mean_f2):
    if mean_f1 < 500 and mean_f2 > 1500:
        return "high-front"
    
    if mean_f1 >= 500 and mean_f2 > 1500:
        return "low-front"

    if mean_f1 < 500 and mean_f2 <= 1500:
        return "high-back"

    if mean_f1 >= 500 and mean_f2 <= 1500:
        return "low-back"
</pre>
</td>
<td>
<img width="350" src="https://user-images.githubusercontent.com/9938298/169724568-3d7cf729-5481-41d9-a2f4-ecc75f8e4c70.png">
</td>
</tr>
</table>

More complex tasks require more features extraction and classification systems, some of which require many hundreds of hours of labelled speech in order to train. Luckily, it's become relatively easy to use 'pre-trained' systems either in an off-the-shelf manner or to 'fine-tune' it to your needs with a relatively small amount of data. The tutorials on this website will provide an introduction on how to go about using these systems to process language documentation data.
