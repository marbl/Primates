# Chain files for lifting Chrs XY data from v1.1 to v2.0

The XY chromosomes released along with [Makova et al.](https://www.nature.com/articles/s41586-024-07473-2) were mapped to the newly assembled, curated v2.0 assemblies of the primates.

Since the XY chromosomes were gapless, and were assembled from the same individual, a simple global alignment approach was emploid for each X and Y chromosome. The XY chromosomes were extracted with [samtools v1.19](https://github.com/samtools/samtools), then mapped to each corresponding chromosome individually using [wfmash v0.14.0](https://github.com/waveygang/wfmash). The output paf files were converted to chain files with [paf2chain v0.1.1](https://github.com/AndreaGuarracino/paf2chain) and UCSC [chainSwap v466](https://hgdownload.soe.ucsc.edu/admin/exe/).

Note mSynSyn1 v2.1 XY chromosomes are identical to v2.0 (the only change from v2.0 to v2.1 was in chr12 and chr19).

```shell
for sp in mGorGor1 mPanPan1 mPanTro3 mPonAbe1 mPonPyg2 mSymSyn1
do
  for chr in X Y
  do
    # Extract chromosome name in v2.0
	chr_name=`grep chr$chr ${sp}_v2.0/${sp}_v2.0.fa.fai | cut -f1`

	# Extract chromosome
	samtools faidx ${sp}_v2.0.fa $chr_name > ${sp}_v2.0.$chr.fa
	bgzip -i ${sp}_v2.0.$chr.fa -@12

	wfmash -d -N --one-to-one -s 50000 --map-pct-id=95 \
	  -t12 ${sp}_v2.0.$chr.fa.gz ${sp}_v1.1.$chr.fa > ${sp}_${chr}_v1.1_to_v2.0.paf

	# We need to swap the chain, as liftOver lifts from target to query
	paf2chain -i ${sp}_${chr}_v1.1_to_v2.0.paf > ${sp}_${chr}_v2.0_to_v1.1.chain
	chainSwap ${sp}_${chr}_v2.0_to_v1.1.chain ${sp}_${chr}_v1.1_to_v2.0.chain
	set +x
  done

  cat ${sp}_X_v1.1_to_v2.0.chain ${sp}_Y_v1.1_to_v2.0.chain > ${sp}_XY_v1.1_to_v2.0.chain
  cat ${sp}_X_v2.0_to_v1.1.chain ${sp}_Y_v2.0_to_v1.1.chain > ${sp}_XY_v2.0_to_v1.1.chain
done
```
All alignments were ensured to have no breaks.

## An example use case
A simple way to lift over an input bed file from v1.1 is to use UCSC [liftOver](https://genome.ucsc.edu/goldenPath/help/hgTracksHelp.html#Liftover). Below is an example using the first and last 5 bps as anchors to lift over a region.
```shell
liftOver -ends=5 v1.1.bed ${sp}_XY_v1.1_to_v2.0.chain v1.1.lifted.bed v1.1.unlifted.bed
```

## PAF files (simple version with no MD tag)
```
# mGorGor1
chrX    177553137       0       174217600       +       chrX_mat_hsaX   177558554       2935    174224672       174185717       174252437       34
chrY    65269191        2560    64915200        +       chrY_pat_hsaY   67405748        3       64779171        63452037        66233392        14

# mPanPan1
chrX    160249396       0       160173696       +       chrX_mat_hsaX   160241051       0       160164070       160160014       160177071       40
chrY    46925665        22369   46925665        -       chrY_pat_hsaY   46954669        11      46932296        46901735        46933618        32

# mPanTro3
chrX    153892427       0       153774592       +       chrX_hap1_hsaX  153896354       0       153778533       153692053       153851601       30
chrY    36445859        5283    36445859        -       chrY_hap2_hsaY  36447294        0       36442010        36434038        36446003        35

# mPonAbe1
chrX    162586350       0       162570368       +       chrX_hap1_hsaX  162586321       0       162570341       162570258       162570410       60
chrY    67808326        0       67790080        +       chrY_hap2_hsaY  67827326        0       67797136        67603130        67978971        23

# mPonPyg2
chrX    161100531       0       161089408       +       chrX_hap1_hsaX  160969493       0       160958368       160948007       161098357       30
chrY    49349692        0       49189376        +       chrY_hap2_hsaY  49419398        0       49201396        49092685        49294380        24

# mSynSyn1
chrX    165588543       0       165576960       +       chrX_hap1       165573899       0       165562949       165402591       165729913       27
chrY    29919963        0       29580800        +       chrY_hap2       29938336        1       29586980        29577176        29590276        34
```