Annotation
================

# ERCC

``` bash
## dummy ercc transcript gene conversion
awk '{print $1 " " $1}' annotation/ercc/ercc.gtf > annotation/ercc/transcript_gene_conv_ercc.txt
```

# MOUSE

Download genome and annotation gtf for each annotation used:

  - [vega](http://vega.archive.ensembl.org/Mus_musculus/Info/Index)
  - [gencode](https://www.gencodegenes.org/mouse/release_M15.html)
  - [refseq](ftp://ftp.ncbi.nih.gov/refseq/M_musculus/)

Furthermore, convert it to transcript fasta files for transcriptome
alignment and pseudoalignment.

## vega

``` bash
# reduce to exons
more Mus_musculus.GRCm38.68_vega.gtf | awk '$3 =="exon"' > Mus_musculus.GRCm38.68_vega_exon.gtf
# get sequences matching annotation in gtf file
gffread Mus_musculus.GRCm38.68_vega_exon.gtf -g Mus_musculus.VEGA68.dna.toplevel.fa -w Mus_musculus.GRCm38.68_vega_exon.fa
# combine transcript fasta with ERCC fasta
cat Mus_musculus.GRCm38.68_vega_exon.fa annotation/ercc/ercc.fa > mm10_transcriptome_spike_vega.fa
# transcript / gene id conversion table
awk '{print $10 " " $12}' Mus_musculus.GRCm38.68_vega_exon.gtf  | sed 's/[";]//g' | sort | uniq > transcript_gene_conv_vega.txt
# combine transcript conversion with ERCC dummy
cat transcript_gene_conv_vega.txt annotation/ercc/transcript_gene_conv_ercc.txt > transcript_gene_conv_spike_vega.txt
# combine genomes fasta with spike-ins fasta
cat Mus_musculus.VEGA68.dna.toplevel.fa annotation/ercc/ercc.fa > mm10_genome_spike_vega.fa
# combine genomes gtf with spike-ins gtf
cat Mus_musculus.GRCm38.68_vega.gtf annotation/ercc/ercc.gtf > mm10_genome_spike_vega.gtf
```

## gencode

``` bash
# reduce to exons
more gencode.vM15.primary_assembly.annotation.gtf | awk '$3 =="exon"' > gencode.vM15.primary_assembly.annotation_exon.gtf
# get sequences matching annotation in gtf file
gffread gencode.vM15.primary_assembly.annotation_exon.gtf -g GRCm38.primary_assembly.genome.fa -w gencode.vM15.primary_assembly_exon.fa
# combine transcript fasta with ERCC fasta
cat gencode.vM15.primary_assembly_exon.fa ../ercc/ercc.fa > mm10_transcriptome_spike_gencode.fa
# transcript / gene id conversion table
awk '{print $10 " " $12}' gencode.vM15.primary_assembly.annotation_exon.gtf | sed 's/[";]//g' | sort | uniq > transcript_gene_conv_gencode.txt
# combine transcript conversion with ERCC dummy
cat transcript_gene_conv_gencode.txt annotation/ercc/transcript_gene_conv_ercc.txt > transcript_gene_conv_spike_gencode.txt
# combine genomes fasta with spike-ins fasta
cat GRCm38.primary_assembly.genome.fa  annotation/ercc/ercc.fa > mm10_genome_spike_gencode.fa
# combine genomes gtf with spike-ins gtf
cat gencode.vM15.primary_assembly.annotation.gtf ../ercc/ercc.gtf > mm10_genome_spike_gencode.gtf
```

## refseq

``` bash
# reduce to exons
more Refseqcurated_mm10.gtf | awk '$3 =="exon"' > Refseqcurated_mm10_exon.gtf
# get sequences matching annotation in gtf file
gffread Refseqcurated_mm10_exon.gtf -g mm10.fa -w Refseqcurated_mm10_exon.fa
# combine transcript fasta with ERCC fasta
cat Refseqcurated_mm10_exon.fa ../ercc/ercc.fa > mm10_transcriptome_spike_refseq.fa
# transcript / gene id conversion table
awk '{print $10 " " $12}' Refseqcurated_mm10_exon.gtf | sed 's/[";]//g' | sort | uniq > transcript_gene_conv_refseq.txt
# combine transcript conversion with ERCC dummy
cat transcript_gene_conv_refseq.txt ../ercc/transcript_gene_conv_ercc.txt > transcript_gene_conv_spike_refseq.txt
# combine genomes fasta with spike-ins fasta
cat mm10.fa ../ercc/ercc.fa > mm10_genome_spike_refseq.fa
# combine genomes gtf with spike-ins gtf
cat Refseqcurated_mm10.gtf ../ercc/ercc.gtf > mm10_genome_spike_refseq.gtf
```
