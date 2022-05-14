---
title : Parledoct tutorials environment
notetype : feed
date : 14-05-2022
---

- Once you have [Docker installed and running](environment-setup-with-docker), download and unzip the [Parledoct tutorials](https://github.com/parledoct/tutorials/archive/refs/heads/main.zip) repository

- In your Finder (macOS) or File Explorer (Windows), go to the folder where you unzipped the repository. The folder will be called `tutorials-main` and have a file called `docker-compose.yml`. Make note of the path to this folder, e.g.:
    - macOS/Linux: `/users/nay/Downloads/tutorials-main`

        Tip: if you have Path Bar displayed (View > Show Path Bar) in Finder, you can right click the `tutorials-main` folder and click `Copy "tutorials-main" as Pathname`.

    - Windows: `C:\Users\nay\Downloads\tutorials-main\tutorials-main`

        Tip: if you are in the folder that contains `docker-compose.yml`, you can also right-click on the address bar, and click `Copy address as text`.

- Launch the Terminal app (macOS) or Command Prompt (Windows) and then to **c**hange **d**irectory to the folder noted above type in:
    ```bash
    # macOS/Linux
    cd /users/nay/Downloads/tutorials-main
    
    # Windows
    cd C:\Users\nay\Downloads\tutorials-main\tutorials-main
    ```
- Let's just to double check that `docker-compose.yml` is inside the folder we've just browsed to by typing:

    ```bash
    # macOS/Linux
    ls

    # Windows
    dir
    ```

- If Docker Desktop is installed and running, you should be able to type:

    ```bash
    docker-compose run --rm --service-ports base
    ```

    **Note**. The very first time you run this command Docker will need to download (and sometimes setup) the computation environment and this may take some time.

    Once the environment is set up, you will see a message that looks like:

    <pre>
    [I 16:36:21.830 LabApp] Writing notebook server cookie secret to /home/jovyan/.local/share/jupyter/runtime/notebook_cookie_secret
    [I 16:36:24.204 LabApp] JupyterLab extension loaded from /opt/conda/lib/python3.8/site-packages/jupyterlab
    [I 16:36:24.205 LabApp] JupyterLab application directory is /opt/conda/share/jupyter/lab
    [I 16:36:24.218 LabApp] Serving notebooks from local directory: /home/jovyan/work
    [I 16:36:24.218 LabApp] Jupyter Notebook 6.1.4 is running at:
    [I 16:36:24.218 LabApp] http://64a3b04aa05f:8888/?token=86acc1e58d49d087468c9ae0c441464f70f5aa9db67121e4
    [I 16:36:24.218 LabApp]  or http://127.0.0.1:8888/?token=86acc1e58d49d087468c9ae0c441464f70f5aa9db67121e4
    [I 16:36:24.219 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
    [C 16:36:24.235 LabApp]

        To access the notebook, open this file in a browser:
            file:///home/jovyan/.local/share/jupyter/runtime/nbserver-1-open.html
        Or copy and paste one of these URLs:
            http://64a3b04aa05f:8888/?token=86acc1e58d49d087468c9ae0c441464f70f5aa9db67121e4
        or http://127.0.0.1:8888/?token=86acc1e58d49d087468c9ae0c441464f70f5aa9db67121e4
    </pre>

    If you copy and paste one of the last URLs **including the `token=...`** into your browser, e.g.:

    ```
    http://127.0.0.1:8888/?token=86acc1e58d49d087468c9ae0c441464f70f5aa9db67121e4
    ```

    You will see the `JupyterLab` interface that looks like:

    <p style="text-align:center">
        <img alt="jupyter-lab-tutorials-env" src="https://user-images.githubusercontent.com/9938298/168440902-c787d289-d006-4e35-bf3f-3e51cca34f5f.png">
    </p>

    To open an interface where you can start typing Python commands, click the `Python 3` option under `Console`.

    <p style="text-align:center">
        <img alt="Screen Shot 2022-05-14 at 9 53 03 AM" src="https://user-images.githubusercontent.com/9938298/168441288-75b33570-b344-403f-8bd7-80824869e2ef.png">
    </p>

    You can try typing a Python command such as `print("Hello!")` in the input area at the bottom then press `Shift + Enter` to execute the command(s).

 - To exit the environment, close the browser window and then in the command line interface (i.e. where you copied the URL from) press `Ctrl + C` to shut down the Docker container.

    Congrats! You're now ready to perform some speech processing tasks!
