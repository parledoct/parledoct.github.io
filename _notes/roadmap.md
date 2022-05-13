---
title: Parledoct roadmap
permalink: /roadmap/
layout: none
---

<title>Parledoct roadmap</title>

<svg id="mindmap" style="width:100%;height:100%;"></svg>
<script src="https://cdn.jsdelivr.net/npm/d3@5"></script>
<script src="https://cdn.jsdelivr.net/npm/markmap-lib@0.7.11/dist/browser/view.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/markmap-lib@0.7.11/dist/browser/transform.min.js"></script>

<pre id="mm" hidden>
# Parledoct

## Concepts

### Audio basics

- [Sampling rate](../notes/Welcome-to-the-garden)
- Stereo vs. Mono

### Programming basics

- Computation environment
- Data structures
	- Lists
- Control structures
	- For loop

## Recipes

### Calcuate total duration of wav files in folder

##### By hand in a spreadsheet
##### Using Python
##### Using R

### Voice activity detection

#### On Google Colab with provided files
#### On your own computer with provided files
</pre>

<script>
markdown = document.getElementById("mm").innerHTML;
const data = markmap.transform(markdown);

markmap.Markmap.create("#mindmap", null, data);
</script>
