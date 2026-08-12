# Huequito

Jacobo de la Cuesta-Zuluaga. August 2026.

`Huequito` is the workflow for the base calling and genome assembly
of Oxford Nanopore reads of the Maier Lab. It takes the raw signal files
(`pod5`) produced by the sequencer and returns an assembled, annotated
bacterial genome. For base calling it uses the `dorado` basecaller available
[here](https://software-docs.nanoporetech.com/dorado/latest/). For genome assembly,
it implements the `nf-core` pipeline `bacass` available
[here](https://nf-co.re/bacass/2.6.0/).

The notebooks walk you through the download of the software, the
creation of files and the execution of the pipelines.

## Quickstart

1. Clone this repository and move into it:

    ```bash
    git clone https://github.com/jacodela/Huequito.git
    cd Huequito
    ```

2. Create the two Conda environments (see below). You only need to do this once.
3. Open `notebooks/01_Basecalling.ipynb` and follow it. It ends
    by submitting a job to the cluster, which can take up to a couple hours.
4. Once that job has finished, open `notebooks/02_Genome_assembly.ipynb`.

## Requirements for Running the Notebooks
### Jupyter Notebook
To successfully execute the notebooks in this repository, you
will need to have Jupyter Notebook installed on your system.
You can run Jupyter Notebooks in two ways:

* Using VSCode (Recommended): you can run Jupyter Notebooks
    within Visual Studio Code, which provides a user-friendly
    interface for working with notebooks. If you use this, make
    sure to install the `Remote - SSH` and `Jupyter` extensions

* Standalone Installation: you can install Jupyter Notebook
    independently and run notebooks from your local environment.

The notebooks are written in R, not Python. Once you open one, click the
kernel selector on the top right and pick the R kernel from the `VScode`
environment.

### Conda
In addition to Jupyter Notebooks, you will need to have the
ability to create and manage Conda environments. Conda is a
package and environment management system that allows you to
install dependencies and manage different project environments
easily.

If you are using this notebook on the M3 HPC you should have
the ability to create Conda environments.

To create the VScode conda environment from the provided YAML file,
run the following command in your terminal:

```bash
conda env create -f envs/VScode.yaml
```

This command will set up a new environment with all the specified packages.
You only need to create the environment once.

To activate the environment after creation, use:

```bash
conda activate VScode
```

YAML files for other Conda environments necessary to execute the pipelines
are provided in the `./envs` folder. The second notebook needs the `Nextflow`
environment, which is created the same way:

```bash
conda env create -f envs/Nextflow.yaml
```

### Cluster and storage
Base calling runs on GPUs, so you need access to a GPU partition. The job
usually takes a few hours, and the assembly pipeline takes a few more, depending
on the number of samples processed and the sequencing depth.

The `pod5` files of a run can take up tens of GB, and the intermediate files
of the assembly pipeline are of a similar size, so make sure you have enough
space before you start.

## Repository structure

```
Huequito
├── config/         # Nextflow configuration used by notebook 02
├── envs/           # Conda environment files
└── notebooks/      # The notebooks, to be run in order
```

The notebooks create a `data` folder for the sequences, sample sheets and
assemblies, and a `bin` folder for `dorado`. Neither is tracked by git.

## Running Huequito outside the Maier Lab

The notebooks assume you are working on M3. If you are not, these are the
things you need to change:

* The path to the raw `pod5` files and to the Kraken2 database, both of which
    point to an internal Maier Lab folder.
* The partition and number of GPUs in the slurm script of notebook 01
* The `-profile m3c` argument of the `bacass` command in notebook 02
* The `time` limits in `config/nextflow.config`, which follow the 24 hour
    limit of M3

## Glossary

| Term | Meaning |
|---|---|
| `pod5` | File with the raw electrical signal recorded by the sequencer |
| Base calling | Translating that signal into DNA sequences |
| `fastq` | File with the DNA sequences and their quality scores |
| Demultiplexing | Separating the samples of a run that carried several barcodes |
| slurm | The program that queues and runs jobs on the cluster |
| Contig | A continuous stretch of sequence assembled from overlapping reads |
| Polishing | Correcting the errors that remain in a draft assembly |

## A note on the use of generative AI

I used Claude Opus 5 to improve the documentation, explanations, and code comments
in this repo (including this README file). The code itself was not AI generated, 
although I implemented a few changes after requesting feedback. I reviewed and 
tested all notebooks manually.

## License

This repository is released under the MIT License. See the `LICENSE` file for the full terms.

## Why `Huequito`?
Huequito means small hole in Spanish. I know, it is not my best joke.