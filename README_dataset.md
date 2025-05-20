# Primate dataset

Cleaned up on May 20 2025.

## Species ID

| species_id | species_name             | Yoo_et_al2025 | Maternal | Paternal |
| ---------- | ------------------------ | ------------- | -------- | -------- |
| mGorGor1   | Gorilla_gorilla          | Jim_GGO       | mGorGor2 | mGorGor3 |
| mPanPan1   | Pan_paniscus             | PR00251_PPA   | mPanPan2 | mPanPan3 |
| mPanTro3   | Pan_troglodytes          | AG18354_PTR   |
| mPonAbe1   | Pongo_abelii             | AG06213_PAB   |
| mPonPyg2   | Pongo_pygmaeus           | AG05252_PPY   |
| mSymSyn1   | Symphalangus_syndactylus | Jambi_SSY     |


## Download genomic data

```sh
module load aws
aws s3 cp --no-sign-request --recursive s3://genomeark/species/$species_name/$species_id/genomic_data/ .
```
## Illumina
* Only mGorGor1 and mPanPan1 are available in trios.

## HiFi
* `hifi_reads`
  * `hifi_reads.bam` - re-called and packed bam with kinetics information
  * `hifi_reads.fq.gz` - extracted from the `hifi_reads.bam` with `MM` and `ML` tags. Was re-called later using the latest available primrose model for methylation analysis.
* `subreads` : Original bam with all subreads. This was the input for generating `hifi` reads or `DeepConsensus` reads.
* `DeepConsensus_v0.3rc0` : Files called with DeepConsensus v0.3rc0. This dataset predates the re-basecalled `hifi_reads` and was used in assembly and polishing. Generated in courtesy of Andrew Carroll at Google Health.

## ONT
* Latest basecalling was done with Guppy v6.3.8 if raw data was available, including MM and ML tags.
* Re basecall by downloading the fast5 and begin from there if needed.
* Some ONT data did not have original fast5 data to re-basecall with the methylation tags.

## HiC
Aug/30/2022 (Bob Harris)
This is HiC sequencing data for six primates.

Barb McGrath grew the primate cell lines (see below) using standard cell
culture methods for adherent (b'orang, s'orang, bonobo & gorilla) and
non-adherent (chimp2 and Jambi) cell types.

| Species                                  | Our sample IDs  | BioSample ID | Cell Type      |  Source                                                               |
| ---------------------------------------- | --------------- | ------------ | -------------- | --------------------------------------------------------------------- |
| Pongo pygmaeus, Bornean Orangutan        | AG05252         | SAMN10521809 | fibroblast     |  Coriell Institute, originally the San Diego Zoo                      |
| Pongo abelii, Sumatran Orangutan         | AG06213/GM06213 | SAMN10521808 | fibroblast     |  Laura Carrel PSU Hershey Med. Ctr., orignally from Coriell Institute |
| Pan paniscus, Pygmy Chimpanzee/Bonobo    | KB8711/PR00251  | SAMN13935689 | fibroblast     |  San Diego Zoo                                                        |
| Gorilla gorilla, Gorilla                 | KB3781          | SAMN04003007 | fibroblast     |  San Diego Zoo                                                        |
| Pan Troglodytes, Chimpanzee2             | AG18354         | This study   | lymphoblastoid |  Laura Carrel PSU Hershey Med. Ctr., orignally from Coriell Institute |
| Symphalangus syndactylus, Gibbon, Jambi  | Jambi           | This study   | lymphoblastoid |  Lucia Carbone, Oregon Healt & Science University                     |

5 million cells were centrifuged to generate pellets which were washed once
with phosphate buffered saline and snap-frozen in liquid nitrogen.

Frozen pellets were stored at -80 degrees prior to shipment to UC Santa Cruz on
dry ice.

The HiC libraries were prepared from 6 primate cell lines by Sam Sacco in Ed
Green's lab at UC Santa Cruz. They used the Dovetail Hi-C approach (with the
Dovetail Omni-C Kit (Catalog # 21005)) for HiC library generation. Fragment
sizes of 150-1400 to 1600 bp, as determined by Fragment Analyzer analyses were
included in the libraries. Qubit was used for quantification of each sample.

The pooled samples were shipped directly from Santa Cruz to Sirisha Pochareddy
at Hershey at the Hershey Genome Science Facility for sequencing on the
Illumina NovaSeq 6000 instrument. Makova Lab requested 400 million read pairs
at 2x150bp/sample from the Genome Sciences Core at Hershey. For demultiplexing,
bcl2fastq version 2.20.0 was used. A single NovaSeq S2 flow cell was used.

The targeted library size was 300-1,000, but at the end it included fragments
150-1400(1600) bp.

The delivered data includes two subdirectories with information about the run -
Stats and Reports. The Reports directory has an html file - a table describing
yield.

Note: those reports are described in the "bcl2fastq2 Conversion Software v2.20
Software Guide", in its "HTML Reports" section. As of this writing, that
document can be found [here](https://support.illumina.com/content/dam/illumina-support/documents/documentation/software_documentation/bcl2fastq/bcl2fastq2-v2-20-software-guide-15051736-03.pdf).

The yield shown is 1.0TBase. The space on disk for all the files is 0.5TBytes,
nearly all of that is gzipped fastq files. So if fastq compressed to 50% that
would jibe.

"Clusters (Raw)" are apparently reads (or read pairs). The file
G929_Bon_a_1_S1_R2_001.fastq.gz contains 164,278,963 reads and the flowcell
summary for Bon_a_1 says there are 164,278,963 clusters.

Average read length over the whole must then be 1,054,068,000,000 bases /
5,761,400,832 reads ≈ 183 bases per read.

The sample IDs used for sequencing are as follows:

| Customer Sample ID  | Core sample ID | raw clusters |  PF clusters | %bases>PHRED 30 |
| ------------------- | -------------- | ------------ | ------------ | --------------- |
| Bon_a_1             | G929-1         | 164,278,963  |  164,278,963 | -               |
| (lane 1)            | -              | -            |   80,627,132 | 86.38           |
| (lane 2)            | -              | -            |   83,651,831 | 86.57           |
| Gor_b_1             | G929-2         | 263,798,269  |  263,798,269 | -               |
| (lane 1)            | -              | -            |  129,309,344 | 84.92           |
| (lane 2)            | -              | -            |  134,488,925 | 85.12           |
| B.Ora_b_1           | G929-3         | 222,757,364  |  222,757,364 | -               |
| (lane 1)            | -              | -            |  109,629,738 | 85.97           |
| (lane 2)            | -              | -            |  113,127,626 | 86.17           |
| S.Ora_a_1           | G929-4         | 143,758,733  |  143,758,733 | -               |
| (lane 1)            | -              | -            |   70,784,561 | 86.23           |
| (lane 2)            | -              | -            |   72,974,172 | 86.43           |
| Chimp_a_1           | G929-5         | 276,820,405  |  276,820,405 | -               |
| (lane 1)            | -              | -            |  136,873,825 | 86.98           |
| (lane 2)            | -              | -            |  139,946,580 | 87.18           |
| Jam_a_1             | G929-6         | 439,486,510  |  439,486,510 | -               |
| (lane 1)            | -              | -            |  217,759,453 | 86.49           |
| (lane 2)            | -              | -            |  221,727,057 | 86.70           |
| Bon_a_2             | G929-7         | 189,705,391  |  189,705,391 | -               |
| (lane 1)            | -              | -            |   93,285,191 | 86.89           |
| (lane 2)            | -              | -            |   96,420,200 | 87.08           |
| Gor_b_2             | G929-8         | 226,155,855  |  226,155,855 | -               |
| (lane 1)            | -              | -            |  110,694,054 | 85.12           |
| (lane 2)            | -              | -            |  115,461,801 | 85.33           |
| B.Ora_b_2           | G929-9         | 339,416,654  |  339,416,654 | -               |
| (lane 1)            | -              | -            |  166,619,161 | 84.85           |
| (lane 2)            | -              | -            |  172,797,493 | 85.04           |
| S.Ora_a_2           | G929-10        | 226,993,291  |  226,993,291 | -               |
| (lane 1)            | -              | -            |  111,823,513 | 86.41           |
| (lane 2)            | -              | -            |  115,169,778 | 86.59           |
| Chimp_a_2           | G929-11        | 352,725,267  |  352,725,267 | -               |
| (lane 1)            | -              | -            |  174,589,051 | 86.68           |
| (lane 2)            | -              | -            |  178,136,216 | 86.90           |
| Jam_a_2             | G929-12        | 415,361,845  |  415,361,845 | -               |
| (lane 1)            | -              | -            |  206,750,429 | 86.71           |
| (lane 2)            | -              | -            |  208,611,416 | 86.92           |

## mGorGor1 specific

All the rest of the ape genomes have methylation tags in its ONT fq.gz files on AWS.

### ONT
* no-methyl:
  * 8a45ea80774cf84a765801d37088c45b  PAM69683_guppy-6.3.7.fq.gz
  * 09214640082adeaf7da30b0944dec863  PAG68695_guppy-6.3.7.fq.gz
  * dd5a027915fa5db4ef82f7aff1fdba4f  PAG68076_guppy-6.3.7.fq.gz
  * da5189a7f8de900f0008c3d55b0d7258  PAG67391_guppy-6.3.7.fq.gz
  * ee4070019e0a98a3c2dd87317a7556e0  PAG68568_guppy-6.3.7.fq.gz
* with-methyl:
  * 125caa3e366f25c26bd53b72ecfd6c25  PAG26046_guppy-6.3.8.fq.gz
  * 5dbbae2695dba1617dc8835d311b358e  PAG26815_guppy-6.3.8.fq.gz
  * 10db7449721b71daf0bf5d693ad36fc9  PAI19139_guppy-6.3.8.fq.gz
  * c4e0d2234dfa8c2722bad52150526750  PAI19180_guppy-6.3.8.fq.gz
  * 2f31a27ba33d88519e50824a69fbbca0  PAI19210_guppy-6.3.8.fq.gz

