# T2T Primates project
T2T-Primates is a project of the [Telomere-to-Telomere consortium](https://sites.google.com/ucsc.edu/t2tworkinggroup/) and is led by the [Makova](https://www.bx.psu.edu/makova_lab/), [Phillippy](https://genomeinformatics.github.io/), and [Eichler](https://eichlerlab.gs.washington.edu/) labs. The project seeks to finish complete, diploid assemblies for key non-human primate species. The project is currently focused on gorilla, bonobo, chimpanzee, orangutan, and gibbon. Following the approach of the human [T2T-CHM13](https://github.com/marbl/CHM13) project, all species have been sequenced with high-coverage PacBio HiFi (~60x) and Oxford Nanopore ultra-long (~40x) sequencing reads. For haplotype phasing, Dovetail Hi-C data was generated for all genomes and Strand-seq data is also expected. Parental Illumina data was collected for bonobo and gorilla, where familial trios were available.

Phase one of the project is focused on completing the sex chromosomes; phase two will focus on finishing the autosomes of bonobo and gorilla; and phase three will focus on the remaining genomes. The project is currently in phase one, with draft T2T sex chromosome assemblies now available for all genomes.

## Latest assembly releases
Version 1 diploid assemblies were generated with [Verkko v1.1](https://github.com/marbl/verkko), and contigs were chromosome-assigned and oriented by alignment to the previous references. Both X and Y chromosomes are complete for all species listed. Gorilla and bonobo were phased using familial trios, and all others using Hi-C:
- [Gorilla gorilla](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Gorilla_gorilla/mGorGor1/assembly_verkko_1.1-0.2-freeze/) (gorilla)
- [Pan paniscus](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Pan_paniscus/mPanPan1/assembly_verkko_1.1-0.1-freeze/) (bonobo)
- [Pan troglodytes](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Pan_troglodytes/mPanTro3/assembly_verkko_1.1-hic-freeze/) (chimpanzee)
- [Pongo abelii](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Pongo_abelii/mPonAbe1/assembly_verkko_1.1-hic-freeze/) (Sumatran orangutan)
- [Pongo pygmaeus](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Pongo_pygmaeus/mPonPyg2/assembly_verkko_1.1-hic-freeze/) (Bornean orangutan)
- [Symphalangus syndactylus](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Symphalangus_syndactylus/mSymSyn1/assembly_verkko_1.1-hic-freeze/) (siamang gibbon)

## Downloads
All generated sequencing data and assemblies are available for browsing and download from [GenomeArk](https://genomeark.github.io/t2t-all/).

### Notes on downloading files
Files are generously hosted by Amazon Web Services under `s3://genomeark`. Although available as HTTP links above, download performance is improved by using the Amazon Web Services [command-line interface](https://aws.amazon.com/cli/). References should be amended to use the `s3://` addressing scheme. Amending the `max_concurrent_requests` etc. settings as per [this guide](https://docs.aws.amazon.com/cli/latest/topic/s3-config.html) will improve download performance further.

## Data reuse and license
All data is released to the public domain ([CC0](https://creativecommons.org/publicdomain/zero/1.0/)) and we encourage its reuse. However, we are in the process of finishing and analyzing these genomes, so to avoid duplicating effort, we encourage you to contact us if you are interested in contributing. The following working groups have been formed:
- Assembly
- Annotation
- Sex chromosome genomics
- Comparative and evolutionary genomics
- Segmental duplications
- Acrocentric chromosomes
- Satellite DNAs
- Mobile elements
- Pangenomics

## Contact
For any problems related to this dataset, please raise issues on this GitHub repository. For general questions regarding the project, please contact <adam.phillippy@nih.gov>. More information about our consortium can be found on the [T2T homepage](https://sites.google.com/ucsc.edu/t2tworkinggroup/).

## History

    * Dec 2022. Initial release.
