# GA2 answers - `Noah Dameron`

## Part A: Starting a Git repository

### 1: Copy the template file and add your name

```bash
# Current directory is /fs/ess/PAS3493/people/ndameron

# Make GA2 directory
mkdir GA2/

# Move to GA2 directory
cd GA2/

# Copy template to directory and rename as README.md
cp /fs/ess/PAS3493/share/GA2/GA2_template.md README.md
```

After copying the template to the GA2 dir and renaming, the GA2 folder was opened which restarted VS Code. This moves the entire VS Code environment to this folder. 


### 2: Initialize a Git repository

To initialize a Git repo, use `git init` then confirm the repo was made using `git status`.

### 3: Stage and commit README

Use the `git add` command to stage `README.md`. Check using `git status`. Then to commit the file, use `git commit -m` followed by a message describing the action. Check using `git status`. Check commit history using `git log`. 

## Part B: Exploring the GTF file with Unix data tools

### 4: Create `data`/`results` dirs and copy the GTF file

```bash
# Make a dir called data and a dir called results
mkdir data results

# Copy GTF file annot.gtf.gz from garrigos dir to data dir, keeping the same file name
cp /fs/ess/PAS3493/people/ndameron/garrigos/ref/annot.gtf.gz data/

# Verify directories were made and file was copied
tree
```

The output of the command was:

```
.
├── data
│   └── annot.gtf.gz
├── README.md
└── results

2 directories, 2 files
```

### 5: File size of `annot.gtf.gz`

```bash
# List file size of annot.gtf.gz (current directory is GA2/)
ls -lh data/annot.gtf.gz
```

The output of the command was:

```
-rw-rw----+ 1 ndameron PAS2401 5.1M Sep 22 11:28 data/annot.gtf.gz
```

The file size of `annot.gtf.gz` is 5.1 M.

### 6: Decompress the GTF file and check its size

```bash
gunzip data/annot.gtf.gz
```
```bash
# Check the file size of the uncompressed file
ls -lh data/annot.gtf
```

The output of the command was:

```
-rw-rw----+ 1 ndameron PAS2401 123M Sep 22 11:45 data/annot.gtf
```
The unzipped file is 123 M. 

*Answer*: The uncompressed file is approximately 24 times larger.

### 7: Total number of lines in `annot.gtf`

The GTF file starts with 4 header lines followed by the table of "genomic features"
```bash
# Total number of lines (including headers)
wc -l data/annot.gtf

# Total number of lines (excluding headers)
tail -n +5 data/annot.gtf | wc -l
```

The output of the command was:

```
# Total number of lines (including headers)
408402 data/annot.gtf

# Total number of lines (excluding headers)
408398
```

*Answer*: Total number of lines: 408402 lines (including headers)

### 8: Number of lines in the table section (header lines counted by eye)

```bash
# Open GTF file to few lines (lines are not wrapped by using -S)
less -S annot.gtf

# Total number of lines (excluding headers)
tail -n +5 data/annot.gtf | wc -l
```

The output of the command was:

```
Output of GTF file: 

#gtf-version 2.2
#!genome-build TS_CPP_V2
#!genome-build-accession NCBI_Assembly:GCF_016801865.2
#!annotation-source NCBI RefSeq GCF_016801865.2-RS_2022_12
NC_068937.1     Gnomon  gene    2046    110808  .       +       .       gene_id "LOC120427725"; transcript_id ""; db_xref "GeneID:120427725"; description "homeotic protein deformed"; gbkey "Gene"; gene "LOC120427725"; >
NC_068937.1     Gnomon  transcript      2046    110808  .       +       .       gene_id "LOC120427725"; transcript_id "XM_052707445.1"; db_xref "GeneID:120427725"; gbkey "mRNA"; gene "LOC120427725"; model_evidence "Sup>
NC_068937.1     Gnomon  exon    2046    2531    .       +       .       gene_id "LOC120427725"; transcript_id "XM_052707445.1"; db_xref "GeneID:120427725"; gene "LOC120427725"; model_evidence "Supporting evidence inclu>
NC_068937.1     Gnomon  exon    52113   52136   .       +       .       gene_id "LOC120427725"; transcript_id "XM_052707445.1"; db_xref "GeneID:120427725"; gene "LOC120427725"; model_evidence "Supporting evidence inclu>
NC_068937.1     Gnomon  exon    70113   70962   .       +       .       gene_id "LOC120427725"; transcript_id "XM_052707445.1"; db_xref "GeneID:120427725"; gene "LOC120427725"; model_evidence "Supporting evidence inclu>
NC_068937.1     Gnomon  exon    105987  106087  .       +       .       gene_id "LOC120427725"; transcript_id "XM_052707445.1"; db_xref "GeneID:120427725"; gene "LOC120427725"; model_evidence "Supporting evidence inclu>
NC_068937.1     Gnomon  exon    106551  106734  .       +       .       gene_id "LOC120427725"; transcript_id "XM_052707445.1"; db_xref "GeneID:120427725"; gene "LOC120427725"; model_evidence "Supporting evidence inclu>

# Number of lines in the table section:
408398
```

*Answer*: Number of header lines (counted by eye): 4

*Answer*: Number of lines in the table section: 408398

### 9: Number of lines in the table section (`grep -v`)

```bash
# Count the number of lines in the table section by exculding lines that start with `#` using grep -v (-v inverts what grep does)
grep -v "#" data/annot.gtf | wc -l
```

The output of the command was:

```
408397
```

*Answer*: Number of lines in the table section: 408397 <br>
(Minor discrepency between grep output and tail -n +5/self count output; after futher exploration, this is because the GTF file has `###` as its last line, which is not picked up when using tail or self count)

### 10: Count distinct sequences, save to `results/scaffolds.txt`

```bash
# Determine number of distinct sequences in file and store results as txt file
grep -v "#" data/annot.gtf | cut -f1 | sort | uniq > results/scaffolds.txt

# Count number of distinct sequences in new file
wc -l results/scaffolds.txt 
```

The output of the command was:

```
167 results/scaffolds.txt
```

*Answer*: Number of distinct sequences: 167

### 11: Frequency table of feature types

```bash
# Create frequency table of feature types
grep -v "#" data/annot.gtf | cut -f3 | sort | uniq -c
```

The output of the command was:

```
 141444 CDS
 166177 exon
  19673 gene
  25864 start_codon
  25892 stop_codon
  29347 transcript
```

### 12: Predict, then run `grep -c "gene"`

*Answer* (prediction, before running): a / **b** / c / d <br>
I predict that the correct answer is B. The `grep -c` command counts all lines that contain the target text string, in this case "gene". It does not filter for specific columns. To do this, you could use the `cut -f` command.

```bash
grep -c "gene" data/annot.gtf
```

The output of the command was:

```
408397
```

*Answer*: Was your prediction correct, and what does the comparison with
the number of genes in your frequency table from question 11 tell you?

My prediction was correct. The `grep` command, as used above, overcounts the number of genes within the GTF file as it counts lines where gene occurs, even if "gene" is not in the feature column. The frequency table shows that there are 19673 genes within the data, based on the feature column, while the `grep` command shows 408397 genes. The grep command shows nearly 20x the amount of genes than the frequency table. When we go back to question 9 looking at the number of lines in the GTF file, we see that the output of `grep` and `wc` in question 9 are the same. This further shows that grep is indiscriminately counting lines anywhere the word "gene" appears.

## Part C: Updating your Git repository

### 13: Check Git status

```bash
# Check Git status
git status
```

The output of the command was:

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        data/
        results/

no changes added to commit (use "git add" and/or "git commit -a")
```

### 14: Create `.gitignore` and check status again

<!-- You can create the file with a command or in VS Code; include your git status command and its output. -->

```bash
# Create .gitignore file
touch .gitignore

# Add data/ and results/ directories to .gitignore
echo "data/" >> .gitignore
echo "results/" >> .gitignore

# Confirm they were added
cat .gitignore

# Check status of repo
git status
```

The output of the command was:

```
# Contents added to .gitignore
data/
results/

# Status of git repo
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore

no changes added to commit (use "git add" and/or "git commit -a")
```

*Answer*: Is the `.gitignore` working as intended? <br>
After checking the status of the git repo, it shows that only README.md has been modified and the previously shown `data/` and `results/` directories are no longer included. Rather, they are processed under the .gitignore file. Git will now ignore these files when you add and commit files to git. This is further confirmed by the .gitignore file being included under the untracked files only. The .gitignore file will still need to be added and committed to git. 

### 15: Stage and commit `.gitignore`, then README again

The `.gitignore` file was added using `git add` then commited using `git commit -m` followed by a descriptive message. The same steps were followed for the README file. You can confirm all changes were committed used `git status` and then look at changes using `git log`. 

## Part D: Exploring the FASTQ files with Unix data tools

### 16: Copy the FASTQ file

```bash
# Copy S63_R1.fastq.gz file to data/ and keep same name
cp /fs/ess/PAS3493/people/ndameron/garrigos/fastq/S63_R1.fastq.gz data/

# Confirm file was copied correctly
ls data/
```

The output of the command was:

```
# Confirmation file was properly copied
annot.gtf  S63_R1.fastq.gz
```

### 17: Check Git status again

```bash
# Check status of git repo
git status
```

The output of the command was:

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

*Answer*: Does the FASTQ file show up? Why/why not? <br>
The FASTQ file does not show up in the Git repo because it was added under the `data/` directory, which we previously told Git to ignore using .gitignore. 

### 18: Number of reads in the FASTQ file

```bash
# Count the number of reads in FASTQ file (remember each read starts with an @; there are 4 lines per read; need to use zcat to view compressed file)
zcat data/S63_R1.fastq.gz | grep -c "@"
```

The output of the command was:

```
# Read count using grep
500000
```

*Answer*: Number of reads: 500000 

You can confirm this is correct by counting the total lines in the file by piping the zcat code into `wc -l` to count total lines, then divide by four.

### 19: Reads with at least 10 consecutive `N`s

```bash
# Count reads with at least 10 consecutive `N`s (assume consecutive `N`s only occur in read line and not in quality score line)
zcat data/S63_R1.fastq.gz | grep -c "NNNNNNNNNN"
```

The output of the command was:

```
# Number of reads with 10 consecutive `N`s
32612
```

*Answer*: Number of reads with at least 10 consecutive `N`s: 32612

### 20: Stage and commit README again

Use `git add README.md` and `git commit`

## Bonus

### 21: Why do the counts from questions 8 and 9 differ?

<!-- Add any commands you ran and their output in code blocks, as above. -->

*Answer*:
Question 8 was asking how many lines existed in the GTF table after excluding the header lines. This was determined using the below command which counts all lines in the GTF file starting at line 5 (which excludes the four header lines).
```bash
# Total number of lines (excluding headers) for question 8
tail -n +5 data/annot.gtf | wc -l
```
```
Output:
408398
```
Question 9 was asking the same question, but using a different approach. Rather than using `tail` to exclude the first four lines, we used `grep -v` to select against lines that contain `#`, which occur at the beginning of each header. This, in theory, should only count the lines within the table. This was done with the code below. 
```bash
# Total number of lines (excluding headers) for question 9
grep -v "#" data/annot.gtf | wc -l
```
```
Output:
408397
```
This leads to a discrepency of one line between the two commands. Upon further investigation of the GTF, it was found that the GTF file has `###` as its last line (line 408402), which is not picked up when using tail or self count. This was determined using the below code.
```bash
grep -n "#" data/annot.gtf | cat
```
```
Output:
1:#gtf-version 2.2
2:#!genome-build TS_CPP_V2
3:#!genome-build-accession NCBI_Assembly:GCF_016801865.2
4:#!annotation-source NCBI RefSeq GCF_016801865.2-RS_2022_12
408402:###
```
The `###` at line `408402` was included when using the method described for question 8, providing a false answer.


### 22: Concepts/commands you don't (fully) understand

*Answer*:

I feel fairly confident on using the commands we have learned so far. I have even began using many of these commands to analyze metada from a metagenomic dataset that I am looking at potentially using for my research. Commands like `cut`, `sort`, and `uniq` have been extremely useful for creating frequency tables of this metadata, especially since the dataset contains over 1000 variables and over 10,000 samples. I still need to reference my notes when writing the commands as I still forget what some of the commands do, but overall, I feel more confident on using them. 

One concept that I am still unfamiliar with is the structure of the GTF file. Is it correct to state that the GTF file contains annotated genomic sequences from your metagenomic data with the last line being its annotation? How are these files created; are target genomic elements from your metagenomic data annotated with tools like HUMAnN3 to predict functional pathways? What can you do with these GTF files?


Github repo created on 2026-10-01 (https://github.com/20ndameron/PLNTPTH_5006_Ex3)
