# Linux

## VSCode

Go to the [visual studio code website](https://code.visualstudio.com/) and click
the download button for either the `.deb` (Ubuntu, Debian) or the `.rpm`
(Fedora, CentOS) file.

Double-click the downloaded file to install VSCode. (You may be prompted to type
your administrator password during the install).

## Python

1. Open a new terminal and type the following lines (separately) into the
   terminal, pressing `Enter` after each one:

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
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
1. Re-open a new terminal. You should also see a `(base)` at the beginning of
   your prompt on your terminal.Type `which python` into the terminal and it
   should return a path (e.g., `/home/$USER/miniconda3/bin/python`).
   - If you do not see a path like this then please try typing `conda init`,
     closing your terminal, and repeating this step. If your issue is still not
     resolved skip the following step and contact Rémi so he can help you
     directly.
1. Type the following to remove the installation script that was downloaded:

```bash
rm ./Miniconda3-latest-Linux-x86_64.sh
```

## Docker

1. You will be following different instructions depending on your distro
   ([Ubuntu](https://docs.docker.com/engine/install/ubuntu/),
   [Debian](https://docs.docker.com/engine/install/debian/),
   [Fedora](https://docs.docker.com/engine/install/fedora/),
   [CentOS](https://docs.docker.com/engine/install/centos/)). Make sure to
   follow the “Install using the repository” method!
1. Once you’ve installed Docker make sure to follow the
   [post-install instructions](https://docs.docker.com/engine/install/linux-postinstall/)
   as well. You only need to do the “Manage Docker as a non-root user” and
   “Configure Docker to start on boot” steps.
1. Open a new terminal and type `docker run hello-world`. A brief introductory
   message should be printed to the screen.
