<a href="../bids_workshop"><button>home</button></a>

<h1> bidscoin tutorial </h1>

If you have set up a special python environment for bidscoin, activate it. For
example with `conda activate bidscoin`.

<h2 id="TOC"> Table of content </h2>

- [DEMO 1: using bidscoin data](#demo-1-using-bidscoin-data)
  - [Get the data.](#get-the-data)
  - [Reorganize the DICOM data](#reorganize-the-dicom-data)
  - [Read DICOM metadata and initialize the mapping](#read-dicom-metadata-and-initialize-the-mapping)
  - [Fieldmaps](#fieldmaps)
  - [bidsmap.yml](#bidsmapyml)
  - [Run the conversion](#run-the-conversion)
  - [Post-processing](#post-processing)
  - [Validate the results](#validate-the-results)
  - [Inspect file types we have not seen before](#inspect-file-types-we-have-not-seen-before)
- [DEMO 2: using the dcm2bids test data](#demo-2-using-the-dcm2bids-test-data)
  - [Get the data](#get-the-data-1)
    - [Option 1](#option-1)
    - [Option 2](#option-2)
    - [Option 3](#option-3)
  - [Dataset description](#dataset-description)
  - [Sort DICOMS](#sort-dicoms)
  - [Collect DICOM info](#collect-dicom-info)
  - [Create the bidsmap](#create-the-bidsmap)
  - [Run the conversion](#run-the-conversion-1)

## DEMO 1: using bidscoin data

This is a massively reduced version of the
[original bidscoin tutorial](https://bidscoin.readthedocs.io/en/stable/tutorial.html).
Please refer back to the original tutorial for more information.

There is also a [video tutorial](https://youtu.be/aRDK4Gj5qzE?t=817) about it

### Get the data.

```bash
bidscoin --download .
cd bidscointutorial
```

This is a small phantom dataset.

Note that in this dataset the `raw` folder contains the DICOM files, and the
`bids_ref` folder contains the BIDS dataset you are supposed to get after
conversion.

### Reorganize the DICOM data

bidscoin has strict requirements on how the source DICOM data should be
organized.

If some DICOM data is not properly organized, this can be done with the
`dicomsort` command:

```bash
dicomsort raw/sub-002/ses-01/
```

You can also extract some specific DICOM metadata with the `rawmapper` command:

```bash
rawmapper raw -f "AcquisitionDate" "EchoTime"
```

### Read DICOM metadata and initialize the mapping

```bash
bidsmapper raw bids
```

Opens a GUI to create a mapping to say how each DICOM file should be renamed:

- T1 scan acquisition label `mprage`
- task label is `reward` for the acq-mb8 scans
- task label is `stop` for the acq-mb3me3 scans

### Fieldmaps

Add a search pattern to the `IntendedFor` field such that:

- the first fieldmap will select your `reward` runs,
- and the second fieldmap your `stop`.

Make sure that the dcm2bids command is correct in the `Options` tab.

Save it in `bids/code/bidscoin/bidsmap.yaml`.

### bidsmap.yml

Note that this YAML file (Yet Another Markup Language). You can read a quick
intro about this file format
[on the Turing-way jupyter book](https://the-turing-way.netlify.app/reproducible-research/renv/renv-yaml.html#yaml-files)
or a longer one [here](https://learnxinyminutes.com/docs/yaml/). If you are
working with VS code you can install the
[YAML extension](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml).

If you need to re-edit the bidsmap, you can relaunch the GUI with:

```bash
bidseditor bids -b bids/code/bidscoin/bidsmap.yaml
```

You can also
[edit the default bidsmap or create a new one](https://bidscoin.readthedocs.io/en/stable/advanced.html#customized-template-bidsmap)
to adapt it to the needs of your lab or your MRI center.

### Run the conversion

```bash
bidscoiner raw bids -p 001 002
```

### Post-processing

This data set has functional multi-echo data and bidscoins offers a way to
combine them.

```bash
echocombine bids "func/*task-reward*echo-1*" -p 001 002
echocombine bids "func/*task-stop*echo-1*" -p 001 002
```

You will also need to manually edit some files:

- `README`
- `dataset_description.json`
- `participants.json` that contains details about each column of the
  `participants.tsv` file.

### Validate the results

Use the [BIDS validator](https://bids-standard.github.io/bids-validator/).

### Inspect file types we have not seen before

- [`scans.tsv`](https://bids-specification.readthedocs.io/en/latest/03-modality-agnostic-files.html#scans-file)
- [`.bidsignore`](https://github.com/bids-standard/bids-validator#bidsignore)

## DEMO 2: using the dcm2bids test data

This part was adapted from the
[dcm2bids converter tutorial](https://unfmontreal.github.io/Dcm2Bids/docs/tutorial/first-steps/)
to reuse their demo data.

### Get the data

#### Option 1

Download and unzip the data:

https://github.com/neurolabusc/dcm_qa_nih/archive/refs/heads/master.zip

#### Option 2

From the terminal:

```bash
wget -O dcm_qa_nih-master.zip https://github.com/neurolabusc/dcm_qa_nih/archive/refs/heads/master.zip
unzip dcm_qa_nih-master.zip -d sourcedata/
mv sourcedata/dcm_qa_nih-master sourcedata/dcm_qa_nih
```

#### Option 3

Clone the repository:

```bash
git clone https://github.com/neurolabusc/dcm_qa_nih/ sourcedata/dcm_qa_nih
```

### Dataset description

The data you want to focus on are in the `In` folder.

You have data coming from a Siemens scanner and data coming from a General
Electrics scanner.

### Sort DICOMS

```bash
dicomsort sourcedata/dcm_qa_nih/In/20180918GE
dicomsort sourcedata/dcm_qa_nih/In/20180918Si
```

Let's also rename the folders and add the the prefix `sub-`.

- `20180918GE` -> `sub-GE`
- `20180918Si` -> `sub-Si`

### Collect DICOM info

```bash
rawmapper sourcedata/dcm_qa_nih/In -f "SeriesDescription"
```

You are mostly interested in those sequences:

- 004_In_DCM2NIIX_regression_test_20180918114023 003_In_EPI_PE=AP_2018091812123

  - `SeriesDescription`: "Axial EPI-FMRI (Interleaved I to S)\*"
  - will become a functional file with the task label `rest`

- 004_In_EPI_PE=PA_2018091812123

### Create the bidsmap

```bash
bidsmapper sourcedata/dcm_qa_nih/In raw
```

### Run the conversion

```bash
bidscoiner sourcedata/dcm_qa_nih/In raw -p GE Si
```

<hr>
<button><a href="#TOC">back to the top</a></button>
