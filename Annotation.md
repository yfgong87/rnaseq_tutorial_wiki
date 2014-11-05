#ANNOTATION
Obtain known gene/transcript annotations
In this tutorial we will use annotations obtained from Illumina's iGenomes for chromosome 22 only
For time reasons, these have been downloaded for you.
But you should get familiar with sources of gene annotations for RNA-seq analysis
Copy the gene annotation files to the working directory. 
	
	cd $RNA_HOME/refs/hg19/
	mkdir genes
	cd genes
	cp /media/cbwdata/CourseData/RNA_data/iGenomes/Homo_sapiens/Ensembl/GRCh37/Annotation/Genes/genes_chr22.gtf .
	less -p start_codon -S genes_chr22.gtf
Press 'q' to exit the 'less' display
	
How many unique gene IDs are in the .gtf file?
We can use a perl command-line command to find out:

	perl -ne 'if ($_ =~ /(gene_id\s\"ENSG\w+\")/){print "$1\n"}' genes_chr22.gtf | sort | uniq | wc -l
	
Using perl -ne '' will execute the code between single quotes, on the .gtf file, line-by-line.
The $_ variable holds the contents of each line
The 'if ($_ =~//)' is a pattern-matching command which will look for the pattern "gene_id" followed by a space followed by "ENSG" and one or more word characters (indicated by \w+) surrounded by double quotes.
The pattern to be matched is enclosed in parentheses. This allows us to print it out from the special variable $1.
The output of this perl command will be a long list of ENSG Ids. By piping to sort, then uniq, then word count we can count the unique number of genes in the file
To learn more, see:
http://perldoc.perl.org/perlre.html#Regular-Expressions
http://www.perl.com/pub/2004/08/09/commandline.html
	
	
Definitions:
- Reference genome - The nucleotide sequence of the chromosomes of a species.  Genes are the functional units of a reference genome and gene annotations describe the structure of transcripts expressed from those gene loci.  

- Gene annotations - Descriptions of gene/transcript models for a genome.  A transcript model consists of the *coordinates* of the exons of a transcript on a reference genome.  Additional information such as the strand the transcript is generated from, gene name, coding portion of the transcript, alternate transcript start sites, and other information may be provided.

- GTF (.gtf) file - A common file format referred to as Gene Transfer Format used to store gene and transcript annotation information.  You can learn more about this format here:
http://genome.ucsc.edu/FAQ/FAQformat#format3
http://genome.ucsc.edu/FAQ/FAQformat#format4
	
The purpose of gene annotations (obtained as a .gtf file):
When running the TopHat/Cufflinks/CuffDiff pipeline, known gene/transcript annotations are used for several purposes:
* During the TopHat alignment step, annotations may be provided as a .gtf file using the '-G' option.  TopHat will align reads against the transcriptome first followed by the reference genome.
* During the TopHat alignment step, a junctions database will be assembled from the transcripts in your .gtf file.  TopHat will align reads that do not map within an exon against this junctions database to identify spliced read alignments.  If an alignment still can not be found it will attempt to determine if the read corresponds to a novel exon-exon junction.
* During the Cufflinks step, a .gtf file can be used to specify the transcript models to estimate expression estimates for using the '-G' option (not the same as the -G option for TopHat mentioned above).  This mode of Cufflinks will give you one expression estimate for each of the transcripts in your .gtf file giving you a 'microarray like' expression result.
* During the Cufflinks step, a .gtf file can be used to 'guide' the assembly of novel transcripts (using the '-g' option).  Instead of assuming the known transcript models are correct, they are used as a guide and the resulting expression estimates will correspond to both known and novel/predicted transcripts.
* During the Cuffdiff step, a .gtf file is used to determine the transcripts that will be examined for differential expression.  These may be known transcripts that you download from a public source or a .gtf of transcripts predicted by Cufflinks from the read data.
	
Obtaining gene annotation files formatted for TopHat/Cufflinks/Cuffdiff.
There are many possible sources of .gtf gene/transcript annotation files.  For example, from Ensembl, UCSC, RefSeq, etc.  Three options and related instructions for obtaining the gene annotation files are provided below.
	
1. ILLUMINA IGENOMES.  
Formatted specifically for use with TopHat Cuffinks.  Based on UCSC, Refseq/NCBI, or Ensembl annotations.  Available for many species.  Bowtie indexed reference genome files are pre-computed for your convenience.  Download here:
http://cufflinks.cbcb.umd.edu/igenomes.html
  * Use wget (or similar) to download the Homo_sapiens_Ensembl_GRCh37.tar.gz file 
  * This is found under Homo sapiens -> Ensembl -> GRCh37
  * unzip/untar
  * Individual chromosome fasta sequence files can be in Homo_sapiens/Ensembl/GRCh37/Sequence/Chromosomes/
  * GTF file can be found in Homo_sapiens/Ensembl/GRCh37/Annotation/Genes/genes.gtf
	
2. ENSEMBL FTP SITE  
Based on Ensembl annotations only.  Available for many species.
http://useast.ensembl.org/info/data/ftp/index.html
	
3. UCSC TABLE BROWSER  
Based on UCSC annotations or several other possible annotation sources collected by UCSC. You might chose this option if you want to have a lot of flexibility in the annotations you obtain.  e.g. to grab only the transcripts from chromosome 22 as in the following example:
  * Open the following in your browser: http://genome.ucsc.edu/
  * Click 'Tables' at the top of the page.
  * Select 'Mammal', 'Human', and 'Feb. 2009 (GRCh37/hg19)' from the first row of drop down menus.
  * Select 'Genes and Gene Prediction Tracks' and 'UCSC Genes' from the second row of drop down menus.
    To limit your selection to only chromosome 22, select the 'position' option beside 'region', enter 'chr22' in the 'position' box.
  * Select 'GTF - gene transfer format' for output format and enter 'UCSC_Genes.gtf' for output file.
  * Hit the 'get output' button and save the file.  Make note of its location
	
In addition to the .gtf file you may find uses for some extra files providing alternatively formatted or additional information on the same transcripts.  For example:
How to get a Gene bed file:
  * Change the output format to 'BED - browser extensible data'.
  * Change the output file to 'UCSC_Genes.bed', and hit the 'get output' button.
  * Make sure 'Whole Gene' is selected, hit the 'get BED' button, and save the file.
	
How to get an Exon bed file:
i.) Go back one page in your browser and change the output file to 'UCSC_Exons.bed', the hit the 'get output' button again.
j.) Select 'Exons plus', enter 0 in the adjacent box, hit the 'get BED' button, and save the file.
	
How to get gene symbols and descriptions for all UCSC genes:
k.) Again go back one page in your browser and change the 'output format' to 'selected fields from primary and related tables'.
l.) Change the output file to 'UCSC_Names.txt', and hit the 'get output' button.
m.) Make sure 'chrom' is selected near the top of the page.
m.) Under 'Linked Tables' make sure 'kgXref' is selected, and then hit 'Allow Selection From Checked Tables'.  This will link the table and give you access to its fields.
n.) Under 'hg19.kgXref fields' select: 'kgID', 'geneSymbol', 'description'. 
o.) Hit the 'get output' button and save the file.
	
To get annotations for the whole genome, make sure 'genome' is selected beside 'region'.
By default, the files downloaded above will be compressed.  To decompress, use 'gunzip filename' in linux.
	
Important note on chromosome naming conventions:
In order for your TopHat/Cufflinks analysis to work, the chromosome names in you .gtf file *must match* those in your reference genome (i.e. your reference genome fasta file).  If you get a Cufflinks result where all transcripts have an expression value of 0, you may have overlooked this.  Unfortunately, Ensembl, NCBI, and UCSC can not agree on how to name the chromosomes in many species, so this problem may come up often.  You can avoid this by getting a complete reference genome and gene annotation package from the Illumina iGenomes project mentioned above.
	
Important note on reference genome builds:
Your annotations must correspond to the same reference genome build as your reference genome fasta file.  e.g. both correspond to UCSC human build 'hg18', NCBI human build '37', etc..  Even if both your reference genome and annotations are from UCSC or Ensembl they could still correspond to different versions of that genome.  This would cause problems in any RNA-seq pipeline.
	