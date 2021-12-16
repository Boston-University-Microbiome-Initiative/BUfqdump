# BU Parallelized Fastq Dump

Given a NCBI Bioproject ID
1. Gather SRA IDs
2. Prefetch all SRAs
3. Chunks SRAs into groups and submits to the multithreaded [fasterq-dump](https://github.com/ncbi/sra-tools/wiki/HowTo:-fasterq-dump)

Highly recommend [executing this command](https://www.biostars.org/p/159950/#160125) which will have NCBI download temporary files to `/scratch` instead of your home directory

```bash
echo '/repository/user/main/public/root = "/scratch"' >> $HOME/.ncbi/user-settings.mkfg
```

# Set up
Add an environment variable to your path pointing at the code. Only do this once!

```bash
echo "export dumpproject=/projectnb2/talbot-lab-data/msilver/BUfqdump/dumpproject.qsub" >> ~/.bashrc
source ~/.bashrc
```

# Usage
## Tutorial
`qsub $dumpproject PRJNA485178 $PWD/PRJNA485178 --split-files`

For submitting multiple jobs, it can be convenient to name the qsub job with the bioproject id
```bash
bioproject=PRJNA485178
qsub -N $bioproject $dumpproject $bioproject $PWD/$bioproject --split-files
```
## Documentation
There are two files in this repo:
1. [`dumpproject.qsub`](dumpproject.qsub): 
    1. Retrieves the SRA IDs from a given Bioproject
    2. Breaks the list of those IDs into chunks of 30
    3. Submits each chunk to `fqdump.qsub`
2. [`fqdump.qsub`](fqdump.qsub): Downloads and gzips fastq files for given SRA IDs.

Inputs for each be viewed in the command line with `-h`:
```bash
[msilver4@scc1 ~]$ bash $dumpproject -h
USAGE: qsub dumpproject.qsub <NCBI BIOPROJECT ID> <OUTPUT DIRECTORY> <FASTERQ-DUMP ARGUMENTS>
 - Do not use the -O option for output directory, supply output directory as second argument
 - Do not use --gzip, files will be zipped automatically
```

**NOTICE: Output directory is the second argument, do not provide the -O option. Nor should you provide the --gzip command, refer to the fasterq-dump documentation for allowed arguments.**

```bash
[msilver4@scc1 ~]$ bash $(dirname $dumpproject)/fqdump.qsub -h
# USAGE: qsub fqdump.qsub <PATH TO LIST OF SRAS> <OUTPUT DIRECTORY> <fasterq-dump arguments>
 # Do not use the -O option for output directory, supply output directory as second argument
```


