<a href="../bids_workshop"><button>home</button></a>

<h1 style="width: 120%"> Preparing a BIDS dataset by hand and from scratch </h1>

Adapted from the [BIDS cookbook](https://doi.org/10.5281/zenodo.7111042).

<center>
<h3 style="color:red;">
  ⚠️ Note that this is purely for learning purposes and it is NOT recommended to BIDSify real datasets by hand . ⚠️
</h3>
</center>

<h2 id="TOC"> Table of content </h2>

- [Ingredients](#ingredients-and-tools)
- [Recipe](#recipe)
  - [Preheat the oven: folder structure](#preheat)
  - [Starters: anatomical data](#starters)
  - [Main course: functional data](#main-course)
- [Useful links](#useful-links)

<details><summary> <b>CLICK ME</b> </summary><br>

... to see what I hide !!!

<center>
<a href="https://twitter.com/RemiGau/status/1115513296134778880" target="_blank">
    <img src="https://pbs.twimg.com/media/D3sYRfhWkAAlevT?format=jpg&name=small" width="100%" />
</a>
</center>

</details>

<br>

## Ingredients and tools

Get them fresh from your local ~~market~~:

- MRI scanner 🧲
- EEG amplifier 🌩
- MEG squid [🦑](https://theupturnedmicroscope.com/comic/squid/)
- ...

<details><summary> <b> 🧠 some <code>source</code> data to be converted into BIDS </b> </summary><br>
  <p>
    For this workshop you can choose to work with several datasets that are adapted from some of the SPM12 tutorials.
    You can download them from OSF by using one of the following links:
  <ul>
    <li> <a href="https://files.de-1.osf.io/v1/resources/3vufp/providers/osfstorage/63306417408a27124476308d/?zip=">
      face repetition event-related design dataset</a> </li>
    <li> <a href="https://files.de-1.osf.io/v1/resources/3vufp/providers/osfstorage/6330b4f76c2401129850ad40/?zip=">
    auditory block design dataset</a> </li>
    <li> <a href="https://files.de-1.osf.io/v1/resources/3vufp/providers/osfstorage/6330bb870b727211b12b0750/?zip=">
    multimodal face dataset</a> </li>
  </ul>
    In the example below I will work with the multimodal face dataset.
  </p>
  <p>
    Very often MRI source data will be in a DICOM format and will require to be converted to nifti.
    here the MRI data is already in "4D" Nifti format <code>.nii</code> format.
    It is also gziped (<code>.nii.gz</code>) to save space.
  </p>
  <p>
    The dataset contains both structural data: should start with the letter <code>s</code>
    and functional data: should start with the letter <code>f</code>.
    It should also contain excel files that contain when each stimulus was presented to the subject.
  </p>
  <p>
    This dataset originally contains EEG, MEG and fMRI data on the same subject within the same paradigm.
    Here we are only working with the MRI data.
    We also extracted some of the information about the data from the SPM manual
    and put it into the <code>README</code>.
  </p>
  <p>
    When you have DICOM data, it is usually a good idea
    to keep the PDF of MRI acquisition parameters with your source data.
  </p>
</details>

<details><summary> <b> 🖋 a text editor </b> </summary><br>
    A good choice is to use
    <a href="https://code.visualstudio.com" target="_blank">Visual Studio code</a>
    (Notepad does not really count).
</details>

<details><summary> <b> 📥 [OPTIONAL] BIDS validator </b> </summary><br>
  <ul>
      <li>Install <a href="https://nodejs.org" target="_blank">Node.js</a> (at least version 12.12.0).</li>
      <li>Update <code>npm</code> to be at least version 7 (<code>npm install --global npm@^7</code>)</li>
      <li>From a terminal run <code>npm install -g bids-validator</code></li>
      <li>Run <code>bids-validator</code> to start validating datasets.</li>
  </ul>
  See the full instruction <a href="https://www.npmjs.com/package/bids-validator#quickstart" target="_blank">here.</a>
</details>

<br>

## Recipe

<h3 id="preheat">1. Preheat the oven: creating folders</h3>

- Create a `raw` folder to host your BIDS data and inside it create:

  - a `sourcedata` folder and put your `source` data in it
  - a subject folder: `sub-01`
    - with session folder: `ses-mri`
      - with an `anat` folder for the structural MRI data
      - with an `func` folder for the functional MRI data

<details><summary> <b>By now you should have this.</b> </summary><br>
  <pre>
  ├── sourcedata
  └── sub-01
      └── ses-mri
          ├── anat
          └── func
  </pre>
</details>

<br>

### a. Cooking is not just about the taste, it is also about how things look: naming files

- Move the `.nii.gz` file you have just created into `sub-01/ses-mri/anat`.
- Give this file a valid BIDS filename.

In case you do not remember which suffix to use and which entities are required
or optional, the BIDS specification has:

- [filename templates](https://bids-specification.readthedocs.io/en/stable/04-modality-specific-files/01-magnetic-resonance-imaging-data.html#anatomy-imaging-data)
  at the beginning of the section for each imaging modality,
- a summary
  [entity table](https://bids-specification.readthedocs.io/en/stable/99-appendices/04-entity-table.html).

#### b. Taste your dish while you prepare it: using the BIDS validator

Try it directly in your
<a href="https://bids-standard.github.io/bids-validator/" target="_blank">browser.</a>

#### c. Season to taste: adding missing files

- `README`
- `dataset_description.json`

You can get template content for those files from:

- from the [BIDS specification](https://bids-specification.readthedocs.io) (use
  the search bar)
- the BIDS starter
  [templates](https://github.com/bids-standard/bids-starter-kit/tree/main/templates)

> Suggestion:
>
> Add the "table" output of the BIDS validator to your README to give a quick
> overview of the content of your dataset.

<details><summary> 🚨 About JSON files </summary><br>
JSON files are text files to store <code>key-value</code> pairs.

<p>
  More information on how read and write JSON files is available on the
  <a  href="https://bids-standard.github.io/bids-starter-kit/folders_and_files/metadata.html#json-files"
      target="_blank">
    BIDS stater kit
  </a>
  and also
  <a  href="https://bids-standard.github.io/stats-models/json_101.html"
      target="_blank">
    on the bids stats model website.
  </a>
</p>
<p>JSON CONTENT EXAMPLE:
  <pre>
  {
    "key": "value",
    "key2": "value2",
    "key3": {
      "subkey1": "subvalue1"
    },
    "array": [ 1, 2, 3 ],
    "boolean": true,
    "color": "gold",
    "null": null,
    "number": 123,
    "object": {
      "a": "b",
      "c": "d"
    },
    "string": "Hello World"
  }</pre>
</p>
</details>

#### d. Icing on the cake: adding extra information

- Add a `T1w.json` file. Use information from `README` to create it.
- Add a participants `participants.tsv`. You can use excel or google sheet to
  create them.

<details><summary> 🚨 About TSV files </summary><br>
A Tab-Separate Values (TSV) file is a text file where tab characters
(<code>\t</code>) separate fields that are in the file.

It is structured as a table, with each column representing a field of interest,
and each row representing a single data point.

<p>More information on how read and write TSV files is available on the
<a href="https://github.com/bids-standard/bids-starter-kit/wiki/Metadata-file-formats#tsv-files"
      target="_blank"> BIDS stater kit </a>
</p>

<pre>TSV CONTENT EXAMPLE:

participant_id\tage\tgender\n
sub-01\t34\tM</pre>
</details>

<details><summary> <b>By now you should have this.</b> </summary><br>
  <pre>
  ├── sourcedata
  ├── sub-01
  │   └── ses-mri
  │       ├── anat
  │       │   ├── sub-01_ses-mri_T1w.json
  │       │   └── sub-01_ses-mri_T1w.nii
  │       └── func
  ├── README
  ├── participants.tsv
  ├── participants.json
  └── dataset_description.json
  </pre>
</details>

<br>

<h3 id="main-course">3. Main course: converting the functional MRI files</h3>

- Give the output files valid BIDS filenames. You will need to use `task` and
  the `run` entities.
- Use the BIDS validator and any eventual missing file (like `*_bold.json`
  file).
- Using the excel files, create `events.tsv` for each run.
- Put the `events.tsv` files in the func folders and give them BIDS valid names.
- Remove duplicate `json` files to make use of the
  ["inheritance principle"](https://bids-specification.readthedocs.io/en/stable/02-common-principles.html#the-inheritance-principle)

<details><summary> <b>By now you should have this.</b> </summary><br>
  <pre>
  ├── sourcedata
  ├── sub-01
  │   └── ses-mri
  │       ├── anat
  │       │   └── sub-01_ses-mri_T1w.nii
  │       └── func
  │           ├── sub-01_ses-mri_task-FaceSymmetry_run-1_bold.nii
  │           ├── sub-01_ses-mri_task-FaceSymmetry_run-1_events.tsv
  │           ├── sub-01_ses-mri_task-FaceSymmetry_run-2_bold.nii
  │           └── sub-01_ses-mri_task-FaceSymmetry_run-2_events.tsv
  ├── README
  ├── participants.tsv
  ├── participants.json
  ├── T1w.json
  ├── task-FaceSymmetry_bold.json
  └── dataset_description.json
  </pre>
</details>

## Useful links

- [BIDS specification](https://bids-specification.readthedocs.io)
- [BIDS starter kit](https://github.com/bids-standard/bids-starter-kit)
  - [templates](https://github.com/bids-standard/bids-starter-kit/tree/main/templates)
  - [wiki](https://github.com/bids-standard/bids-starter-kit/wiki)
    - [validator](https://github.com/bids-standard/bids-starter-kit/wiki/bids-validator-info)
- [BIDS validator](https://github.com/bids-standard/bids-validator)
- [BIDS examples](https://github.com/bids-standard/bids-examples)

<hr>
<button><a href="#TOC">back to the top</a></button>
