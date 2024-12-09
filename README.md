# README

To access the website go to [https://rnayebi21.github.io/floogle/var/www/html/jbrowse2/index.html](https://rnayebi21.github.io/floogle/var/www/html/jbrowse2/index.html)

## Pre-requisites

Ensure the following tools are installed and available in your environment:

- `wget`: For downloading files from FTP.
- `gunzip`: For decompressing `.gz` files.
- `samtools`: For indexing FASTA files.
- `jbrowse`: JBrowse CLI for adding assemblies and tracks.
- `bgzip` and `tabix`: For compressing and indexing `.gff` files.

## Steps for Uploading Genomic Assemblies

### Create a Temporary Directory

Create a temporary directory to store downloaded files:

```bash
mkdir tmp
```

### Set the `APACHE_ROOT` path

```bash
export APACHE_ROOT='/var/www/html'
```

### Download Genomic Data

Use `wget` to download the genomic FASTA and annotation GFF files. Replace the URLs with your specific assembly links. Example:

```bash
wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/007/335/GCA_039007335.1_ASM3900733v1/GCA_039007335.1_ASM3900733v1_genomic.fna.gz
wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/007/335/GCA_039007335.1_ASM3900733v1/GCA_039007335.1_ASM3900733v1_genomic.gff.gz
```

### Process the Genomic FASTA File

Decompress, rename, and index the FASTA file:

```bash
gunzip GCA_039007335.1_ASM3900733v1_genomic.fna.gz
mv GCA_039007335.1_ASM3900733v1_genomic.fna co_2018.fa
samtools faidx co_2018.fa
```

### Add the Assembly to JBrowse

Add the processed assembly to JBrowse using:

```bash
jbrowse add-assembly co_2018.fa --out $APACHE_ROOT/jbrowse2 --load copy
```

### Process the Annotation GFF File

Sort, compress, and index the GFF file:

```bash
gunzip GCA_039007335.1_ASM3900733v1_genomic.gff.gz
jbrowse sort-gff GCA_039007335.1_ASM3900733v1_genomic.gff > co_2018_genes.gff
bgzip co_2018_genes.gff
tabix co_2018_genes.gff.gz
```

### Add the Track to JBrowse

Add the annotation track to JBrowse:

```bash
jbrowse add-track co_2018_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames co_2018
```

---

## Repeat for Multiple Assemblies

Follow the above steps for each genomic assembly and annotation. Replace file names and assembly names as necessary.

---

## Final Steps

After all assemblies and tracks are added, generate the JBrowse text index:

```bash
jbrowse text-index --out $APACHE_ROOT/jbrowse2
```

## For all Data Sources:

Using all of the data sources this is what the entire script looks like

```bash
wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/007/335/GCA_039007335.1_ASM3900733v1/GCA_039007335.1_ASM3900733v1_genomic.fna.gz

gunzip GCA_039007335.1_ASM3900733v1_genomic.fna.gz
mv GCA_039007335.1_ASM3900733v1_genomic.fna co_2018.fa
samtools faidx co_2018.fa
jbrowse add-assembly co_2018.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/007/335/GCA_039007335.1_ASM3900733v1/GCA_039007335.1_ASM3900733v1_genomic.gff.gz

gunzip GCA_039007335.1_ASM3900733v1_genomic.gff.gz
jbrowse sort-gff GCA_039007335.1_ASM3900733v1_genomic.gff > co_2018_genes.gff
bgzip co_2018_genes.gff
tabix co_2018_genes.gff.gz
jbrowse add-track co_2018_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames co_2018

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/037/922/585/GCA_037922585.1_ASM3792258v1/GCA_037922585.1_ASM3792258v1_genomic.fna.gz

gunzip GCA_037922585.1_ASM3792258v1_genomic.fna.gz
mv GCA_037922585.1_ASM3792258v1_genomic.fna russia_1977.fa
samtools faidx russia_1977.fa
jbrowse add-assembly russia_1977.fa --out /var/www/html/jbrowse2/ --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/037/922/585/GCA_037922585.1_ASM3792258v1/GCA_037922585.1_ASM3792258v1_genomic.gff.gz

gunzip GCA_037922585.1_ASM3792258v1_genomic.gff.gz
jbrowse sort-gff GCA_037922585.1_ASM3792258v1_genomic.gff > russia_1977_genes.gff
bgzip russia_1977_genes.gff
tabix russia_1977_genes.gff.gz
jbrowse add-track russia_1977_genes.gff.gz --out /var/www/html/jbrowse2 --load copy --assemblyNames russia_1977

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/038/607/495/GCA_038607495.1_ASM3860749v1/GCA_038607495.1_ASM3860749v1_genomic.fna.gz

gunzip GCA_038607495.1_ASM3860749v1_genomic.fna.gz
mv GCA_038607495.1_ASM3860749v1_genomic.fna netherlands_2011.fa
samtools faidx netherlands_2011.fa

jbrowse add-assembly netherlands_2011.fa --out /var/www/html/jbrowse2/ --load copy
wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/038/607/495/GCA_038607495.1_ASM3860749v1/GCA_038607495.1_ASM3860749v1_genomic.gff.gz

gunzip GCA_038607495.1_ASM3860749v1_genomic.gff.gz
jbrowse sort-gff GCA_038607495.1_ASM3860749v1_genomic.gff > netherlands_2011_genes.gff
bgzip netherlands_2011_genes.gff
tabix netherlands_2011_genes.gff.gz
jbrowse add-track netherlands_2011_genes.gff.gz --out /var/www/html/jbrowse2 --load copy --assemblyNames netherlands_2011

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/037/915/055/GCA_037915055.1_ASM3791505v1/GCA_037915055.1_ASM3791505v1_genomic.fna.gz

gunzip GCA_037915055.1_ASM3791505v1_genomic.fna.gz
mv GCA_037915055.1_ASM3791505v1_genomic.fna wsn_1933.fa
samtools faidx wsn_1933.fa
jbrowse add-assembly wsn_1933.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/037/915/055/GCA_037915055.1_ASM3791505v1/GCA_037915055.1_ASM3791505v1_genomic.gff.gz

gunzip GCA_037915055.1_ASM3791505v1_genomic.gff.gz
jbrowse sort-gff GCA_037915055.1_ASM3791505v1_genomic.gff > wsn_1933_genes.gff
bgzip wsn_1933_genes.gff
tabix wsn_1933_genes.gff.gz
jbrowse add-track wsn_1933_genes.gff.gz --out /var/www/html/jbrowse2 --load copy --assemblyNames wsn_1933

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/037/915/385/GCA_037915385.1_ASM3791538v1/GCA_037915385.1_ASM3791538v1_genomic.fna.gz

gunzip GCA_037915385.1_ASM3791538v1_genomic.fna.gz
mv GCA_037915385.1_ASM3791538v1_genomic.fna puerto_rico_1934.fa
samtools faidx puerto_rico_1934.fa
jbrowse add-assembly puerto_rico_1934.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/037/915/385/GCA_037915385.1_ASM3791538v1/GCA_037915385.1_ASM3791538v1_genomic.gff.gz

gunzip GCA_037915385.1_ASM3791538v1_genomic.gff.gz
jbrowse sort-gff GCA_037915385.1_ASM3791538v1_genomic.gff > puerto_rico_1934_genes.gff
bgzip puerto_rico_1934_genes.gff
tabix puerto_rico_1934_genes.gff.gz
jbrowse add-track puerto_rico_1934_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames puerto_rico_1934

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/005/145/GCA_039005145.1_ASM3900514v1/GCA_039005145.1_ASM3900514v1_genomic.fna.gz

gunzip GCA_039005145.1_ASM3900514v1_genomic.fna.gz
mv  GCA_039005145.1_ASM3900514v1_genomic.fna H1N1_Japan_2018.fa
samtools faidx H1N1_Japan_2018.fa
jbrowse add-assembly H1N1_Japan_2018.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/005/145/GCA_039005145.1_ASM3900514v1/GCA_039005145.1_ASM3900514v1_genomic.gff.gz

gunzip GCA_039005145.1_ASM3900514v1_genomic.gff.gz
jbrowse sort-gff GCA_039005145.1_ASM3900514v1_genomic.gff > h1n1_genes.gff
bgzip h1n1_genes.gff
tabix h1n1_genes.gff.gz
Jbrowse add-track h1n1_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/038/186/485/GCA_038186485.1_ASM3818648v1/GCA_038186485.1_ASM3818648v1_genomic.fna.gz

gunzip GCA_038186485.1_ASM3818648v1_genomic.fna.gz
mv GCA_038186485.1_ASM3818648v1_genomic.fna new_york_2001.fa
samtools faidx new_york_2001.fa
jbrowse add-assembly new_york_2001.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/038/186/485/GCA_038186485.1_ASM3818648v1/GCA_038186485.1_ASM3818648v1_genomic.gff.gz

gunzip GCA_038186485.1_ASM3818648v1_genomic.gff.gz
jbrowse sort-gff GCA_038186485.1_ASM3818648v1_genomic.gff > new_york_2001_genes.gff
bgzip new_york_2001_genes.gff
tabix new_york_2001_genes.gff.gz
jbrowse add-track new_york_2001_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames new_york_2001

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/085/925/GCA_039085925.1_ASM3908592v1/GCA_039085925.1_ASM3908592v1_genomic.fna.gz

gunzip GCA_039085925.1_ASM3908592v1_genomic.fna.gz
mv GCA_039085925.1_ASM3908592v1_genomic.fna alaska_2018.fa
samtools faidx alaska_2018.fa
jbrowse add-assembly alaska_2018.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget 
https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/085/925/GCA_039085925.1_ASM3908592v1/GCA_039085925.1_ASM3908592v1_genomic.gff.gz

gunzip GCA_039085925.1_ASM3908592v1_genomic.gff.gz
jbrowse sort-gff GCA_039085925.1_ASM3908592v1_genomic.gff > alaska_2018_genes.gff
bgzip alaska_2018_genes.gff
tabix alaska_2018_genes.gff.gz
jbrowse add-track alaska_2018_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames alaska_2018

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/038/995/955/GCA_038995955.1_ASM3899595v1/GCA_038995955.1_ASM3899595v1_genomic.fna.gz

gunzip GCA_038995955.1_ASM3899595v1_genomic.fna.gz
mv GCA_038995955.1_ASM3899595v1_genomic.fna arkansas_2017.fa
samtools faidx arkansas_2017.fa
jbrowse add-assembly arkansas_2017.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/038/995/955/GCA_038995955.1_ASM3899595v1/GCA_038995955.1_ASM3899595v1_genomic.gff.gz

gunzip GCA_038995955.1_ASM3899595v1_genomic.gff.gz
jbrowse sort-gff GCA_038995955.1_ASM3899595v1_genomic.gff > arkansas_2017_genes.gff
bgzip arkansas_2017_genes.gff
tabix arkansas_2017_genes.gff.gz
jbrowse add-track arkansas_2017_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames arkansas_2017

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/063/395/GCA_039063395.1_ASM3906339v1/GCA_039063395.1_ASM3906339v1_genomic.fna.gz

gunzip GCA_039063395.1_ASM3906339v1_genomic.fna.gz
mv GCA_039063395.1_ASM3906339v1_genomic.fna oklahoma_2018.fa
samtools faidx oklahoma_2018.fa
jbrowse add-assembly oklahoma_2018.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/063/395/GCA_039063395.1_ASM3906339v1/GCA_039063395.1_ASM3906339v1_genomic.gff.gz

gunzip GCA_039063395.1_ASM3906339v1_genomic.gff.gz
jbrowse sort-gff GCA_039063395.1_ASM3906339v1_genomic.gff > oklahoma_2018_genes.gff
bgzip oklahoma_2018_genes.gff
tabix oklahoma_2018_genes.gff.gz
jbrowse add-track oklahoma_2018_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames oklahoma_2018

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/928/555/GCF_000928555.1_ViralMultiSegProj274585/GCF_000928555.1_ViralMultiSegProj274585_genomic.fna.gz

gunzip GCF_000928555.1_ViralMultiSegProj274585_genomic.fna.gz
mv GCF_000928555.1_ViralMultiSegProj274585_genomic.fna shanghai_2013.fa
samtools faidx shanghai_2013.fa
jbrowse add-assembly shanghai_2013.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/928/555/GCF_000928555.1_ViralMultiSegProj274585/GCF_000928555.1_ViralMultiSegProj274585_genomic.gff.gz

gunzip GCF_000928555.1_ViralMultiSegProj274585_genomic.gff.gz
jbrowse sort-gff GCF_000928555.1_ViralMultiSegProj274585_genomic.gff > shanghai_2013_genes.gff
bgzip shanghai_2013_genes.gff
tabix shanghai_2013_genes.gff.gz
jbrowse add-track shanghai_2013_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames shanghai_2013

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/023/425/GCA_039023425.1_ASM3902342v1/GCA_039023425.1_ASM3902342v1_genomic.fna.gz

gunzip GCA_039023425.1_ASM3902342v1_genomic.fna.gz
mv GCA_039023425.1_ASM3902342v1_genomic.fna brownsville_2009.fa
samtools faidx brownsville_2009.fa
jbrowse add-assembly brownsville_2009.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/023/425/GCA_039023425.1_ASM3902342v1/GCA_039023425.1_ASM3902342v1_genomic.gff.gz

gunzip GCA_039023425.1_ASM3902342v1_genomic.gff.gz
jbrowse sort-gff GCA_039023425.1_ASM3902342v1_genomic.gff > brownsville_2009_genes.gff
bgzip brownsville_2009_genes.gff
tabix brownsville_2009_genes.gff.gz
jbrowse add-track brownsville_2009_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames brownsville_2009

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/038/576/375/GCA_038576375.1_ASM3857637v1/GCA_038576375.1_ASM3857637v1_genomic.fna.gz

gunzip GCA_038576375.1_ASM3857637v1_genomic.fna.gz
mv GCA_038576375.1_ASM3857637v1_genomic.fna texas_jms400_2009.fa
samtools faidx texas_jms400_2009.fa
jbrowse add-assembly texas_jms400_2009.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/038/576/375/GCA_038576375.1_ASM3857637v1/GCA_038576375.1_ASM3857637v1_genomic.gff.gz

gunzip GCA_038576375.1_ASM3857637v1_genomic.gff.gz
jbrowse sort-gff GCA_038576375.1_ASM3857637v1_genomic.gff > texas_jms400_2009_genes.gff
bgzip texas_jms400_2009_genes.gff
tabix texas_jms400_2009_genes.gff.gz
jbrowse add-track texas_jms400_2009_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames texas_jms400_2009

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/001/343/785/GCF_001343785.1_ViralMultiSegProj274766/GCF_001343785.1_ViralMultiSegProj274766_genomic.fna.gz

gunzip GCF_001343785.1_ViralMultiSegProj274766_genomic.fna.gz
mv GCF_001343785.1_ViralMultiSegProj274766_genomic.fna H1N1_California_Human_2009.fa
samtools faidx H1N1_California_Human_2009.fa
jbrowse add-assembly H1N1_California_Human_2009.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/001/343/785/GCF_001343785.1_ViralMultiSegProj274766/GCF_001343785.1_ViralMultiSegProj274766_genomic.gff.gz

gunzip GCF_001343785.1_ViralMultiSegProj274766_genomic.gff.gz
jbrowse sort-gff GCF_001343785.1_ViralMultiSegProj274766_genomic.gff > cali_human_2009_genes.gff
bgzip cali_human_2009_genes.gff
tabix cali_human_2009_genes.gff.gz
jbrowse add-track cali_human_2009_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames H1N1_California_Human_2009

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/038/576/315/GCA_038576315.1_ASM3857631v1/GCA_038576315.1_ASM3857631v1_genomic.fna.gz

gunzip GCA_038576315.1_ASM3857631v1_genomic.fna.gz
mv GCA_038576315.1_ASM3857631v1_genomic.fna H1N1_Guangdong_Swine_2009.fa
samtools faidx H1N1_Guangdong_Swine_2009.fa
jbrowse add-assembly H1N1_Guangdong_Swine_2009.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/038/576/315/GCA_038576315.1_ASM3857631v1/GCA_038576315.1_ASM3857631v1_genomic.gff.gz

gunzip GCA_038576315.1_ASM3857631v1_genomic.gff.gz
jbrowse sort-gff GCA_038576315.1_ASM3857631v1_genomic.gff > guangdong_swine_2009_genes.gff
bgzip guangdong_swine_2009_genes.gff
tabix guangdong_swine_2009_genes.gff.gz
jbrowse add-track guangdong_swine_2009_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames H1N1_Guangdong_Swine_2009

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/202/455/GCA_039202455.1_ASM3920245v1/GCA_039202455.1_ASM3920245v1_genomic.fna.gz

gunzip GCA_039202455.1_ASM3920245v1_genomic.fna.gz
mv GCA_039202455.1_ASM3920245v1_genomic.fna H1N1_Alabama_2020.fa
samtools faidx H1N1_Alabama_2020.fa
jbrowse add-assembly H1N1_Alabama_2020.fa --out $APACHE_ROOT/jbrowse2 --load copy

wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/039/202/455/GCA_039202455.1_ASM3920245v1/GCA_039202455.1_ASM3920245v1_genomic.gff.gz

gunzip GCA_039202455.1_ASM3920245v1_genomic.gff.gz
jbrowse sort-gff GCA_039202455.1_ASM3920245v1_genomic.gff > alabama_2020_genes.gff
bgzip alabama_2020_genes.gff
tabix alabama_2020_genes.gff.gz
jbrowse add-track alabama_2020_genes.gff.gz --out $APACHE_ROOT/jbrowse2 --load copy --assemblyNames H1N1_Alabama_2020

jbrowse text-index --out $APACHE_ROOT/jbrowse2

```

## Instructions for Viewing Multiple Sequence Alignments

Go into the config.json file in the `$APACHE_ROOT/jbrowse2` folder and add these configurations to the end of the file to install the MSAViewer plugin on your JBrowse instance.

```json
"plugins": [
    {
      "name": "MsaView",
      "url": "https://unpkg.com/jbrowse-plugin-msaview/dist/jbrowse-plugin-msaview.umd.production.min.js"
    }
]
```

Install Clustal Omega on your system.

```bash
sudo apt -y install clustalo
```

From here, reconfigure the .fa files for each viral genome assembly so that they are non-segmented. This step is necessary to ensure that MSA compares the entire viral genomes to each other.

```bash
cd tmp
export input_fasta=co_2018.fa
export output_fasta=co_2018_cont.fa
echo ">co_2018" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta
```

Repeat these commands for each viral genome assembly in your tmp folder. For our dataset, run these bash commands to create the modified .fa files.

```bash
export input_fasta=co_2018.fa
export output_fasta=co_2018_cont.fa
echo ">co_2018" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

export input_fasta=russia_1977.fa
export output_fasta=russia_1977_cont.fa
echo ">russia_1977" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

export input_fasta=netherlands_2011.fa
export output_fasta=netherlands_2011_cont.fa
echo ">netherlands_2011" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

export input_fasta=wsn_1933.fa
export output_fasta=wsn_1933_cont.fa
echo ">wsn_1933" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

export input_fasta=puerto_rico_1934.fa
export output_fasta=puerto_rico_1934_cont.fa
echo ">puerto_rico_1934" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

export input_fasta=H1N1_Japan_2018.fa
export output_fasta=H1N1_Japan_2018_cont.fa
echo ">H1N1_Japan_2018" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

export input_fasta=new_york_2001.fa
export output_fasta=new_york_2001_cont.fa
echo ">new_york_2001" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

export input_fasta=alaska_2018.fa
export output_fasta=alaska_2018_cont.fa
echo ">alaska_2018" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

input_fasta=arkansas_2017.fa
output_fasta=arkansas_2017_cont.fa
echo ">arkansas_2017" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

input_fasta=oklahoma_2018.fa
output_fasta=oklahoma_2018_cont.fa
echo ">oklahoma_2018" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

input_fasta=shanghai_2013.fa
output_fasta=shanghai_2013_cont.fa
echo ">shanghai_2013" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

input_fasta=brownsville_2009.fa
output_fasta=brownsville_2009_cont.fa
echo ">brownsville_2009" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

input_fasta=texas_jms400_2009.fa
output_fasta=texas_jms400_2009_cont.fa
echo ">texas_jms400_2009" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

input_fasta=H1N1_California_Human_2009.fa
output_fasta=H1N1_California_Human_2009_cont.fa
echo ">H1N1_California_Human_2009" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

input_fasta=H1N1_Guangdong_Swine_2009.fa
output_fasta=H1N1_Guangdong_Swine_2009_cont.fa
echo ">H1N1_Guangdong_Swine_2009" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

input_fasta=H1N1_Alabama_2020.fa
output_fasta=H1N1_Alabama_2020_cont.fa
echo ">H1N1_Alabama_2020" > $output_fasta
grep -v '^>' $input_fasta | tr -d '\n' >> $output_fasta
echo -e "" >> $output_fasta

```

Once you have modified the .fa files, concatenate them so they can be run through Clustal Omega. Then, run MSA on the concatenated file.

```bash
cat co_2018_cont.fa russia_1977_cont.fa netherlands_2011_cont.fa wsn_1933_cont.fa puerto_rico_1934_cont.fa H1N1_Japan_2018_cont.fa new_york_2001_cont.fa alaska_2018_cont.fa arkansas_2017_cont.fa oklahoma_2018_cont.fa shanghai_2013_cont.fa brownsville_2009_cont.fa texas_jms400_2009_cont.fa H1N1_California_Human_2009_cont.fa H1N1_Guangdong_Swine_2009_cont.fa H1N1_Alabama_2020_cont.fa > all_genomes.fa
clustalo -i all_genomes.fa -o aligned_sequences.msa --outfmt=clu 
```

Running the clustalo command on an AWS instance may result in the job being killed due to the instance not having enough RAM to complete the alignment. Follow the instructions on [https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04)  to add swap memory to the instance.

Click on the “Multiple sequence alignment view” link on your JBrowse instance. From there, upload the .msa file to the instance, and the alignment should display on the window.

![Screen Shot 2024-12-08 at 8.36.49 PM.png](README%201579ad28a8e1807a83d8fcb3469c83fa/Screen_Shot_2024-12-08_at_8.36.49_PM.png)

![Screen Shot 2024-12-08 at 8.39.43 PM.png](README%201579ad28a8e1807a83d8fcb3469c83fa/Screen_Shot_2024-12-08_at_8.39.43_PM.png)
