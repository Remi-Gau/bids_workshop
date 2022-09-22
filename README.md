- [Content](#content)
  - ["Theory": The BIDS data management principles](#theory-the-bids-data-management-principles)
  - [BIDS conversion by hand](#bids-conversion-by-hand)
  - [Automated BIDS conversion](#automated-bids-conversion)
  - [Using BIDS apps](#using-bids-apps)
- [Notes](#notes)
- [Not covered](#not-covered)
- [Pre-requisites](#pre-requisites)
- [Softwares to install](#softwares-to-install)
  - [Visual studio code](#visual-studio-code)
  - [Python](#python)
  - [Dcm2niix](#dcm2niix)
  - [BIDScoin](#bidscoin)
  - [Docker](#docker)
  - [Optional: VSCode extensions](#optional-vscode-extensions)

<br>

The
[brain imaging data structure (BIDS)](https://bids-specification.readthedocs.io/en/latest/)
is fast becoming the main standard to organize raw neuroimaging data.

Having your data in BIDS offers many downstream advantages to users: use of
standardized pipelines, facilitate data sharing and reuse…

But getting your data to be BIDS compliant and using can also be source of much
frustration. Especially when it is the first time you are dealing with
neuroimaging data.

This workshop aims to lessen the frustration level you will experience when
curating and using your data.

## Content

This workshop is divided in 4 parts.

### "Theory": The BIDS data management principles

Remembering and following a rule is always easier when you understand why the
rule was put in place: this first part explains some of the reasons why BIDS is
structured the way it is.

### BIDS conversion by hand

Using automated BIDS converters is recommended, but using them too early can
turn BIDS into a black box. So doing the conversion of one small dataset by hand
helps better understand "what data/information goes where".

### Automated BIDS conversion

Converting large numbers files quickly becomes impractical and error prone when
by hand. So learning to use a bids converters should be the next logical step.

### Using BIDS apps

To wrap up it is good use BIDS applications on the dataset we have created.

## Notes

This workshop is mostly geared for the MRI part of BIDS but many principles of
BIDS apply to all imaging modalities.

We will finish the workshop with a "ask me anything about BIDS" session where
you can come with more specific questions as they relate to your use case.

## Not covered

The follow issues are be covered in this workshop:

- Where to store data
- Ethical issues around data sharing (notably GDPR)
- Data version control (for example with Datalad)

## Pre-requisites

- Coding experience: `none`
- Prior experience with neuroimaging data: `none`
- Data: feel free to bring your own dataset to work on but if you do not have
  data, some data is provided.
- Data type
  - fMRI and resting state
  - Anat
  - DTI
  - Physio / eyetracking

## Softwares to install

Below is a list of softwares, you will need to install to follow the workshop.

If you have troubles when installing any of these, please contact me before the
workshop and come to the "open-office" that will happen before the workshop`
starts.

For more details and trouble shooting eventual installation problems, check the
page dedicated to your operating system:

- [Windows](./troubleshooting_install/win.md)
- [MacOs](./troubleshooting_install/macos.md)
- [Linux](./troubleshooting_install/linux.md)

### Visual studio code

Install [Visual studio code](https://code.visualstudio.com/) as your code
editor.

### Python

If you have never used Python before, install it with the installer adapted to
your system (32 vs 64 bit, Apple M1 vs Intel...)
[miniconda](https://docs.conda.io/en/latest/miniconda.html#latest-miniconda-installer-links).

- For Windows, you should only have to run the `.exe` installer.
- For MacOs, you can try to simply use the `.pkg` installer adapted for your
  computer. If that does not work try the method described
  [here](./troubleshooting_install/macos.md#python).
- For Linux, you should use the bash installer as described
  [here](./troubleshooting_install/linux.md#python).

### Dcm2niix

Install the DICOM to Nifti converter:
[Dcm2niix](https://www.nitrc.org/plugins/mwiki/index.php/dcm2nii:MainPage#Download)

If installing with
[the correct installer for your operating system](https://github.com/rordenlab/dcm2niix/releases)
does not work, try installing
[MRIcroGL](https://www.nitrc.org/frs/?group_id=889) that should ship with
`Dcm2niix`.

### BIDScoin

Install the [BIDScoin converter](https://bidscoin.readthedocs.io/en/stable/)

If you have installed Miniconda properly, you should only need to open a
Terminal (for Linux and MacOS) or an Anaconda Powershell Prompt (for Windows)
and type:

```bash
pip install bidscoin
```

### Docker

Install [Docker Desktop](https://www.docker.com/)

To check that things are properly installed, open a Terminal (for Linux and
MacOS) or an Anaconda Powershell Prompt (for Windows) and type the following to
make sure everything works:

```bash
docker run hello-world
```

A brief introductory message should then be printed to the screen.

Once you have done this, you can download the "images" of 2 bids app: be careful
that they may take quite a bit of space on your hard drive (about 12 Go each),
so make sure you have enough free space.

- download the latest version of the
  [MRIQC](https://mriqc.readthedocs.io/en/latest/) docker image for quality
  control.

```bash
docker pull nipreps/mriqc:latest
```

- download the latest version of the [fmriprep](https://fmriprep.org/en/stable/)
  docker image for preprocessing.

```bash
docker pull nipreps/fmriprep:latest
```

Note that to run fmriprep, you will also need a license for Freesurfer that you
can get for free here: https://surfer.nmr.mgh.harvard.edu/registration.html.

### Optional: VSCode extensions

Those extensions are not needed but may help you when dealing with python code
or json files in general.

1. Open Visual studio code
2. Press `Ctrl/Cmd+Shift+X` to open the extensions side bar.
3. A new panel should appear on the side of the screen with a search bar. Search
   for each of the following extensions and press `Install` for the first entry
   that appears. (The author listed for all of these extensions should be
   `Microsoft`.)
   - `Python` (n.b., you will need to reload VSCode after installing this)
   - `json`

<footer>
    <hr>
    <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">
        <img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png"/>
    </a>
</footer>
