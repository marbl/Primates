# Issues

Issues were collected from HiFi and ONT alignments to the primate diploid analysis-set using the T2T-Polish coverage pipeline.

## Issues by HiFi and ONT-UL read mappings

`run_issues.sh`
```shell
#!/bin/sh

module load aws
module load ucsc

species=species.txt
len=`wc -l species.txt | awk '{print $1}'`
ref="dip"

set -o pipefail
set -x

for i in $(seq 1 6)
do
  path_l=`sed -n ${i}p $species`
  sp=`echo $path_l | awk '{print $1}'`
  sp_path=`echo $path_l | awk '{print $2}'`
  ver=${sp}_v2.0
  sizes=../../pattern/$ver.fa.fai

  for pf in HiFi ONT
  do
    path_old=winnowmap.analysis-$ref.$pf
    path_old=winnowmap.$ref.$pf
    path_new=$ver.$ref.$pf
    mkdir -p $path_new && cd $path_new
    ln -sf ../../mapping/$ver.$ref.$pf/$path_old.pri.paf $path_new.pri.paf
    ln -sf ../../mapping/$ver.$ref.$pf/$path_old.pri.cov.wig $path_new.pri.cov.wig
    ln -sf ../../mapping/$ver.$ref.$pf/$path_old.pri.clip_abs.wig $path_new.pri.clip_abs.wig
    ln -sf ../../mapping/$ver.$ref.$pf/$path_old.pri.clip_norm.wig $path_new.pri.clip_norm.wig

    $tools/T2T-Polish/coverage/issues.sh $path_new.pri.paf ${pf}_to_${ref} $ver $pf ../../pattern
    awk -v OFS='\t' -v FS='\t' 'NR==FNR { mapping[$1] = $2; next } {if ($3 > mapping[$1]) { $3=mapping[$1]; $8=mapping[$1];} print}' $sizes $path_new.pri.issues.bed > $path_new.pri.issues.fm.bed
    wigToBigWig -clip $path_new.pri.cov.wig $sizes $path_new.pri.cov.bw
    bedToBigBed -type=bed9 $path_new.pri.issues.fm.bed $sizes $path_new.pri.issues.bb

    aws s3 cp --profile=vgp --no-progress --recursive --exclude "*" --include "$path_new.*wig"        ./ s3://genomeark/species/$sp_path/$sp/assembly_curated/issues/
    aws s3 cp --profile=vgp --no-progress --recursive --exclude "*" --include "$path_new.*issues.bed" ./ s3://genomeark/species/$sp_path/$sp/assembly_curated/issues/
    aws s3 cp --profile=vgp --no-progress --recursive --exclude "*" --include "$path_new.*.bw"        ./ s3://genomeark/species/$sp_path/$sp/assembly_curated/issues/
    aws s3 cp --profile=vgp --no-progress --recursive --exclude "*" --include "$path_new.*.bb"        ./ s3://genomeark/species/$sp_path/$sp/assembly_curated/issues/
    cd ../
  done
done
```

## Finalizing issues.bed
Merging issues bed tracks from HiFi and ONT platforms

`merge_issues.sh`

```shell
#!/bin/sh

module load bedtools

species=species.txt
ref="dip"

set -o pipefail
set -x

for i in $(seq 1 6)
do
  path_l=`sed -n ${i}p $species`
  sp=`echo $path_l | awk '{print $1}'`
  sn=`echo $path_l | awk '{print $2}'`
  ver=${sp}_v2.0
  sizes=../../pattern/$ver.fa.fai

  hifi_issues=$ver.$ref.HiFi/$ver.$ref.HiFi.pri.issues.bed
  ont_issues=$ver.$ref.ONT/$ver.$ref.ONT.pri.issues.bed
  
  bedtools intersect -u -a $hifi_issues -b $ont_issues | grep -v "MT" | grep -v "random" | grep -v "rDNA" > ${ver}.issues.bed
done
```

Count bps by categories
* rDNA
* Gap
* Assembly Issues (exclude end of scaffolds)

### CenSat track
rDNA and GAPs are present in the CenSat track. Let's pull them out.
```shell
for sp in mGorGor1 mPanPan1 mPanTro3 mPonAbe1 mPonPyg2 mSymSyn1
do
  ver=${sp}_v2.0
  #ln -s /data/Phillippy2/projects/T2T-Browser/incoming/CenSatData/T2TPrimates/${ver}_CenSat_v2.0.bed
  awk '$4=="rDNA"' ${ver}_CenSat_v2.0.bed > ${ver}.rDNA.bed
  awk '$4=="GAP"' ${ver}_CenSat_v2.0.bed > ${ver}.GAP.bed
done
```

### Scaffold ends
```shell
for sp in mGorGor1 mPanPan1 mPanTro3 mPonAbe1 mPonPyg2 mSymSyn1
do
  ver=${sp}_v2.0
  sizes=../pattern/$ver.fa.fai
  grep -v "MT" $sizes | grep -v "rDNA" | grep -v "random" | awk '{print $1"\t0\t100\n"$1"\t"($2-100)"\t"$2}' > ${ver}.end.bed
  grep "random" $sizes | awk '{print $1"\t0\t100\n"$1"\t"($2-100)"\t"$2}' > ${ver}.random.bed
done
```

### Exclude rDNA and gap from assembly issues

```shell
for sp in mGorGor1 mPanPan1 mPanTro3 mPonAbe1 mPonPyg2 mSymSyn1
do
  ver=${sp}_v2.0
  # Merge issues within 100kb
  bedtools merge -d 100000 -i ${ver}.issues.bed > ${ver}.issues.100k_mrg.bed

  # Collect rDNA or END overlapping issue chunks
  cat ${ver}.end.bed ${ver}.rDNA.bed | cut -f1-3 | sort -k1,1 -k2,2n | bedtools intersect -u -a ${ver}.issues.100k_mrg.bed -b - > ${ver}.issues.100k_mrg.rDNA.bed

  # Remove any overlaps from $ver.issues.bed
  bedtools subtract -A -a ${ver}.issues.bed -b ${ver}.issues.100k_mrg.rDNA.bed > ${ver}.issues.no_rDNA.no_end.bed

  # Report
  bed=${ver}.GAP.bed
  gap=`awk '{sum+=$3-$2} END {print sum}' $bed`

  bed=${ver}.issues.100k_mrg.rDNA.bed
  rDNA=`awk '{sum+=$3-$2} END {print sum}' $bed`

  bed=${ver}.random.bed
  random=`awk '{sum+=$3-$2} END {print sum}' $bed`

  bed=${ver}.issues.no_rDNA.no_end.bed
  assembly=`awk '{sum+=$3-$2} END {print sum}' $bed`

  echo $ver
  echo -e "GAP\t$gap"
  echo -e "Collapsed rDNA\t$rDNA"
  echo -e "Unlocalized contigs\t$random"
  echo -e "Other assembly issues\t$assembly"
  echo
done
```

### Collect num. of rDNA gaps
```shell
for sp in mGorGor1 mPanPan1 mPanTro3 mPonAbe1 mPonPyg2 mSymSyn1
do
  ver=${sp}_v2.0
  echo $ver
  cat $ver.GAP.bed | awk '$3-$2==1000000' | wc -l
done
```

### Upload final issues track
```shell
module load ucsc
module load aws

species=../species.txt
for i in $(seq 1 6)
do
  path_l=`sed -n ${i}p $species`
  sp=`echo $path_l | awk '{print $1}'`
  sn=`echo $path_l | awk '{print $2}'`
  ver=${sp}_v2.0
  sizes=../pattern/$ver.fa.fai
  bedToBigBed -type=bed9 -tab ${ver}.issues.no_rDNA.no_end.bed $sizes ${ver}.issues.no_rDNA.no_end.bb
  aws s3 cp --profile=vgp --no-progress ${ver}.issues.no_rDNA.no_end.bb s3://genomeark/species/$sn/$sp/assembly_curated/issues/
done
```
