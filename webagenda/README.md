## Generating the Schedule for the Website

This directory contains the data files and code used to generate the schedule for the NAACL 2019 official website. To do this, it relies on the code and data provided by the [NAACL 2019 schedule repository](https://github.com/naacl-org/naacl-schedule-2019), which is used as a submodule in this repository. This is structured in this way because it makes it much easier to allow syncing any changes in the data files much easier across both the websites and the apps and also allows for code sharing.

### Setting Up

In order to run the code in this repository, you first need to create a [conda](https://conda.io/en/latest/) environment. First you should [install miniconda](https://conda.io/en/latest/miniconda.html). Once you have it installed, run the following to create the Python environment:

```bash
conda create -n naacl2019 -c conda-forge --file agenda/requirements.txt
```

This will create a conda environment called `naacl2019` which you will need to "activate" before running any of the code. To activate this environment, run:

```bash
conda activate naacl2019
```

### Contents 

There are two main files in this directory:

1. `generate.py` : This is the main driver script that generates the schedule markdown file for the repository. This script uses the classes defined in the `orderfile.py` module from the NAACL 2019 schedule repository that is integrated as a submodule in this repository under the `agenda` directory. It takes as input a single JSON configuration file that contains the following fields:
    
    - `order_file` : The manually combined order file for the main conference.

    - `mapping_file` : File mapping the anthology IDs to the START / order file IDs for the main conference.

    - `xml_file` : The XML file from the Anthology containing the titles, authors, abstracts, and anthology URLs for the items in the main conference.

    - `extra_metadata_file` : An optional TSV file containing the title, authors, and abstracts for the main conference items that are not in the anthology (e.g., TACL papers, SRW non-archival workshop papers, etc.)
    
    - `plenary_info_file` : Another optional TSV file containing additional info about some of the plenary sessions (e.g., keynote abstracts etc.)

    - `pdf_icons` : A boolean indicating whether to generate icons in the schedule that link to anthology PDFs or PDFs specified in the extra metadata file.

    - `video_icons` : A boolean indicating whether to generate icons in the schedule that link to the talk videos hosted on Vimeo/YouTube or other video platforms.

For more details on how this script works, please refer to the code and the comments in the script.

2. `frontmatter.md` : This markdown document contains the Jekyll frontmatter that needs to be appended on top of the HTML generated by `generate.py` to generate a valid markdown file that will be rendered properly by Jekyll. This file expected by `generate.py` to live in the same directory without being provided explicitly as input.

### Generating the Schedule

The following command will generate the schedule markdown file at `_pages/program/schedule.md` for the website without the video and paper icons. This command should be run in the top level of the cloned repository:

```
python webagenda/generate.py webagenda/config.json _pages/output/schedule.md
```

The configuration file `config.json` is checked into the repository and looks like this:

```json
{
    "order_file": "agenda/data/order/manually_combined_order",
    "mapping_file": "agenda/data/mapping/manually_combined_id_map.txt",
    "xml_file": "agenda/data/xml/N19.xml",
    "extra_metadata_file": "agenda/data/extra-metadata/main.tsv",
    "plenary_info_file": "agenda/data/plenary-info.tsv",
    "pdf_icons": false,
    "video_icons": false
}
```

To add the PDF and video icons where available, modify the above config file to have the values for `pdf_icons` and `video_icons` fields to be `true`. 