<a href=".."><button>home</button></a>

# BIDS apps

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

```bash
docker  run -it --rm \
        -v /path/to/bids_dataset:/bids_dataset \
        -v /path/to/output_dir:/output_dir \
        -v /path/to/work_dir:/work_dir \
        app_repository/app_name:version \
        /bids_dataset /output_dir participant
```

When running a BIDS app, you need to specify the input and output directories
