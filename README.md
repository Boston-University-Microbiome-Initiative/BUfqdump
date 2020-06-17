# BU Parallelized Fastq Dump

Given a NCBI Bioproject ID
1. Gather SRA IDs
2. Prefetch all SRAs
3. Chunks SRAs into groups and submits to parallel-fastq-dump, which chunks *each file* and fastq-dumps each of those in parallel

Highly recommend [executing this command](https://www.biostars.org/p/159950/#160125) which will have NCBI download files to /tmp instead of your home directory

`echo '/repository/user/main/public/root = "/tmp"' >> $HOME/.ncbi/user-settings.mkfg`

# Tutorial

`qsub dumpproject.qsub PRJNA485178 --split-files --gzip -O $PWD/PRJNA485178`

