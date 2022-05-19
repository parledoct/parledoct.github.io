---
title : Environment setup with Docker
notetype : feed
date : 13-05-2022
---

<p style="text-align:center">
    <img src="https://user-images.githubusercontent.com/9938298/168322173-12e4624a-1e9b-4b31-abc7-06416373de17.jpg" style="width:100%">
</p>

When certain recipes call for specialised tools (a stand mixer, pressure cooker, etc.), not having these tools in your home kitchen (i.e. the environment in which you're carrying out the steps) can make  trying out a recipe very much a chore (or sometimes just not possible at all).

The same thing applies to computation environments: if you don't have a certain set of tools installed on your computer, you probably won't be able to carry out the steps to process your data in a certain way.

To make sure that you can quickly get set up with the tools you need for a given task, we'll use [Docker](https://www.docker.com/) to ensure that you have a computational environment that is identical to that the task is intended to be carried out in. As the illustration below shows, Docker lets use 'containers' which has installed all the tools you need to process your data.

<p style="text-align:center">
    <img src="https://user-images.githubusercontent.com/9938298/168325619-b93a74d2-a41b-4a01-a193-d5c7fd771bf4.png" width="300px">
</p>

## Background: virtualisation 

First, some very brief background on the Docker container set up in order to make sense of some of the installation steps you may have to perform (depending on your operating system, hardware, etc.).

Docker helps ensure a standardised computation environment by running a 'virtual' guest operating system (e.g. Ubuntu Linux) on top of a host, i.e. your computer's operating system (whatever that may be: Windows, macOS, Linux) and hardware (e.g. CPU: Intel, AMD, etc.).

As the combination of the latter two can be variable, there may be additional steps to take to get Docker set up for your specific configuration. I've curated a list of 'Notes' for each operating system by those who have tried this tutorial, so please let me know if you run into any trouble! 

<p style="text-align:center">
    <img src="https://user-images.githubusercontent.com/9938298/169365953-66b37ef6-4ac9-42fd-8e62-a458f5d5cfd4.png" width="500px">
</p>

## Docker installation

### Mac

Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/)

**Notes**
- You may be prompted to allow Docker Desktop to have priveleged access.  

### Windows

Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/)

**Notes**
- You may be asked to enable 'hardware assisted virtualisation' in the computer BIOS if it has been disabled by default by your computer manufacturer. If so, you can search Google for 'enable hardware acceleration {your computer manufacturer}', e.g. 'enable hardware acceleration lenovo'. This should lead you to a step-by-step instruction for your specific computer, possibly from the manufacturer itself, e.g. [https://support.lenovo.com/us/en/solutions/ht500006-how-to-enable-virtualization-technology-on-lenovo-computers](https://support.lenovo.com/us/en/solutions/ht500006-how-to-enable-virtualization-technology-on-lenovo-computers)

### Linux

Follow steps on: [https://docs.docker.com/desktop/linux/install/#generic-installation-steps](https://docs.docker.com/desktop/linux/install/#generic-installation-steps)

## Verification

Once you have Docker installed and running, open a [command-line interface](https://tutorial.djangogirls.org/en/intro_to_command_line/) (e.g. macOS Terminal or Windows Command Prompt), and type in the command `docker run hello-world`:

<script id="asciicast-vY9XqvToi13S6iWjKBIOrDvix" src="https://asciinema.org/a/vY9XqvToi13S6iWjKBIOrDvix.js" async></script>

If you get the expected output above then you're ready to start using Docker! Let's set up the environment we'll use for all the [Parledoct tutorials now](parledoct-tutorials-environment).
