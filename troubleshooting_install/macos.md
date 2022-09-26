<h1> MacOS </h1>

<h2 id="TOC"> Table of content </h2>

- [VSCode](#vscode)
- [Python](#python)
- [Docker](#docker)

## VSCode

1. Go to the [visual studio code website](https://code.visualstudio.com/) and
   click the download button.
1. Unzip the downloaded file (e.g., `VSCode-darwin-stable.zip`) and moving the
   resulting `Visual Studio Code` file to your Applications directory.

## Python

1. Open a new terminal and type the following lines (separately) into the
   terminal, pressing `Enter` after each one:

```bash
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh
bash Miniconda3-latest-MacOSX-x86_64.sh
```

1. A license agreement will be displayed and the bottom of the terminal will
   read `--More--`. Press `Enter` or the space bar until you are prompted with
   "Do you accept the license terms? [yes|no]." Type `yes` and then press
   `Enter`
1. The installation script will inform you that it is going to install into a
   default directory (e.g., `/home/$USER/miniconda3`). Leave this default and
   press `Enter`.
1. When you are asked "Do you wish the installer to initialize Miniconda3 by
   running conda init? [yes|no]," type `yes` and press `Enter`. Exit the
   terminal once the installation has finished.
1. Re-open a terminal. You should also see a `(base)` at the beginning of your
   prompt on your terminal. Type `which python` into the terminal and it should
   return a path (e.g., `/home/$USER/miniconda3/bin/python`).
   - If you do not see a path like this then please try typing `conda init`,
     closing your terminal, and repeating this step. If your issue is still not
     resolved skip the following step and contact and contact Rémi so he can
     help you directly.
1. Type the following to remove the installation script that was downloaded:

```bash
rm ./Miniconda3-latest-MacOSX-x86_64.sh
```

## Docker

1. Go to this
   [website](https://hub.docker.com/editions/community/docker-ce-desktop-mac/)
   and press `Get Docker`.
1. Open the `Docker.dmg` file that is downloaded and drag and drop the icon to
   the Applications folder
1. Open the Docker application and enter your password. An icon will appear in
   the status bar in the top-left of the screen. Wait until it reads
   `Docker Desktop is now up and running!`
1. Open a new terminal and type `docker run hello-world`. A brief introductory
   message should be printed to the screen.

The above step-by-step Docker instructions are distilled from
[here](https://docs.docker.com/docker-for-mac/install/). If you have questions
during the installation procedure please check that link for potential answers!

<hr>
<button><a href="#TOC">back to the top</a></button>
