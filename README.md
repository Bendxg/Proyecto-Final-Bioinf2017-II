

Contains the scripts to build a de Novo assembly of the *Pieris rapae* transcriptome, the files containing the RNA sequences of *P. rapae* used to assamble the transcriptome in Meslin et al (2015), and the output files.

## `/bin/`:

The scripts in `/bin` should be read in the following order
  1. `/Trim` Contains the scripts to filter the raw RNA squences of Pieris rapae using FASTX Toolkit. For each file, quality trimming of sequences with quality score < 30 is aplied. The script uses all files in the directory `/SecuenciasPierisrapae`.
  2. `/Assambly` Contains the script to assambly the sequence using default parameters, using paired end reads with all files in the directory `/FilteredSec`.


## `/data/`:

Contains data for transcriptome de novo assembly as follows:

`/SecuenciasPierisrapae` Contains fasta.gz files  data of the RNA raw sequences with barcode sequences previously removed

`/FilteredSec` Data corresponds to FASTX Toolkit output filtered sequences, generated with 1. `/Trim`

`/Transcriptoma` Data corresponds to Trinity output filtered sequences, generated with  2. `/Assambly`.
