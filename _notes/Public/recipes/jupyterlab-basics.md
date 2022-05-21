---
title : JupyterLab basics
notetype : feed
date : 21-05-2022
---

Now that you have [set up Docker](environment-setup-with-docker) and [launched a JupyterLab interface](parledoct-tutorials-environment), let's get familiar with how this interface works.

## Data sharing

One of the first things you might notice is the file browser pane on the left side of the interface and that there may be some files (e.g. `docker-compose.yml`) and folders (e.g. `data`, `tmp`) you might have seen before. These files and folders are the same as those you might have seen in the `tutorials-intro` folder if you happen to have opened it in your Finder (macOS) or File Explorer (Windows).

<p style="text-align:center">
    <img alt="data-sharing" src="https://user-images.githubusercontent.com/9938298/169670857-338e7d0d-8391-445d-90e1-6448a84ee26f.png">
</p>

You can verify this by clicking on the 'New Folder' icon to create a new folder (I've left it unnamed as 'Untitled Folder'), and this folder will also appear on your computer inside the `tutorials-intro` folder which you can browse to using Finder or File Explorer.

Thus, both you/your computer and the computation environment with the data processing tools can read/write/modify the contents of these files and folders. So when we create ELAN eaf files in `data` in later tutorials, you can browse to the folder on your computer to open in ELAN the files you created using the tools inside the computation environment.

## Python and shell commands

As mentioned in the [Docker setup tutorial](environment-setup-with-docker), Docker helps to set up on top of your own operating system (macOS, Windows, Linux) and hardware (Intel, AMD, etc.) a standardised virtual operating system that contains various tools we want to use, e.g. Python.
JupyterLab provides a conveinent way to execute both Python commands and '[shell](https://datacarpentry.org/shell-genomics/01-introduction/)' commands which are passed along to the virtual operating system.

<p style="text-align:center">
    <img alt="shell-python" src="https://user-images.githubusercontent.com/9938298/169671681-5e3106ad-bd48-4580-99b5-22e6c545529d.png">
</p>

### Your first Python command

To execute a Python command, let's create a new `Python 3 Console` in JupyterLab:

<p style="text-align:center">
    <img alt="python3-console" src="https://user-images.githubusercontent.com/9938298/169671440-79d010d4-d9bc-47cd-b150-b98d32d802cb.png">
</p>

In the input area at the bottom, you can type in a Python command such as `print("Hello!")` and press `Shift + Enter` to execute the command.

<p style="text-align:center">
    <img alt="python3-hello" src="https://user-images.githubusercontent.com/9938298/169671555-6f44c5e7-9148-438a-85ff-f22bbe172ca1.png">
</p>

### Your first shell command

As we noted above, we can use the same JupyterLab Python 3 Console to execute shell commands. All we have to do to denote that commands we enter into the console should be interpreted as shell commands is to prefix them with an exclamation mark. Thus you can type in a command such as `!echo "Hello!"` and press `Shift + Enter`. In later tutorials, we will mainly be using shell commands for file management, e.g. download files with `wget`.

## Wrap up

This concludes the JupyterLab basics tutorial! I'll grow the contents of this page as I uncover aspects of the JupyterLab interface that new users find confusing when going through the Parledoct material. 

If you came here straight from the Parledoct tutorials environment set up, be sure to head back there and learn how to [shut down Docker containers](parledoct-tutorials-environment#shut-down-environment).