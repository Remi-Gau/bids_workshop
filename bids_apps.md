<a href="../bids_workshop"><button>home</button></a>

<h1> BIDS apps </h1>

Note that if you do not have a BIDS dataset at hand, you can use one of BIDS
dataset from SPM12 tutorials.

<h2 id="TOC"> Table of content </h2>

- [Containerized BIDS apps: docker](#containerized-bids-apps-docker)
        - ["Mapping" folders inside the container](#mapping-folders-inside-the-container)
        - [Running a docker as non-root user](#running-a-docker-as-non-root-user)
- [Running a BIDS app using docker](#running-a-bids-app-using-docker)
- [App specific arguments and parameters](#app-specific-arguments-and-parameters)
        - [MRIQC](#mriqc)

BIDS apps are software packages that take a BIDS dataset as input.

They also have the same way to be called from the command line.

```bash
app_name    bids_dir output_dir analysis_level \
            [app specific arguments and parameters]
```

- `bids_dir`: directory containing the input dataset in BIDS format
- `output_dir`: directory where the output files should be stored
- `analysis_level`: this can usually be
  - `participant`
  - `group`

The list of existing BIDS apps is available here:
https://bids-apps.neuroimaging.io/apps/

Note that most BIDS apps will have a `--help` flag that you can use to get
information about all the possible arguments and parameters that you can use.

## Containerized BIDS apps: docker

To make them easier to install most BIDS apps are "containerized" using Docker.

You can learn more about containers, on
[the Turing way](https://the-turing-way.netlify.app/reproducible-research/renv/renv-containers.html)
jupyter book.

You can then download the app by "pulling its image" by running a command like:

```bash
docker pull app_repository/app_name:version
```

An image is pretty much a virtual machine that contains all the software needed
to run the app.

To see all the images you have on your computer, you can run:

```bash
docker images
```

This can give an output that looks like this:

```bash
REPOSITORY         TAG       IMAGE ID       CREATED         SIZE
nipreps/fmriprep   latest    bff6645c2142   12 days ago     12GB
nipreps/mriqc      latest    7ed42219c6f9   4 weeks ago     15.6GB
hello-world        latest    feb5d9fea6a5   12 months ago   13.3kB
```

When running a BIDS app by using its docker image you need to call `docker run`
in the following way:

```bash
docker  run \
        [docker arguments and parameters] \
        app_repository/app_name:version \
        bids_dir output_dir analysis_level \
        [app specific arguments and parameters]
```

Typical docker arguments and parameters are:

- The `-it` flag tells docker that it should open an **interactive** container
  instance.
- The `--rm` flag tells docker that the container should automatically be
  **removed** after we close docker.

Another options that can be useful for "debugging" is the `--entrypoint`
parameter followed by a command to run. That can be used to start bash terminal
in the container to explore its content.

For example, to start a bash terminal in the MRIQC container you can

```bash
docker  run -it --rm \
        --entrypoint /bin/bash \
        nipreps/mriqc:latest
```

Your terminal default prompt should have changed now show you something like
this:

```bash
root@17c3eeb3fc5a:/tmp#
```

You can get out of the container by typing `exit`.

Note that your are by default a "root" user in the container, pretty much
meaning that you have "administrator" rights when inside the container.

### "Mapping" folders inside the container

When running a BIDS app using docker, you need to "map" some folders from your
computer inside the container.

This would be done with then `-v` flag followed by

- the path to the folder on your computer
- and the **absolute** path to the folder in the container separated by a `:`.

For example:

```bash
docker  run -it --rm \
        -v /path/to/bids_dataset:/bids_dataset \
        app_repository/app_name:version \
        /bids_dataset /output_dir participant
```

Let's try again using the MRIQC image:

```bash
docker  run -it --rm \
        --entrypoint /bin/bash \
        -v ~/:/home/me \
        nipreps/mriqc:latest
```

The `~/` is a shortcut for your "home" folder on your computer.

The `/home/me` is the absolute path to the folder in the container.

Once you have run this command this should have opened a bash terminal in the
container and you can now list the content of this folder using the `ls`
command:

```bash
ls /home/me
```

and it should show you the content of your home folder on your computer.

You can now add a file to this folder and then list the content of the folder
again by rerunning the same command.

This file should also appear in the container.

Exit the container by typing `exit`.

### Running a docker as non-root user

Relaunch the MRIQC container:

```bash
docker  run -it --rm \
        --entrypoint /bin/bash \
        -v ~/:/home/me \
        nipreps/mriqc:latest
```

Now try to create an empty test file in the `/home/me` folder using the touch
command:

```bash
touch /home/me/test.txt
```

If you now try to open and edit this file on your computer you will see that you
cannot save it, because you do not have "write" permission to this file because
it was created by a root user from inside the container.

This can be annoying when working with BIDS apps because it makes it harder to
work with the files that are created by the app.

Exit the container by typing `exit` and reopen it with the following command
with the `--user "$(id -u):$(id -g)"` parameter:

```bash
docker  run -it --rm \
        --user "$(id -u):$(id -g)" \
        --entrypoint /bin/bash \
        -v ~/:/home/me \
        nipreps/mriqc:latest
```

If you now try to create an empty test file in the `/home/me`:

```bash
touch /home/me/test_2.txt
```

You will see that you can edit AND save this file.

## Running a BIDS app using docker

Now we can start running a BIDS app using docker.

The general command would look like this:

```bash
docker  run -it --rm \
        --user "$(id -u):$(id -g)" \
        -v /path/to/bids_dataset:/bids_dataset \
        -v /path/to/output_dir:/output_dir \
        app_repository/app_name:version \
        /bids_dataset /output_dir analysis_level
```

Writing those commands can be a bit tedious, and getting all the paths right is
usually the main source of errors.

To help with this, it can help to always use the same folder structure to
organize where you put your data and where you run your BIDS apps from.

You for example use something like the following one that is very similar to the
"YODA" structure recommended by the
[datalad team](https://handbook.datalad.org/en/latest/basics/101-127-yoda.html#p1-one-thing-one-dataset).

```bash
├── code
│   └── README.md
├── inputs
│   └── raw             # this is your BIDS dataset
└── outputs
```

There is template for such folder in the [`template directory`](./template).

In this case, if you are running commands from within the `code` folder, you
would run a bids docker container like this by using relative paths:

```bash
docker  run -it --rm \
        --user "$(id -u):$(id -g)" \
        -v $PWD/../inputs/raw:/bids_dataset \
        -v $PWD/../outputs:/output_dir \
        app_repository/app_name:version \
        /bids_dataset /output_dir analysis_level
```

Note that `$PWD` refers to a terminal varible that means 'Present Working
Directory'.

So to run MRIQC we would then do:

```bash
docker  run -it --rm \
        --user "$(id -u):$(id -g)" \
        -v $PWD/../inputs/raw:/bids_dataset \
        -v $PWD/../outputs:/output_dir \
        nipreps/mriqc:latest \
        /bids_dataset /output_dir participant
```

## App specific arguments and parameters

### MRIQC

Check the MRIQC options either by using the `--help` flag or checking the
[online documentation](https://mriqc.readthedocs.io/en/latest/running.html#command-line-interface).

```bash
docker  run -it --rm \
        nipreps/mriqc:latest \
        /bids_dataset /output_dir participant \
        --help
```

Some of the most frequent options would will be using are:

- `--modalities`: followed by the modality you want to analyze. For example:
  `func fmap` if you want to analyse functional data and fieldmaps, but skip
  anatomical data.

- `--participant-labels`, `--run-id`, `--task-id`: followed by respectively the
  subject labels, run index or task label you want to run.

For example:

```bash
docker  run -it --rm \
        --user "$(id -u):$(id -g)" \
        -v $PWD/../inputs/raw:/bids_dataset \
        -v $PWD/../outputs:/output_dir \
        nipreps/mriqc:latest \
        /bids_dataset /output_dir participant \
        --participant-labels 01 \
        --run-id 1 \
        --task-id rest
```
