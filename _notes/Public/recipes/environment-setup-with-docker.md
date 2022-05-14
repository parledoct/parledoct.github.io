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

## Docker installation

- Mac/Windows:
    - Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- Linux:
    - Follow steps on: https://docs.docker.com/desktop/linux/install/#generic-installation-steps

### Verification

Once you have Docker installed and running, open a [command-line interface](https://tutorial.djangogirls.org/en/intro_to_command_line/) (e.g. macOS Terminal or Windows Command Prompt), and type in the command `docker run hello-world`:

<script id="asciicast-vY9XqvToi13S6iWjKBIOrDvix" src="https://asciinema.org/a/vY9XqvToi13S6iWjKBIOrDvix.js" async></script>

If you get the expected output above then you're ready to start using Docker! Let's set up the environment we'll use for all the [Parledoct tutorials now](parledoct-tutorials-environment).
