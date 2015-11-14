## Table of contents
1. Introduction
  1. Description of the lab
  2. Requirements
  3. Compatibility
  4. Data Set for IGV
  5. Check the online wiki
2. Visualization Part 1: Getting familiar with IGV
  1. Get familiar with the interface
    1. Load a Genome and Data Tracks
    2. Navigation
  2. Region Lists
  3. Loading Read Alignments
  4. Visualizing read alignments
3. Visualization Part 2: Inspecting SNPs, SNVs, and SVs
  1. Neighbouring somatic SNV and germline SNP
  2. Homopolymer repeat with indel
  3. Coverage by GC
  4. Heterozygous SNPs on different alleles
  5. Low mapping quality
  6. Homozygous deletion
  7. Mis-alignment
  8. Translocation
4. Visualization Part 3: Automating Tasks in IGV


## Introduction 

### Description of the lab

Welcome to the lab for **Genome Visualization**! This lab will introduce you to the [Integrative Genomics Viewer](http://www.broadinstitute.org/igv/home), one of the most popular visualization tools for High Throughput Sequencing (HTS) data.

After this lab, you will be able to:
* Visualize a variety of genomic data
* Quickly navigate around the genome
* Visualize read alignments
* Validate SNP/SNV calls and structural re-arrangements by eye

Things to know before you start:
* The lab may take between **1-2 hours**, depending on your familiarity with genome browsing. Do not worry if you do not complete the lab. It will remain available to review later.
* There are a few thought-provoking **Questions** or **Notes** pertaining to sections of the lab. These are **optional**, and may take more time, but are meant to help you better understand the visualizations you are seeing. These questions will be denoted by boxes, as follows:
 **Question(s):**

```
Thought-provoking question goes here
```

### Requirements

* [Integrative Genomics Viewer](http://www.broadinstitute.org/igv/home)
* Ability to run Java
* Note that while most tutorials in this course are performed on the cloud, IGV will always be run on your **local** machine

### Compatibility

This tutorial was intended for **IGV v2.3**, which is available on the [http://www.broadinstitute.org/software/igv/download Download] page. It is *strongly* recommended that you use this version.

=== Data Set for IGV ===

* Chromosome 21: 19,000,000-20,000,000
* [[Media:HCC1143.normal.21.19M-20M.bam|HCC1143.normal.21.19M-20M.bam]]
* [[Media:HCC1143.normal.21.19M-20M.bam.bai|HCC1143.normal.21.19M-20M.bam.bai]]

=== Check the online wiki ===

Your instructors may update the lab with clarifications or more bonus sections.

= Visualization Part 1: Getting familiar with IGV=

We will be visualizing read alignments using the
[http://www.broadinstitute.org/igv/home IGV], 
a popular visualization tool for HTS data.

First, lets familiarize ourselves with it.

== Get familiar with the interface ==

=== Load a Genome and Data Tracks===

By default, IGV loads Human hg19. If you work with another version of the human genome, or another organism altogether, you can change the genome by clicking the drop down menu in the upper-left. For this lab, we'll be using Human hg19.  

We will also load additional tracks from '''Server''':

* Ensembl genes (or your favourite source of gene annotations)
* GC Percentage
* dbSNP 1.3.1 or 1.3.7

[[Image:igv_load.data.tracks.png|thumb|550px|center|Load hg19 genome and additional data tracks]]

=== Navigation ===

You should see listing of chromosomes in this reference genome. Choose '''1''', for chromosome 1.

[[Image:igv-chromosomes.png|thumb|635px|center|Chromosome chooser]]

Navigate to '''chr1:10,000-11,000''' by entering this into the location field (in the top-left corner of the interface) and clicking '''Go'''. This shows a window of chromosome 1 that is 1,000 base pairs wide and beginning at position 10,000.

[[Image:igv-1.png|thumb|626px|center|Navigition using Location text field. Sequence track displayed as sequence of colours.]]

IGV displays the sequence of letters in a genome as a sequence of colours (e.g. A = green). This makes repetitive sequences, like the ones found at the start of this region, easy to identify.

You can navigate to a gene of interest by typing it in the same box the genomic coordinates are in and pressing Enter/Return. Try it for your favourite gene, or BRCA1 if you can't decide. 

[[Image:igv-genes.png|thumb|261px|center|Gene model.]]

Genes are represented as lines and boxes. Lines represent intronic regions, and boxes represent exotic regions. The arrows indicate the strand on which the gene lies.

When loaded, tracks are stacked on top of each other. You can identify which track is which by consulting the label to the left of each track.

== Region Lists ==

Sometimes, it's really useful to save where you are, or to load regions of interest. For this purpose, there is A '''Region Navigator''' in IGV. To access it, click Regions > Region Navigator. While you browse around the genome, you can save some bookmarks by pressing the Add button at any time.

[[Image:igv-bookmarks.png|thumb|407px|center|Bookmarks in IGV.]]

== Loading Read Alignments ==
We will be using the breast cancer cell line HCC1143 to visualize alignments.  For speed, only a small portion of chr21 will be loaded (19M:20M).

''HCC1143 Alignments to hg19:'' 
* http://bioinformatics.ca/workshop_wiki/images/5/54/HCC1143.normal.21.19M-20M.bam
* http://bioinformatics.ca/workshop_wiki/images/a/a0/HCC1143.normal.21.19M-20M.bam.bai

Copy the files to your local drive, and in IGV choose '''File > Load from File...''', select the bam file, and click '''OK'''.  Note that the bam and index files must be in the same directory for IGV to load these properly.

[[Image:igv_load_bam.png|thumb|630px|center|Load BAM track from File]]

== Visualizing read alignments ==

Navigate to a narrow window on chromosome 21: "chr21:19,480,041-19,480,386". 

To start our exploration, right click on the track-name, and select the following options:
* Sort alignments by “start location”
* Group alignments by "pair orientation"

Experiment with the various settings by right clicking the read alignment track and toggling the options. Think about which would be best for specific tasks (e.g. quality control, SNP calling, CNV finding).

[[Image:igv_sort_and_group.png|thumb|400px|center|Read information.]]

You will see reads represented by grey or white bars stacked on top of each other, where they were aligned to the reference genome. The reads are pointed to indicate their orientation (i.e. the strand on which they are mapped).  Mouse over any read and notice that a lot of information is available. To toggle read display from "hover" to "click", select the yellow box and change the setting.

[[Image:igv_show_details_on_click.png|thumb|600px|center|Read information.]]

Once you select a read, you will learn what many of these metrics mean, and how to use them to assess the quality of your datasets.  At each base that the read sequence '''mismatches''' the reference, the colour of the base represents the letter that exists in the read (using the same colour legend used for displaying the reference).

[[Image:igv_click_read.png|thumb|800px|center|Read information.]]

= Visualization Part 2: Inspecting SNPs, SNVs, and SVs =

In this section we will be looking in detail at 8 positions in the genome, and determining whether they represent real events or artifacts.

== Neighbouring somatic SNV and germline SNP ==

Navigate to position "chr21:19,479,237-19,479,814"
  # center on the SNV, sort by base (window "chr21:19,478,749-19,479,891" is centered on the SNV)
  # Sort alignments by “base”
  # Color alignments by “read strand”  
 
[[Image:igv_example1_color.png|thumb|630px|center|Example1. Good quality SNVs/SNPs]]

 '''Notes:'''
 * High base qualities in all reads except one (where the alt allele is the last base of the read) 
 * Good mapping quality of reads, no strand bias, allele frequency consistent with heterozygous mutation

 '''Question(s):'''
 * What does Shade base by quality do? How might this be helpful?
 * How does Color by "read strand" help?

== Homopolymer repeat with indel ==

Navigate to position "chr21:19,518,412-19,518,497"
 # Group alignments by "read strand"
 # Center on the second "T", and Sort alignments by “base” on the forward strand reads

[[Image:igv_example2a.png|thumb|630px|center|Example2]]

 # center on the one base deletion, and Sort alignments by “base” on the reverse strand reads
 
[[Image:igv_example2b.png|thumb|630px|center|Example2]]

 '''Notes:'''
 * The alt allele is either a deletion or insertion of one or two "T"s
 * The remaining bases are mismatched, because the alignment is now out of sync
 * The dpSNP entry at this location (rs74604068) is an A->T, and in all likelyhood an artifact 
 * (i.e. the common variants included some cases that are actually common misalignments caused by repeats)

== Coverage by GC ==

Navigate to position "chr21:19,611,925-19,631,555"  
Note that the range contains areas where coverage drops to zero in a few places.

 # use Collapsed view
 # load GC track 
   # see concordance of coverage with GC content

[[Image:igv_example3.png|thumb|630px|center|Example3]]

 '''Question:'''
 * Why are there blue and red reads throughout the alignments?

== Heterozygous SNPs on different alleles ==

Navigate to region "chr21:19,666,833-19,667,007"
  # sort by base

[[Image:igv_example4.png|thumb|630px|center|Example4]]

 '''Note:'''
 * Linkage between alleles is obvious in this case because both are spanned by the same reads

== Low mapping quality == 

Navigate to region "chr21:19,800,320-19,818,162"
# load repeat track

[[Image:igv_load_repeats.png|thumb|330px|center|Load repeats]]
[[Image:igv_example5.png|thumb|630px|center|Example5]]

 '''Notes:'''
 * Mapping quality plunges in all reads (white instead of grey).  Once we load repeat elements, we see that
 *: there are two LINE elements that cause this.

== Homozygous deletion ==

Navigate to region "chr21:19,324,469-19,331,468"

 # sort reads by insert size
 # turn on "View as Pairs" and "Expanded" view
 # click on a red read pair to pull up information on alignments

[[Image:igv_example6.png|thumb|630px|center|Example6]]

 '''Notes:'''
 * Typical insert size of read pair in the vicinity: 350bp
 * New insert size of red read pairs: 2,875bp
 * This corresponds to a homozygous deletion of 2.5kb

== Mis-alignment ==

Navigate to region "chr21:19,102,154-19,103,108"

[[Image:igv_example7.png|thumb|630px|center|Example7]]

 '''Notes:'''
 * This is a position where AluY element causes mis-alignment.  
 * Misaligned reads have mismatches to the reference and 
 * Well-aligned reads have partners on other chromosomes where additional ALuY elements are encoded.

== Translocation ==

Navigate to region "chr21:19,089,694-19,095,362"

 # Expanded view
 # Group by Pair Orientation
 # Color by Pair Orientation

[[Image:igv_example8.png|thumb|630px|center|Example8]]

 '''Notes:'''
  * many reads with mismatches to reference
  * read pairs in RL pattern (instead of LR pattern)
  * region is flanked by reads with poor mapping quality (white instead of grey)
  * presence of reads with pairs on other chromosomes (coloured reads at the bottom when scrolling down)

= Visualization Part 3: Automating Tasks in IGV =

We can use the Tools menu to invoke running a batch script.  Batch scripts are described on the IGV website:
# batch file requirements: https://www.broadinstitute.org/igv/batch
# commands recognized in a batch script: https://www.broadinstitute.org/software/igv/PortCommands
# We also need to provide sample attribute file as described here:   http://www.broadinstitute.org/software/igv/?q=SampleInformation

Download the batch script and the attribute file for our dataset:
# batch script: [[Media: run_batch_IGV_snapshots.txt|run_batch_IGV_snapshots.txt]]
# attribute file: [[Media: igv_HCC1143_attributes.txt|igv_HCC1143_attributes.txt]]

Now run the file from the Tools menu:
[[Image:igv_run_batch_script.png|thumb|630px|center|Automation]]

 '''Notes:'''
 * This script will navigate automatically to each location in the lab
 * A screenshot will be taken and saved to the screenshots directory specified


Contributors/acknowledgements
Jim Robinson, Sorana Morrissy, Obi Griffith, Malachi Griffith


