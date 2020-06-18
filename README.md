# BU Parallelized Fastq Dump

Given a NCBI Bioproject ID
1. Gather SRA IDs
2. Prefetch all SRAs
3. Chunks SRAs into groups and submits to the multithreaded [fasterq-dump](https://github.com/ncbi/sra-tools/wiki/HowTo:-fasterq-dump)

Highly recommend [executing this command](https://www.biostars.org/p/159950/#160125) which will have NCBI download files to /tmp instead of your home directory

`echo '/repository/user/main/public/root = "/tmp"' >> $HOME/.ncbi/user-settings.mkfg`

# Set up
Add an environment variable to your path pointing at the code. Only do this once!

```bash
export dumpproject=/projectnb2/talbot-lab-data/msilver/BUfqdump/dumpproject.qsub
source ~/.bashrc
```

# Tutorial

`qsub $dumpproject PRJNA485178 $PWD/PRJNA485178 --split-files`

