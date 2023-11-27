# Telomere-to-Telomere consortium primates project
T2T-Primates is a project of the [Telomere-to-Telomere consortium](https://sites.google.com/ucsc.edu/t2tworkinggroup/) and is led by the [Makova](https://www.bx.psu.edu/makova_lab/), [Phillippy](https://genomeinformatics.github.io/), and [Eichler](https://eichlerlab.gs.washington.edu/) labs. The project seeks to finish complete, diploid assemblies for key non-human primate species. The project is currently focused on gorilla, bonobo, chimpanzee, orangutan, and gibbon. Following the approach of the human [T2T-CHM13](https://github.com/marbl/CHM13) project, all species have been sequenced with high-coverage PacBio HiFi (~60x) and Oxford Nanopore ultra-long (~40x) sequencing reads. For haplotype phasing, Dovetail Hi-C data was generated for all genomes and Strand-seq data is also expected. Parental Illumina data was collected for bonobo and gorilla, where familial trios were available.

Phase one of the project focused on completing the sex chromosomes (v1 release), and phase two focused on finishing the autosomes (v2 release). Version 2 assemblies for all species are now available and a comparative analysis is underway.

## Version 2 assembly release
Version 2 diploid assemblies were generated by [Verkko](https://github.com/marbl/verkko) with additional finishing and polishing steps to reach T2T. The interior of large rDNA arrays are currently represented by N's and a few telomeres are missing, but all chromosomes are otherwise T2T. Chromosomes were named and oriented according to the prior cytogenetics literature for each species. For convenience, the "hsa" suffix in the chromosome names refers to the human homologous chromosome, where applicable. Gorilla and bonobo were phased using familial trios, and so complete maternal and paternal haplotypes are available for these species. All other species were phased using Hi-C. In the case of Hi-C phasing, each chromosome is completely phased, but it is not known which comes from the maternal or paternal haplotype, so the higher quality haplotype was assigned to hap1 and the lower quality haplotype to hap2. All assemblies have been submitted to NCBI GenBank and are currently being processed. The curated and submitted versions can be downloaded from AWS in a variety of configurations:

- [Gorilla gorilla](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Gorilla_gorilla/mGorGor1/assembly_curated/) (gorilla)
- [Pan paniscus](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Pan_paniscus/mPanPan1/assembly_curated/) (bonobo)
- [Pan troglodytes](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Pan_troglodytes/mPanTro3/assembly_curated/) (chimpanzee)
- [Pongo abelii](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Pongo_abelii/mPonAbe1/assembly_curated/) (Sumatran orangutan)
- [Pongo pygmaeus](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Pongo_pygmaeus/mPonPyg2/assembly_curated/) (Bornean orangutan)
- [Symphalangus syndactylus](https://genomeark.s3.amazonaws.com/index.html?prefix=species/Symphalangus_syndactylus/mSymSyn1/assembly_curated/) (siamang gibbon)

There are a number of files within these directories with the following tags:

`dip` : diploid assembly including both haplotypes  
`chrEBV/MT/rDNA` : consensus EBV, mitochondria, and rDNA contigs  
`analysis-dip` :  diploid assembly + MT + rDNA morph + EBV contigs  
`mat/pat` : maternal and paternal haplotypes, with chrX in mat and chrY in pat  
`hap1/hap2` : hap1 and hap2 haplotypes, which chrX in hap1 and chrY in hap2  
`pri/alt` :  hap1 + ChrY (primary), hap2 - ChrY (alternate)  
`unloc` : any unlocalized sequences from unresolved gaps

Files with the date tag `20231122` are the v2 assemblies that were submitted to GenBank. Both diploid and primary assemblies were submitted, but only the primary assemblies containing both chrX and chrY will be annotated and serve as a linear reference for each species.

## Version 1 assembly release
Version 1 diploid assemblies were generated with [Verkko](https://github.com/marbl/verkko), and contigs were chromosome-assigned and oriented by alignment to the previous references. Both X and Y chromosomes are complete for all species listed. Gorilla and bonobo were phased using familial trios, and all others using Hi-C. To avoid confusion, we have removed links to these assemblies, but they still exist in the AWS bucket.

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
- Acrocentric chromosomes and rDNAs
- Satellite DNAs
- Mobile elements
- Pangenomics

## Contact
For any problems related to this dataset, please raise [issues](https://github.com/marbl/Primates/issues) on this GitHub repository. For general questions regarding the project, please contact <adam.phillippy@nih.gov>. More information about our consortium can be found on the [T2T homepage](https://sites.google.com/ucsc.edu/t2tworkinggroup/).

## History

    * Dec 2022. v1 release.
    * Nov 2023. v2 release.
