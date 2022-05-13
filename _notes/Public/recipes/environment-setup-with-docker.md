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

## Check environment

- Once you have docker installed and running, download and unzip the [Parledoct tutorials](https://github.com/parledoct/tutorials/archive/refs/heads/main.zip) repository

- In your Finder (macOS) or File Explorer (Windows), go to the folder where you unzipped the repository. The folder will be called `tutorials-main` and have a file called `docker-compose.yml`. Make note of the path to this folder, e.g.:
    - macOS/Linux: `/users/nay/Downloads/tutorials-main`
    - Windows: `C:\Users\nay\Downloads\tutorials-main\tutorials-main`

- Launch the Terminal app (macOS) or Command Prompt (Windows) and then to **c**hange **d**irectory to the folder noted above type in:
    ```bash
    # macOS/Linux
    cd /users/nay/Downloads/tutorials-main
    
    # Windows
    cd C:\Users\nay\Downloads\tutorials-main\tutorials-main
    ```
- We can make sure `docker-compose.yml` is inside the folder we've just browsed to by typing:

    ```bash
    # macOS/Linux
    ls

    # Windows
    dir
    ```
    
- If Docker Desktop is installed and running, you should be able to type:

    ```bash
    docker-compose run hello
    ```
    
    Which should output something like:
    
    ```
    Creating network "tutorials_default" with the default driver
    Creating tutorials_hello_run ... done

    Hello from Docker!
    This message shows that your installation appears to be working correctly.

    To generate this message, Docker took the following steps:
     1. The Docker client contacted the Docker daemon.
     2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
        (amd64)
     3. The Docker daemon created a new container from that image which runs the
        executable that produces the output you are currently reading.
     4. The Docker daemon streamed that output to the Docker client, which sent it
        to your terminal.

    To try something more ambitious, you can run an Ubuntu container with:
     $ docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
     https://hub.docker.com/

    For more examples and ideas, visit:
     https://docs.docker.com/get-started/
    ```
