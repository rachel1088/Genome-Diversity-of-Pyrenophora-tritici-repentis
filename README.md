# Genome-Diversity-of-Pyrenophora-tritici-repentis
## Code for the poster created for the SDSU REU symposium
GitHub Repo: [Genome-Diversity-of-Pyrenophora-tritici-repentis](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis)  
Repo Author: [Rachel C. Hall](https://github.com/rachel1088)
***
<ins>About:</ins>  
This repository was created to showcase the code used for the poster presented at South Dakota State University REU symposium in Sioux Falls, SD. The project presented is to study structural variations present in the Pyrenophora trtici-repentis pathogen genome related to virulence encoding regions under selection pressure to enhance the pathogen fitness on its diverse hosts.
***
<ins>Data:</ins> 
First 25 isolates are Illumina sequenced and 26 to 28 are sequenced by Oxford Nanopore MinION at SDSU. Shown below are all of the isolates in detail.  

![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/bd68baae-a643-4d2b-bd0a-35346d7e7a7a)  
 
***
## <ins>De Novo Assembler Comparison:</ins> 
For Illumina sequenced reads, 5 de novo assemblers were compared: Shovill with SPAdes, Shovill with MEGAHIT, SOAPdenovo2, Platanus and SPAdes. For Oxford Nanopore reads, 2 de novo assemblers were compared: CLC Workbench and Flye. At the time of the poster, the Flye filtering and cleaning was not complete. 
***
### <ins>Illumina Code:</ins>
<ins>Shovill with SPAdes:</ins>  
```
for i in {1..25}; do   R1="/scratch/reeu2023/rachelhall/ptr/data/reads/combined.${i}R1.fastq";     R2="/scratch/reeu2023/rachelhall/ptr/data/reads/combined.${i}R2.fastq";   out="/scratch/reeu2023/rachelhall/ptr/data/reference_assembly/"${i}".assembled/";  shovill "$R1" "$R2" --outdir "${out}/shovill_ref_${i}"; done
```  

<ins>Shovill with MEGAHIT:</ins>  
```
for i in {1..25}; do   R1="/scratch/reeu2023/rachelhall/ptr/data/reads/combined_${i}R1.fastq";   R2="/scratch/reeu2023/rachelhall/ptr/data/reads/combined_${i}R2.fastq";   out="/scratch/reeu2023/rachelhall/ptr/data/megahit/";   shovill --assembler megahit --R1 "$R1" --R2 "$R2" --outdir "${out}/mega_ref_${i}"; done
```  

<ins>SOAPdenovo2:</ins>  
```
output=/scratch/reeu2023/rachelhall/ptr/data/soapdenovo/outputGraph for x in *.config; do SOAPdenovo-63mer all -s $x -o $output/${x%.config} -K 31; done
```  

Config File: 
```
  #maximal read length
  max_rd_len=150
  [LIB]
  #average insert size of the library
  avg_ins=300
  #if sequences are forward-reverse of reverse-forward
  reverse_seq=0
  #in which part(s) the reads are used (only contigs, only scaffolds, both contigs and scaffolds, only gap closure)
  asm_flags=3
  #cut the reads to the given length
  rd_len_cutoff=100
  #in which order the reads are used while scaffolding
  rank=1
  # cutoff of pair number for a reliable connection (at least 3 for short insert size)
  pair_num_cutoff=3
  #minimum aligned length to contigs for a reliable read location (at least 32 for short insert size)
  map_len=32
  #paired-end fastq files, read 1 file should always be followed by read 2 file
  q1=R1
  q2=R2
```

<ins>Platanus:</ins>  
```
for x in *R1.fastq; do platanus assemble -o /scratch/reeu2023/rachelhall/fusarium/data/platanus/${x%_R1.fastq} -f $x ${x%1.fastq}2.fastq; done
``` 
 
<ins>SPAdes:</ins>  
```
for i in {1..25}; do R1="/scratch/reeu2023/rachelhall/ptr/data/reads/combined.${i}R1.fastq"; R2="/scratch/reeu2023/rachelhall/ptr/data/reads/combined.${i}R2.fastq"; out="/scratch/reeu2023/rachelhall/ptr/data/reference_assembly/${i}.assembled/"; spades.py -1 "$R1" -2 "$R2" -o "${out}/spades_ref_${i}"; done
```  

### <ins>Illumina Results:</ins>
<ins>About:</ins> 
Results were determined by running QUAST and graphing in Google Sheets. Code was the same throughout, just file names and locations were changed.

Code:  
```
for i in {1..25}; do directory="/scratch/reeu2023/rachelhall/ptr/data/soapdenovo/SOAP.assembled"; contig_file="${directory}/combined.${i}assembled.contig"; output_dir="/scratch/reeu2023/rachelhall/ptr/data/soapdenovo/SOAP.assembled/reports/${i}.report"; mkdir -p "$output_dir"; quast.py -o "$output_dir" "$contig_file"; done
```

```
for i in {1..25}; do report_dir="/scratch/reeu2023/rachelhall/ptr/data/soapdenovo/SOAP.assembled/reports/${i}.report"; tsv_dir="/scratch/reeu2023/rachelhall/ptr/data/soapdenovo/SOAP.assembled/reports/tsv"; mv "${report_dir}/report.tsv" "${tsv_dir}/${i}report.tsv"; done
```  
 
```
for x in *report.tsv; do echo "Isolate ${x%report.tsv}" >> soap_combined_report.tsv; cat "$x" >> soap_combined_report.tsv; done
```
 
<ins>Shovill with SPAdes:</ins>  
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/8fae5ac4-1407-4be5-b5c9-d052bccd2aa6)  

<ins>Shovill with MEGAHIT:</ins>  
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/8a71a578-dcd7-45e3-8d40-1d6fa5df272a)  
 
<ins>SOAPdenovo2:</ins> 
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/5cf3304d-e1bd-45a7-a415-396b85aac46f)  
 
<ins>Platanus:</ins>  
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/06092685-4efb-4d6a-befd-495119dcd6d9)  
 
<ins>SPAdes:</ins> 
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/9ccdca34-5d9f-4e59-9d7b-3bc2e2ba67b9)  
***
### <ins>Oxford Nanopore Code:</ins> 
<ins>Flye:</ins> 
``` flye --nano-raw combined_barcodes.fastq --out-dir flye_out --genome-size 6m --threads 10 --trestle```
 
<ins>CLC Workbench:</ins> 
CLC Workbench has no commandline code due to it being a software. 
 
### <ins>Oxford Nanopore Results:</ins>
<ins>About:</ins> 
A table was just created to compare the two results since there wasn't as many results. 
 
<ins>Table Comparison for Oxford Nanopore Assemblies:</ins> 
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/d2f35f89-ddea-41a7-bb71-aee9c27c1a21)  
 
Code for the above table in RStudio with R 4.3.0: 
```
  library(reactable)
  library(shiny)
  
  flye <- read.table("flye_unfiltered.csv", header = TRUE, sep = ",")
  
  reactable(flye, resizable = TRUE, showPageSizeOptions = TRUE,
            onClick = "expand", highlight = TRUE, compact = TRUE, height = "auto",
            columns = list(
              De.Novo.Assembler = colDef(name = "De Novo Assembler", aggregate = "max"),
              Size..MB. = colDef(
                name = "Size (MB)",
                defaultSortOrder = "desc",
                aggregate = "mean",
                format = list(aggregated = colFormat(suffix = " (avg)", digits = 2)),
                cell = function(value) {
                  if (value >= 33.0) {
                    style <- "background-color: #b3ffcc; color: black; border-radius: 60%; width: 80%; height: 100%; display: flex; justify-content: center; align-items: center; padding: 6px;"
                  } else if (value >= 30.0) {
                    style <- "background-color: #fff7b3; color: black; border-radius: 60%; width: 80%; height: 100%; display: flex; justify-content: center; align-items: center; padding: 6px;"
                  } else {
                    style <- "background-color: #ff7f7f; color: black; border-radius: 60%; width: 80%; height: 100%; display: flex; justify-content: center; align-items: center; padding: 6px;"
                  }
                  value <- format(value, nsmall = 1)
                  span(style = style, value)
                }
              ),
              Name = colDef(name = "Isolate Number"),
              Contig = colDef(name = "Contig"),
              GC.... = colDef(name = "GC %"),
              N50..bp. = colDef(name = "N50 (bp)"),
              L50..bp. = colDef(name = "L50 (bp)")
            )
  )
```
***
## <ins>Oxford Nanopore Genome Ribbon:</ins> 
The genome ribbon was created on CLC Workbench with the three new isolates that were sequenced with MinION. The reference shown at the bottom is *Pyrenophora tritici-repentis* Pt-1C-BFP. Barcode 01 is isolate 26, barcode 02 is isolate 27 and barcode 03 is isolate 28.  

### <ins>Genome Ribbon Results:</ins> 
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/6eeede08-ca31-4caf-9d84-02015747cd38) 

## <ins>Not Shown on the Poster:</ins> 
Not all the code and figures created made the cut for the poster, but it is still relevant data. Phylogenetic trees were created for the Tox genes and necrosis gene found in *Pyrenophora tritici-repentis*. These genes were gathered from the NCBI, then were blasted with BLAST+ to find the sequences which were then ran through Clustal-Omega and that output file was used in RAXML-NG to make a phylogenetic tree with bootstraps number and visualized in MEGA11. 

## <ins>Code for Phylogenetic Trees:</ins> 
<ins>NCBI Tox Genes:</ins>  
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/177fe368-35e6-41c4-a416-5b1eb2bc0707)   
  
<ins>BLAST+:</ins> 
 ```
makeblastdb -in combined.fasta -dbtype nucl -out genomedb
```
```
blastn -query referene_gene.fasta -db genomedb -out gene_name -outfmt "6 sseqid sseq" -evalue 1e-30
```

<ins>Clustal-Omega:</ins> 
```
clustalo -i gene_name -o  aligned_genes.fasta --threads=4
```

<ins>RAXML-NG:</ins> 
```
raxml-ng --all --msa-format FASTA --msa aligned_genes.fasta --model LG+G8+F --tree pars{10} --bs-trees 200 --threads 5 --prefix gene_name
```

<ins>MEGA11:</ins>  
The output files of RAXML-NG (.bestTree), were turned into a .newick file and then uploaded to MEGA11 to be visualized.

### <ins>MEGA11 Results:</ins>
<ins>Pti2 ToxC Gene:</ins>  
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/3077a4d2-3620-4fd0-9e6a-83dcb6105053)  
 
<ins>331-9 ToxC Gene:</ins> 
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/f9b374e9-a90e-4263-966c-e365ab6c4e6d)  
  
<ins>EW4-4 ToxA Gene:</ins> 
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/b203f9d7-0421-4c13-9ee5-954f7580ba8e)  
  
<ins>Necrosis Gene:</ins> 
![image](https://github.com/rachel1088/Genome-Diversity-of-Pyrenophora-tritici-repentis/assets/139996981/4c59dc74-6eb2-4e64-af66-4c3ac121e342)  
