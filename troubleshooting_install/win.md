<h1> Windows </h1>

<h2 id="TOC"> Table of content </h2>

- [VSCode](#vscode)
- [Python](#python)
- [The hard way](#the-hard-way)
   - [Windows Subsystem for Linux (WSL)](#windows-subsystem-for-linux-wsl)
   - [Docker](#docker)

## VSCode

1. Go to the [visual studio code website](https://code.visualstudio.com/) and
   click the download button, then run the `.exe` file.
1. Leave all the defaults during the installation with the following exception:
   - Please make sure the box labelled
     `Register Code as an editor for supported file types` is selected

## Python

1. Get the correct `.exe` installer for your system (32 or 64 bit) from
   [here](https://docs.conda.io/en/latest/miniconda.html#latest-miniconda-installer-links).
1. Double click on the installer to launch it.
1. Accept the license agreement when it is displayed.
1. The installation will inform you that it is going to install into a default
   directory. Leave this default.
1. You can now remove the installer.

## The hard way

If you are encountering issues installing or running Docker, you may have to go
via another way and use the Windows Subsystem for Linux that allows you to run
Linux on your windows computer.

### Windows Subsystem for Linux (WSL)

1. Search for `Windows Powershell` in your applications; right click and select
   `Run as administrator`. Select `Yes` on the prompt that appears asking if you
   want to allow the app to make changes to your device.
2. Type the following into the Powershell and then press `Enter`:

   ```bash
   Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
   ```

3. Press `Enter` again when prompted to reboot your computer.
4. Once your computer has rebooted, open the Microsoft Store and search for
   "Ubuntu." Install the program labelled "Ubuntu 18.04" (NOT "Ubuntu 20.04"
   (bug in gpg that makes git clone from https fail) NOT "Ubuntu 16.04" NOT
   "Ubuntu") by clicking the tile, pressing `Get`, and then `Install`.
5. Search for and open Ubuntu from your applications. There will be a slight
   delay (of a few minutes) while it finishes installing.
6. You will be prompted to `Enter new UNIX username`. You can use any
   combination of alphanumeric characters here for your username, but a good
   choice is `<first_initial><last_name>` (e.g., `jsmith` for John Smith). You
   will then be prompted to enter a new password. (Choose something easy to
   remember as you will find yourself using it frequently.)
7. Right click on the top bar of the Ubuntu application and select "Properties".
   Under the "Options" tab, under the "Edit Options" heading, make sure the box
   reading "Use Ctrl+Shift+C/V as Copy/Paste" is checked. Under the "Terminal"
   tab, under the "Cursor Shape" heading, make sure the box reading "Vertical
   Bar" is checked. Press "Okay" to save these settings and then exit the
   application.

(The above step-by-step WSL instructions are distilled from
[here](https://learn.microsoft.com/en-us/windows/wsl/install). If you have
questions during the installation procedure those resources may have answers!)

From this point on whenever the instructions specify to "open a terminal" please
assume you are supposed to open the Ubuntu application.

### Docker

Unfortunately, Docker for Windows is a bit of a mess. The recommended version of
Docker to install varies dramatically depending not only on which version of
Windows you have installed (e.g., Windows 10 Home versus
Professional/Enterprise/Education), but also which _build_ of Windows you have.
As such, developing a comprehensive set of instructions for installing Docker is
rather difficult.

Having said that, if you're lucky enough to have Windows 10
Professional/Enterprise/Education (i.e. it is _not_ Windows 10 Home), and you
don't use VirtualBox (or if you don't know what VirtualBox is), then you can
download and install
[Docker for Windows Desktop](https://docs.docker.com/docker-for-windows/install/).
Follow the instructions at that link.

If you are using Windows 10 Home, or any other version of Windows, then the best
option may be to install
[Docker Toolbox for Windows](https://docs.docker.com/toolbox/toolbox_install_windows/).

Instructions for installing Docker Toolbox for Windows:

1. Download the latest
   [Docker Toolbox installer](https://github.com/docker/toolbox/releases/download/v19.03.1/DockerToolbox-19.03.1.exe)
   (note: that link will automatically download the file)
1. Run the downloaded `.exe` file and leave all the defaults during the
   installation procedure. Click `Yes`on the prompt that appears asking if the
   application can make changes to your computer.
1. Search for and open the newly-installed "Docker Quickstart" application.
   Again, click `Yes`on the prompt that appears asking if the application can
   make changes to your computer. The application will do a number of things to
   finish installing and setting up Docker.
1. Once you see a `$` prompt type `docker run hello-world`. A brief introductory
   message should be printed to the screen.
1. Close the "Docker Quickstart" application and open a terminal (i.e., the
   Ubuntu application).
1. Copy-paste the following commands. You will be prompted to enter your
   password once.

   ```bash
   # Update the apt package list.
   sudo apt-get update -y
   # Install Docker's package dependencies.
   sudo apt-get install -y \
       apt-transport-https \
       ca-certificates \
       curl \
       software-properties-common
   # Download and add Docker's official public PGP key.
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   # Verify the fingerprint.
   sudo apt-key fingerprint 0EBFCD88
   # Add the `stable` channel's Docker upstream repository.
   sudo add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable"
   # Update the apt package list (for the new apt repo).
   sudo apt-get update -y
   # Install the latest version of Docker CE.
   sudo apt-get install -y docker-ce
   # Allow your user to access the Docker CLI without needing root access.
   sudo usermod -aG docker $USER
   ```

1. Close and re-open the terminal.
1. Type `pip install docker-compose`.
1. Type `powershell.exe "docker-machine config"`. You should get output similar
   to the following:

   ```bash
   --tlsverify
   --tlscacert="C:\\Users\\<YOUR_USERNAME>\\.docker\\machine\\machines\\default\\ca.pem"
   --tlscert="C:\\Users\\<YOUR_USERNAME>\\.docker\\machine\\machines\\default\\cert.pem"
   --tlskey="C:\\Users\\<YOUR_USERNAME>\\.docker\\machine\\machines\\default\\key.pem"
   -H=tcp://xxx.xxx.xx.xxx:xxxx
   ```

   where `<YOUR_USERNAME>` will have an actual value (likely your Windows
   username), and `tcp=xxx.xxx.xx.xxx:xxx` will be a series of numbers. If you
   don't get this output then something has gone wrong. Please make sure you
   were able to run the `docker run hello-world` command, above. If you were and
   you still don't receive this output, please contact and contact Rémi so he
   can help you directly.

1. You will use the the outputs of the above command to modify the commands
   below before running them in the terminal. First, take the numbers printed in
   place of the `x`s on the output of the line `-H=tcp://xxx.xxx.xx.xxx:xxxx`
   from above and replace the placeholder `xxx.xxx.xx.xxx:xxxx` on the first
   command below (`export DOCKER_HOST`). Second, take whatever value is printed
   in place of `<YOUR_USERNAME>` above and replace the `<YOUR_USERNAME>`
   placeholder on the second command below (`export DOCKER_CERT_PATH`). Once you
   have updated the commands appropriately, copy and paste them into the
   terminal:

   ```bash
   echo "export DOCKER_HOST=tcp://xxx.xxx.xx.xxx:xxxx" >> $HOME/.bashrc
   echo "export DOCKER_CERT_PATH=/mnt/c/Users/<YOUR_USERNAME>/.docker/machine/certs" >> $HOME/.bashrc
   echo "export DOCKER_TLS_VERIFY=1" >> $HOME/.bashrc
   ```

1. Close and re-open a terminal (i.e., the Ubuntu application). Type
   `docker run hello-world`. The same brief introductory message you saw before
   should be printed to the screen.

Note: If you restart your computer (or somehow otherwise shut down the Docker
VM) you will need to re-open the "Docker Quickstart" application and wait until
you see the `$` prompt again before your `docker` commands will work again! If
you are having problems running `docker` commands in the terminal, try
re-opening the "Docker Quickstart" application.

(The above step-by-step instructions are distilled from
[here](https://docs.docker.com/toolbox/toolbox_install_windows/) and
[here](https://medium.com/@joaoh82/setting-up-docker-toolbox-for-windows-home-10-and-wsl-to-work-perfectly-2fd34ed41d51).
If you have questions during the installation procedure please check those links
for potential answers!)

<hr>
<button><a href="#TOC">back to the top</a></button>
