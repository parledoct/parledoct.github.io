---
title : Parledoct tutorials environments
notetype : feed
date : 14-05-2022
---

As mentioned in the [Docker setup tutorial](environment-setup-with-docker), Docker lets us standardise computation environments across different operating systems and hardware.

That said, instead of having a single Docker container with the tools for all the Parledoct tutorials, there will be several separate ones (e.g. `tutorials-introduction`, `tutorials-voice-activity-detection`, etc.) to keep each one relatively small, as some of the 'heavyweight' tools that we only require for more advanced tutorials are several gigabytes in size (e.g. the [HuggingFace Transformers](https://huggingface.co/docs/transformers/index) library for automatic speech recognition).

The process of setting up each of these environments will be the same and we'll learn this process.

## Setup

**Note**: if you've landed here from a linked page, substitute references `tutorials-introduction` with the one that's relevant for you, e.g. `tutorials-voice-activity-detection`.

### Download tutorial zip file

At the start of each tutorial sequence (e.g. Part 1 of 3), I will provide a link to a zip file download for the computation environment needed for all tutorials in that sequence. For this tutorial, let's download the `tutorials-introduction` zip file:

- [https://github.com/parledoct/tutorials/archive/refs/heads/introduction.zip](https://github.com/parledoct/tutorials/archive/refs/heads/introduction.zip)

### Unzip and browse to parent folder

Unzip the `tutorials-introduction.zip` and in your file browser (e.g. Finder for macOS, File Explorer for Windows), navigate to the folder that contains the unzipped `tutorials-introduction` folder, **but do not go inside it**.

### Launch command-line interface at folder

Right click on the `tutorials-introduction` folder and launch a command-line interface:

- **Windows**: Shift + Right click on the `tutorials-introduction` folder > Open command window here
- **Linux**: Right click on the `tutorials-introduction` folder > Open in Terminal
- **macOS**: Right click on the `tutorials-introduction` folder > (Services >) New Terminal at Folder
    - **Note**: You may first need to set up the 'New Terminal at Folder' service via  > System Preferences > Keyboard > Shortcuts > Services

### Launch environment

In the command-line interface, type in: `docker-compose up` and press enter.

**Note**. The very first time you run this command Docker will need to download (and sometimes setup) the computation environment and this may take some time. Once the environment is set up, you will see a message that looks like:

<pre>
Attaching to tutorials-introduction-tutorial
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:01.505 ServerApp] jupyterlab | extension was successfully linked.
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:01.518 ServerApp] nbclassic | extension was successfully linked.
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:02.159 ServerApp] notebook_shim | extension was successfully linked.
tutorials-introduction-tutorial  | [W 2022-05-21 20:33:02.206 ServerApp] WARNING: The Jupyter server is listening on all IP addresses and not using encryption. This is not recommended.
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:02.207 ServerApp] notebook_shim | extension was successfully loaded.
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:02.208 LabApp] JupyterLab extension loaded from /opt/conda/lib/python3.8/site-packages/jupyterlab
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:02.208 LabApp] JupyterLab application directory is /opt/conda/share/jupyter/lab
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:02.211 ServerApp] jupyterlab | extension was successfully loaded.
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:02.216 ServerApp] nbclassic | extension was successfully loaded.
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:02.216 ServerApp] Serving notebooks from local directory: /parledoct-tutorials
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:02.216 ServerApp] Jupyter Server 1.17.0 is running at:
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:02.216 ServerApp] http://localhost:29796/lab?token=6fd6419aa2fd08e1a97d4a3727c308da9bf16fe7f6f2caa8
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:02.216 ServerApp]  or http://127.0.0.1:29796/lab?token=6fd6419aa2fd08e1a97d4a3727c308da9bf16fe7f6f2caa8
tutorials-introduction-tutorial  | [I 2022-05-21 20:33:02.216 ServerApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
tutorials-introduction-tutorial  | [C 2022-05-21 20:33:02.219 ServerApp] 
tutorials-introduction-tutorial  |     
tutorials-introduction-tutorial  |     To access the server, open this file in a browser:
tutorials-introduction-tutorial  |         file:///root/.local/share/jupyter/runtime/jpserver-1-open.html
tutorials-introduction-tutorial  |     Or copy and paste one of these URLs:
tutorials-introduction-tutorial  |         http://localhost:29796/lab?token=6fd6419aa2fd08e1a97d4a3727c308da9bf16fe7f6f2caa8
tutorials-introduction-tutorial  |      or http://127.0.0.1:29796/lab?token=6fd6419aa2fd08e1a97d4a3727c308da9bf16fe7f6f2caa8
</pre>

If you copy and paste one of the last URLs **including the `token=...`** into your browser, e.g.:

```
http://127.0.0.1:29796/?token=86acc1e58d49d087468c9ae0c441464f70f5aa9db67121e4
```

You will see the `JupyterLab` interface that looks like:

<p style="text-align:center">
    <img alt="jupyter-lab-tutorials-env" src="https://user-images.githubusercontent.com/9938298/169668286-8440ae70-d782-413e-8f07-ca8a57f16ee2.png">
</p>

If you're able to see this interface, congrats — you've set up your first standardised computation environment! If this is your first time ever seeing a JupyterLab interface, head on over to the [JupyterLab basics tutorial](jupyterlab-basics) before starting the others, which will assume some basic familiarity with the interface.

### Shut down environment

Once you're done with a given tutorial, it is important to shut down the container. To do this, all you need to down is to go back to the command-line interface (e.g. your macOS Terminal/Windows Command Prompt where you copied the `http://127.0.0.1:29796?token=...` address) and press `Ctrl + C` (all platforms: even on macOS, i.e. not  + C).

#### Shut down containers from Docker Dashboard

If you accidentally left a previous container running, it may prevent a new one from starting up (one message you may see is `Bind for 0.0.0.0:29796 failed: port is already allocated`).

To see and manage containers that are running on your computer, you can go to the Docker Dashboard:

- **macOS**: click the Docker menu bar icon (top right of screen near your clock, wifi, etc.) then click Dashboard
- **Windows**: click the Docker icon to open the Dashboard
- **Linux**: open Docker Desktop

To stop a running container, hover your mouse over the container name to reveal various actions, then press the Stop button:

<p style="text-align:center">
    <img alt="docker-dashboard-env" src="https://user-images.githubusercontent.com/9938298/169669767-bba36570-974c-482f-b67d-eb365d2222d6.png">
</p>
