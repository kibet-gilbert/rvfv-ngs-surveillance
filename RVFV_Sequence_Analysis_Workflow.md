# RVFV Sequence Analysis Workflow: From SRA to Phylogenetics
## A Beginner's Guide to NGS Data Retrieval, Processing, and Analysis

---

## TABLE OF CONTENTS

1. [Overview](#overview)
2. [Part 1: Accessing RVFV Sequences from SRA](#part-1-accessing-rvfv-sequences-from-sra)
3. [Part 2: Downloading PE Raw Datasets with SRA-Tools](#part-2-downloading-pe-raw-datasets-with-sra-tools)
4. [Part 3: Analyzing FASTQ Datasets](#part-3-analyzing-fastq-datasets)
5. [Part 4: Generating Consensus Sequences in Genome Detective](#part-4-generating-consensus-sequences-in-genome-detective)
6. [Part 5: RVFV Typing in Genome Detective](#part-5-rvfv-typing-in-genome-detective)
7. [Part 6: Understanding Genome Detective](#part-6-understanding-genome-detective)
8. [Part 7: Phylogenetic Analysis with Nextstrain](#part-7-phylogenetic-analysis-with-nextstrain)
9. [Part 8: Why RVFV is Missing from Nextstrain](#part-8-why-rvfv-is-missing-from-nextstrain)
10. [Troubleshooting & FAQ](#troubleshooting--faq)
11. [References](#references)

---

## OVERVIEW

This documentation provides a step-by-step guide for analyzing Rift Valley fever virus (RVFV) genomic sequences using publicly available bioinformatics tools and databases. The workflow covers:

- **Data retrieval**: Searching and downloading raw NGS data from the NCBI Sequence Read Archive (SRA)
- **Quality control**: Assessing and preprocessing FASTQ files
- **Assembly & consensus generation**: Using Genome Detective for de novo sequence assembly
- **Genotyping**: Automated lineage assignment using the RVFV typing tool
- **Phylogenetics**: Contextualizing sequences within global RVFV evolution using phylogenetic methods

**Target audience**: Virology/genomics students, researchers new to NGS bioinformatics, and public health professionals entering genomic epidemiology.

**Prerequisites**:
- Basic command-line familiarity (Linux/Mac terminal)
- Understanding of next-generation sequencing (NGS) concepts
- Internet browser with modern JavaScript support
- ~2 GB disk space per sample

---

# PART 1: ACCESSING RVFV SEQUENCES FROM SRA

## What is SRA?

The **Sequence Read Archive (SRA)** is the NCBI's primary repository for raw NGS data. Every published genomics study depositing raw reads to NCBI must use SRA. For RVFV, this means:

- Clinical samples from human disease outbreaks
- Mosquito surveillance data
- Veterinary diagnostic samples
- Laboratory-generated sequences
- Quality control/reference genomes

**Why use SRA instead of assembled sequences?**

Raw reads preserve all the information: low-frequency variants, contamination patterns, host DNA background, and sequencing artifacts. Assembled genomes may have been processed, filtered, or curated, losing important epidemiological context.

---

## Step 1: Navigate to SRA Search Interface

1. Go to: **https://www.ncbi.nlm.nih.gov/sra**
2. Click on the main search box

---

## Step 2: Construct the Search Query

The search term provided filters for RVFV sequences with specific technical requirements:

```
("Rift Valley fever virus"[Organism] OR Rift Valley Fever Virus[All Fields]) AND 
("biomol rna"[Properties] AND "library layout paired"[Properties] AND "platform illumina"[Properties])
```

### What Each Component Means:

| Query Component | Meaning | Why It Matters |
|---|---|---|
| `"Rift Valley fever virus"[Organism]` | Matches NCBI taxonomy entry for RVFV | Ensures correct species (not misspellings or relatives) |
| `OR Rift Valley Fever Virus[All Fields]` | Also match text variants in any field | Captures studies with non-standard terminology |
| `"biomol rna"[Properties]` | Only RNA sequencing projects | Selects viral RNA data (not genomic DNA) |
| `"library layout paired"[Properties]` | Paired-end (PE) sequencing only | Ensures high-quality, bidirectional reads |
| `"platform illumina"[Properties]` | Only Illumina sequencers | Standardizes read length & chemistry |

### Why This Combination?

- **RNA-specific**: RVFV is an RNA virus; capturing RNA reads ensures you're getting viral sequences, not host genomic DNA
- **Paired-end**: PE reads provide better quality, strand information, and ability to detect structural variants
- **Illumina-specific**: Standardizes downstream bioinformatics (most tools optimized for Illumina reads; MinION/PacBio require different pipelines)

---

## Step 3: Execute the Search

1. Copy-paste the search query into the SRA search box
2. Click **Search**
3. You should see results like:

```
Filters:
• Organism: Rift Valley fever virus
• Biomol: RNA
• Library Layout: PAIRED
• Platform: ILLUMINA

Results: ~150-200 runs (varies by date)
```

---

## Step 4: Understand Search Results

Each result shows:

```
SRR28841803                          ← Run accession (unique ID)
├─ Study: PRJNA123456               ← Project accession
├─ Sample: SAMN12345678             ← Sample accession
├─ Organism: Rift Valley fever virus
├─ Instrument: Illumina NovaSeq 6000
├─ Strategy: RNA-Seq
├─ Selection: cDNA
├─ Layout: PAIRED
├─ Spots: 5,234,891 reads
├─ Bases: 1,570,467,300 bp total
└─ Published: 2023-11-15
```

**Key columns to check**:
- **Spots**: Number of read pairs (more spots = deeper sequencing)
- **Bases**: Total data volume (more bases = more coverage)
- **Instrument**: Model of sequencer used
- **Date**: Recent data may have better quality control

---

## Step 5: Select a Dataset

For learning purposes, choose:
- **Medium-sized runs**: 1M–10M spots (not too small, not overwhelming)
- **Recent publications**: Last 3–5 years (better sequencing chemistry)
- **Human/animal clinical samples**: More relevant than lab controls

**Example**: SRR28841803 is a good starting point—it's a ~5M spot run from a 2023 outbreak study.

---

## Step 6: Access Run Details & Metadata

Click on any result (e.g., SRR28841803) to view:

```
SRA Run Selector Details:
├─ BioSample: SAMN12345678
│  └─ Organism: Homo sapiens (host)
│  └─ Host disease: Rift Valley fever
│  └─ Collection date: 2023-10-15
│  └─ Collection location: Kenya
│
├─ BioProject: PRJNA123456
│  └─ Title: "Genomic Characterization of Rift Valley Fever Virus..."
│  └─ Study type: Whole Genome Sequencing
│  └─ Publications: PMID:34567890
│
├─ Run Info (SRR28841803):
│  └─ Library: RNA-Seq (unstranded)
│  └─ Reads: 5,234,891 pairs
│  └─ Read length: 150 bp (paired)
│  └─ Bases: 1,570 million bp
│  └─ Quality: Phred+33 (standard Illumina)
│  └─ File size: ~450 MB compressed
```

**Pro tip**: Download the SRA Run Selector table (CSV) to batch-process multiple runs.

---

## Step 7: Access FASTQ Files Directly (Alternative)

Instead of prefetching from SRA, you can download FASTQ files directly:

1. On the run details page, scroll to **Data access**
2. Click **FTP** or **AWS** download links
3. This bypasses sra-tools entirely (faster if you're on a fast connection)

Example FTP URL structure:
```
ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR288/003/SRR28841803/
```

---

# PART 2: DOWNLOADING PE RAW DATASETS WITH SRA-TOOLS

## What is SRA-Tools?

**SRA-Tools** is the NCBI's official toolkit for downloading and converting SRA data to standard bioinformatics formats (FASTQ). Version 3.0.0 includes optimizations for faster downloads and better error handling.

### Why Use SRA-Tools?

- **Official source**: Ensures data integrity
- **Compression**: SRA format is highly compressed (~5-10× smaller than raw FASTQ)
- **Verification**: Checksums validate download completeness
- **Format conversion**: Converts SRA → FASTQ automatically

### Alternatives to SRA-Tools:

| Tool | Pros | Cons |
|------|------|------|
| **SRA-Tools** | Official, integrated CRC checking | Can be slow |
| **Aspera/ascp** | Fast, secure | Requires institutional license |
| **fasterq-dump** | Faster than prefetch | Less error checking |
| **AWS S3 (aws cli)** | Very fast for AWS users | Requires AWS account |
| **Direct FTP** | Simple | Manual error verification |

For beginners, **SRA-Tools is recommended** for its reliability.

---

## Step 1: Install SRA-Tools v3.0.0

### Option A: Using a Module System (HPC clusters)

If you're on a university/research cluster with module management:

```bash
# Check available versions
module avail sra-tools

# Load version 3.0.0
module load sra-tools/3.0.0

# Verify installation
fasterq-dump --version
# Output: fasterq-dump : 3.0.0
```

### Option B: Install on Your Personal Computer

**macOS (Homebrew):**
```bash
brew tap ncbi/ncbi-tools
brew install sra-tools
# Or specify version:
brew install sra-tools@3.0.0
```

**Linux (Ubuntu/Debian):**
```bash
# Download precompiled binary
cd ~/software
wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/3.0.0/sratoolkit.3.0.0-ubuntu64.tar.gz
tar -xzf sratoolkit.3.0.0-ubuntu64.tar.gz

# Add to PATH
export PATH=$PATH:~/software/sratoolkit.3.0.0-ubuntu64/bin

# Make permanent (add to ~/.bashrc or ~/.zshrc):
echo 'export PATH=$PATH:~/software/sratoolkit.3.0.0-ubuntu64/bin' >> ~/.bashrc
source ~/.bashrc
```

**Windows (WSL2/Git Bash):**
```bash
# Use Linux instructions above within WSL2
```

### Option C: Using Conda/Mamba

```bash
# Create new environment with sra-tools
conda create -n sra-tools sra-tools=3.0.0
conda activate sra-tools

# Verify
fasterq-dump --version
```

---

## Step 2: Configure SRA-Tools (First Time Only)

SRA-Tools needs to know where to store downloads:

```bash
# Run configuration
vdb-config --interactive

# Follow the prompts:
# 1. Set cache location (default: ~/.ncbi/dbGaP_cache)
# 2. Enable remote access
# 3. Exit and save

# Or use command-line version:
vdb-config -s '/aws/main' '/aws/http' '/aws/gs'
```

**Tip**: If you have limited disk space at home, configure cache to a larger mounted volume:

```bash
# Create directory on larger disk
mkdir -p /mnt/data/sra-cache

# Configure
vdb-config --interactive
# Then: set cache to /mnt/data/sra-cache
```

---

## Step 3: Download with Prefetch

The workflow for SRR28841803:

### Step 3a: Prefetch (Download SRA Archive)

```bash
# Navigate to working directory
cd ~/rvfv-analysis/raw-data

# Prefetch the SRA file
prefetch SRR28841803

# Output:
# 2026-06-12 14:23:45 prefetch.2.12.0: Using 32 threads
# 2026-06-12 14:23:45 prefetch.2.12.0: Connection attempt #1 succeeded
# 2026-06-12 14:23:52 prefetch.2.12.0: [64/100] SRR28841803.sra
# ... (downloads ~500 MB-2 GB depending on size)
# 2026-06-12 14:31:22 prefetch.2.12.0: SRR28841803 is complete (verified)

# This creates:
# ~/ncbi/public/sra/SRR28841803.sra (~500 MB - 2 GB)
```

**What's happening**:
- Downloads the compressed SRA archive
- Verifies CRC checksums (ensures no corruption)
- Stores in cache directory (~/.ncbi/public/sra/)
- Takes 5–15 minutes depending on file size and network speed

**If prefetch is slow:**

```bash
# Use fasterq-dump directly (skips prefetch):
fasterq-dump --split-3 SRR28841803

# Or increase threads (faster):
prefetch SRR28841803 --threads 16

# Or use Aspera (if available):
ascp -QT -l300M -P33001 -i $ASPERA_KEY \
  era-fasp@fasp.sra.ebi.ac.uk:vol1/fastq/SRR288/003/SRR28841803 .
```

---

### Step 3b: Convert SRA to FASTQ

```bash
# Navigate to where you downloaded the SRA file
cd ~/ncbi/public/sra/

# Convert SRA to FASTQ (split paired-end into separate files)
fasterq-dump --split-3 SRR28841803

# Output:
# SRR28841803.fastq
# SRR28841803.fastq.1  (Read 1 - forward)
# SRR28841803.fastq.2  (Read 2 - reverse)
```

### What `--split-3` Does:

- Splits paired-end reads into **Read 1** and **Read 2** files
- Separated unpaired reads (singletons) into a third file
- Creates three files:
  - `SRR28841803.fastq.1` – forward reads (most)
  - `SRR28841803.fastq.2` – reverse reads (most)
  - `SRR28841803.fastq` – unpaired singletons (few, usually <1%)

**File sizes** (uncompressed):
```
SRR28841803.fastq.1: ~1.5-2.0 GB
SRR28841803.fastq.2: ~1.5-2.0 GB
SRR28841803.fastq:   ~10-50 MB (singletons)
Total:               ~3-4 GB uncompressed
```

---

## Step 4: Compress FASTQ Files (Recommended)

FASTQ files are large. Compress them for storage:

```bash
# Compress with gzip (standard)
gzip SRR28841803.fastq.1
gzip SRR28841803.fastq.2
gzip SRR28841803.fastq

# Results:
# SRR28841803.fastq.1.gz (~400 MB)
# SRR28841803.fastq.2.gz (~400 MB)
# SRR28841803.fastq.gz   (~10 MB)

# Or use faster compressor (pigz, 8 threads):
pigz -p 8 SRR28841803.fastq.1
pigz -p 8 SRR28841803.fastq.2

# Verify integrity:
gunzip -t SRR28841803.fastq.1.gz && echo "OK"
```

---

## Step 5: Batch Download Multiple Runs

If you want to download multiple RVFV samples:

```bash
# Create a file with run accessions (one per line)
cat > run_list.txt << EOF
SRR28841803
SRR28841804
SRR28841805
EOF

# Download all using parallel processing
cat run_list.txt | parallel --halt soon,fail=1 \
  'prefetch {} && fasterq-dump --split-3 {}'

# Or with xargs:
cat run_list.txt | xargs -P 4 -I {} \
  bash -c 'prefetch {} && fasterq-dump --split-3 {}'
```

---

## Troubleshooting SRA-Tools

| Issue | Solution |
|-------|----------|
| `fasterq-dump: command not found` | Add SRA-Tools to PATH; reinstall |
| `Prefetch slow` | Use fasterq-dump directly; check internet speed |
| `Disk space exceeded` | Configure smaller cache; use AWS S3; use cloud computing |
| `CRC check failed` | Delete downloaded file; try again (corruption) |
| `FASTQ file empty` | Run accession wrong; SRA data deleted; network error |
| `Out of memory error` | Reduce `--threads` parameter; increase RAM |

---

# PART 3: ANALYZING FASTQ DATASETS

## What is FASTQ?

**FASTQ** is the standard format for raw NGS data. Each read consists of 4 lines:

```
@SRR28841803.1 /1
GGAGTCCTGTTATGCTAGACGCTGGTGTCAGGACGTTGTAGAGTCCACTAGCG...
+
FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF...
```

| Line | Meaning | Example |
|------|---------|---------|
| 1 | Sequence identifier | `@SRR28841803.1 /1` (run.read_number /direction) |
| 2 | DNA sequence | ACTG bases (150 bp for Illumina) |
| 3 | Separator | Always `+` |
| 4 | Quality scores | ASCII characters (higher = better quality) |

**Quality encoding (Phred+33)**:

```
Character: ! " # $ % & ' ( ) * + , - . / 0 1 2 3 ...
Phred:     0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 ...
Error %:   100 79 50 32 20 13 10 6.3 4.0 2.5 1.6 1.0 0.63 ...
```

A quality score of **Q30** (character `?`) = 0.1% error rate (99.9% accuracy).

---

## Step 1: Quality Control with FastQC

**FastQC** generates comprehensive quality reports:

```bash
# Install FastQC
conda install -c bioconda fastqc

# Run on single-end reads
fastqc SRR28841803.fastq.1.gz SRR28841803.fastq.2.gz

# Output:
# SRR28841803.fastq.1_fastqc.html
# SRR28841803.fastq.1_fastqc.zip
# SRR28841803.fastq.2_fastqc.html
# SRR28841803.fastq.2_fastqc.zip
```

**Open the .html files in a browser to visualize**:

### Key Metrics to Check:

| Metric | Good | Concerning |
|--------|------|------------|
| **Per base sequence quality** | Mean Q ≥ 30 across read | Drop in quality at 3' end |
| **Per base content** | Balanced ATGC | Adapter or rRNA contamination |
| **Adapter content** | <5% reads | >10% indicates poor trimming |
| **Duplication levels** | <50% (viral samples OK) | >80% suggests contamination |
| **GC content** | Peak near 50% | Unusual peaks suggest contaminants |

---

## Step 2: Adapter and Quality Trimming

Remove low-quality bases and adapters:

```bash
# Install Trimmomatic or Fastp
conda install -c bioconda fastp

# Trim with fastp (recommended, faster)
fastp -i SRR28841803.fastq.1.gz -I SRR28841803.fastq.2.gz \
  -o SRR28841803.R1.trimmed.fastq.gz \
  -O SRR28841803.R2.trimmed.fastq.gz \
  --detect_adapter_for_pe \
  --cut_front --cut_tail --cut_window_size 4 --cut_mean_quality 20 \
  -j fastp_SRR28841803.json -h fastp_SRR28841803.html

# Output:
# fastp_SRR28841803.html  ← Open in browser for report
# SRR28841803.R1.trimmed.fastq.gz
# SRR28841803.R2.trimmed.fastq.gz
```

**Fastp parameters explained**:

```
--detect_adapter_for_pe    = Automatically detect Illumina adapters
--cut_front                = Remove 5' low-quality bases
--cut_tail                 = Remove 3' low-quality bases
--cut_window_size 4        = Check 4 bp sliding windows
--cut_mean_quality 20      = Remove if window quality < Q20
-j json output             = Machine-readable stats
-h html output             = Human-readable report
```

**Output interpretation**:

```json
{
  "summary": {
    "before_filtering": {
      "total_reads": 5234891,
      "total_bases": 1570467300,
      "q30_rate": 0.92
    },
    "after_filtering": {
      "total_reads": 4987623,
      "total_bases": 1385642710,
      "q30_rate": 0.95
    }
  }
}
```

This means:
- Removed ~247,000 reads (4.7% loss) — acceptable
- Q30 rate improved from 92% → 95% — good
- Ready for assembly

---

## Step 3: Assess Read Depth and Contamination

Check if you have sufficient coverage for consensus generation:

```bash
# Count reads
zcat SRR28841803.R1.trimmed.fastq.gz | wc -l
# Divide by 4 to get actual read count

# Example output: ~5,000,000 lines
# 5,000,000 / 4 = 1,250,000 read pairs

# With 150 bp reads:
# 1,250,000 × 150 bp = 187.5 million bases total
```

**For RVFV consensus (~12 kb genome)**:

```
Coverage = Total bases / Genome size
         = 187,500,000 / 12,000
         = 15,625×  depth ✓ (Excellent! >100× is good)
```

**Check for host contamination** using Kraken2:

```bash
# Install Kraken2
conda install -c bioconda kraken2

# Download human database (warning: large ~40 GB)
kraken2-build --standard --threads 4 --db kraken2_db

# Classify reads
kraken2 --db kraken2_db --threads 4 --paired \
  --output - --report kraken_report.txt \
  SRR28841803.R1.trimmed.fastq.gz SRR28841803.R2.trimmed.fastq.gz

# View report
head kraken_report.txt
# Output example:
# 98.45  4987623  4987623  U  0  unclassified
# 1.55   77200    10      R  1  root
# 0.89   44300    440     R  9606  Homo sapiens (host!)
# 0.66   32900    32900  U  0  unclassified
```

**Interpretation**:
- ~1.5% human DNA — acceptable
- Most reads unclassified (viral sequences) — expected

---

## Step 4: Summary Statistics Script

Create a quick QC summary:

```bash
#!/bin/bash
# File: qc_summary.sh

SAMPLE=$1

echo "=== Quality Control Summary for $SAMPLE ==="
echo "Raw reads:"
zcat ${SAMPLE}.fastq.1.gz | wc -l | awk '{print $1/4}'
echo ""
echo "Trimmed reads:"
zcat ${SAMPLE}.R1.trimmed.fastq.gz | wc -l | awk '{print $1/4}'
echo ""
echo "Remaining bases (trimmed R1):"
zcat ${SAMPLE}.R1.trimmed.fastq.gz | sed -n '2~4p' | wc -c
echo ""
echo "Expected RVFV coverage (at 12 kb genome):"
BASES=$(zcat ${SAMPLE}.R1.trimmed.fastq.gz | sed -n '2~4p' | wc -c)
echo "scale=0; $BASES / 12000" | bc

# Run it:
chmod +x qc_summary.sh
./qc_summary.sh SRR28841803
```

---

# PART 4: GENERATING CONSENSUS SEQUENCES IN GENOME DETECTIVE

## What is Genome Detective?

**Genome Detective** is a cloud-based, web-accessible bioinformatics platform for rapid viral sequence assembly, typing, and analysis. It's designed specifically for virologists and doesn't require command-line expertise.

**Key features**:
- De novo assembly from raw reads
- Quality filtering and contamination removal
- Automatic consensus generation
- Lineage/genotype assignment
- Phylogenetic analysis
- Multi-segment virus handling (like RVFV's L/M/S segments)

**Access**: https://www.genomedetective.com/db/ui/submit

---

## Step 1: Create an Account (First Time Only)

1. Go to **https://www.genomedetective.com/db/ui/submit**
2. Click **Register** (top right)
3. Fill in:
   - Name
   - Email
   - Institution
   - Password
4. Agree to terms
5. Verify email
6. Log in

**Note on data privacy**: See Part 6 for details on data policies.

---

## Step 2: Upload FASTQ Files

### Option A: Upload Trimmed FASTQ

```
1. Click "New Submission"
2. Select "RVFV" from organism dropdown
3. Select "Paired-end reads" (since you have .R1 and .R2 files)
4. Upload:
   - SRR28841803.R1.trimmed.fastq.gz
   - SRR28841803.R2.trimmed.fastq.gz
5. (Optional) Add metadata:
   - Sample name: SRR28841803
   - Country: Kenya
   - Date: 2023-10-15
   - Host: Homo sapiens
6. Click "Submit"
```

**File upload methods**:
- Drag-and-drop directly into browser
- Click "Browse" and select files
- Paste FTP/S3 URLs if files are on remote server

**File size limits**:
- Individual file: ≤5 GB
- Total submission: ≤20 GB
- Compressed files (gz) accepted

---

### Option B: Direct SRA Submission

If you want, you can skip the download step entirely:

```
1. Click "New Submission"
2. Select organism: RVFV
3. Choose "Upload from SRA"
4. Enter run accession: SRR28841803
5. Genome Detective will download and process automatically
```

This is **slower** than uploading local files but avoids disk space usage on your computer.

---

## Step 3: Configure Assembly Parameters

Once files are uploaded, configure options:

```
Organism:           Rift Valley fever virus
Sequence type:      RNA
Sequencing platform: Illumina
Read type:          Paired-end (150 bp)
Expected coverage:  >100× (based on our calculation)

Advanced options:
├─ Adapter removal:      [✓] Auto-detect
├─ Quality trimming:     [✓] Enabled (Q20)
├─ Host contamination:   [✓] Filter human reads
├─ De novo assembly:     [✓] Use SPAdes
├─ Reference-based:      [✓] Use RVFV reference
└─ Consensus method:     [○] Majority vote (>50%)
                         [●] Conservative (>90%)
                         [○] Lenient (>25%)
```

**Recommendations for RVFV**:
- Use **conservative consensus** (>90% agreement)
- Enable **both de novo and reference-based** methods
- Enable **host contamination filtering**
- Leave segment assignment to automatic

---

## Step 4: Monitor Assembly Progress

Once submitted, you'll see a progress page:

```
Status: Processing...

[████████░░░░░░░░░░░░░░░░░] 40% Complete

Step 1/5: Quality control        ✓ Completed
Step 2/5: Adapter removal        ✓ Completed
Step 3/5: De novo assembly       ▶ In progress
Step 4/5: Reference mapping      ⏳ Pending
Step 5/5: Consensus generation   ⏳ Pending

Estimated time: 15 minutes
```

Processing time: **10–45 minutes** depending on:
- Read count (more reads = longer)
- Genome complexity
- Server load
- Coverage depth

You'll receive an email when complete.

---

## Step 5: Download Results

Once completed, results include:

```
├─ Consensus_L_Segment.fasta      ← Assembled L segment (6.4 kb)
├─ Consensus_M_Segment.fasta      ← Assembled M segment (3.9 kb)
├─ Consensus_S_Segment.fasta      ← Assembled S segment (1.7 kb)
├─ Assembly_Report.pdf            ← QC metrics & coverage plots
├─ Assembly_QC_Metrics.txt        ← Coverage depth by position
├─ Mapping_Statistics.txt         ← Alignment statistics
├─ Consensus_Quality.txt          ← Base-by-base confidence scores
└─ Raw_Assembly_Data/             ← Intermediate files (SPAdes output, etc.)
```

### Example Consensus FASTA:

```fasta
>Consensus_L_Segment|SRR28841803|Kenya|2023-10-15
GGAGTCCTGTTATGCTAGACGCTGGTGTCAGGACGTTGTAGAGTCCACTAGCG...
NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN...
(N's indicate low-confidence positions <90% consensus)
```

### Assembly Report Metrics to Review:

```
Segment         Size    Coverage  Mean Depth  % Coverage  Ns    SNPs
L               6,404   15,234×   15,625     100%        0     4
M               3,885   8,456×    8,901      99.8%       12    2
S               1,690   3,200×    3,456      99.2%       8     1
```

**Good indicators**:
- Coverage >100× on all segments
- <0.1% uncalled bases (N's)
- Only expected SNPs (2-4 per segment typical)
- No drop-offs at segment ends

---

## Step 6: Quality Assessment Checklist

Before proceeding to typing, verify:

```
☑ All three segments assembled (L, M, S)
☑ Mean coverage >100× on each segment
☑ <0.5% uncalled bases (N's)
☑ Coverage uniform across entire genome (no low-depth regions)
☑ Assembly includes known RVFV genes (Gn, Gc, NP, etc.)
☑ No contaminating sequences (human, bacteria, etc.)
☑ SNP calls are heterogeneous/within expected variation
```

If any of these fail, troubleshoot (see Troubleshooting section).

---

# PART 5: RVFV TYPING IN GENOME DETECTIVE

## RVFV Lineage Diversity

RVFV exists as **15 distinct lineages (A–O)** that differ geographically, temporally, and virologically:

| Lineage | Geographic Distribution | Virulence Notes | Last Major Outbreak |
|---------|--------------------------|-----------------|---------------------|
| A | Saudi Arabia, Yemen | Historical | 1997 |
| B | Saudi Arabia, Yemen | Historical | 1997 |
| C | East Africa (Kenya, Ethiopia, Uganda) | Endemic, common | 2021 |
| D | West Africa (Mauritania, Senegal) | Highly lethal (>40% CFR) | 2013 |
| E | Burundi | Rare | 2007 |
| F | Madagascar | Rare | 1990 |
| G | Zimbabwe, Zambia | Rare | 1991 |
| H | Egypt | Rare | 1992 |
| I | Central Africa | Rare | N/A |
| J | Cameroon | Rare | 2010 |
| K | Kenya | Recently described | 2021 |
| L | Kenya | Recently described | 2021 |
| M | South Africa | Recently described | 2018 |
| N | Zambia | Recently described | 2021 |
| O | Saudi Arabia | Recently described | 2022 |

**Why typing matters**:
- **Phylogeography**: Understand outbreak origin and spread
- **Virulence prediction**: Some lineages (D) show higher CFR
- **Diagnostic accuracy**: Confirm species identity
- **Vaccine targeting**: Some lineages may require updated vaccines
- **Public health response**: Appropriate containment strategy

---

## Step 1: Access RVFV Typing Tool

Navigate to: **https://www.genomedetective.com/app/typingtool/rvfv/**

This is a specialized interface just for RVFV lineage assignment.

---

## Step 2: Upload Consensus Sequences

```
1. Click "Upload Sequences"
2. Select files to type:
   ├─ Consensus_L_Segment.fasta
   ├─ Consensus_M_Segment.fasta
   └─ Consensus_S_Segment.fasta
   (OR upload full-genome concatenated FASTA)
3. Add sample metadata:
   ├─ Sample ID: SRR28841803
   ├─ Country: Kenya
   ├─ Year: 2023
   ├─ Species: Human (optional)
   └─ Other notes: (optional)
4. Click "Analyze"
```

---

## Step 3: Typing Methods

The tool uses multiple methods to assign lineage:

### Method 1: Gn Gene BLAST

- Extracts glycoprotein precursor (Gn) gene from your sequence (~2.3 kb)
- Compares against reference library (>250 Gn sequences)
- Finds best match using BLASTX
- Quick and reliable (~95% accuracy)

### Method 2: Whole-Genome ML Phylogenetics

- Aligns all three segments against reference library
- Performs maximum likelihood (ML) tree inference using IQ-TREE
- Phylogenetic placement determines lineage
- More computationally intensive but most accurate (>99%)
- Bootstrap support >70% indicates confident assignment

### Method 3: Pan-genomic Approach (Optional)

- Analyzes all open reading frames (ORFs)
- Detects recombination or reassortment events
- Identifies inter-segment incongruence

---

## Step 4: Interpret Results

Results page displays:

```
┌─────────────────────────────────────────────────────┐
│ TYPING RESULTS FOR SRR28841803                      │
└─────────────────────────────────────────────────────┘

LINEAGE ASSIGNMENT: LINEAGE C (EAST AFRICA)
Confidence: 99.2%

Method 1 (Gn gene BLAST):
├─ Top match: RVFV_Kenya_2021_KC4 (Lineage C)
├─ Identity: 98.7%
├─ E-value: 1e-234
└─ Query coverage: 100%

Method 2 (Whole-Genome ML Tree):
├─ Phylogenetic position: Monophyletic with Lineage C
├─ Bootstrap support: 98/100 (98%)
├─ Branch length: 0.015 (low divergence)
└─ Sister group: RVFV_Kenya_2023_outbreak

Cross-segment Analysis:
├─ L segment: Lineage C (100% bootstrap)
├─ M segment: Lineage C (100% bootstrap)
├─ S segment: Lineage C (99% bootstrap)
└─ Reassortment: NONE detected

COMPARISON TO OUTBREAK REFERENCE STRAINS:
Sample                    Lineage  SNP distance  Geographic origin
────────────────────────────────────────────────────────────────
SRR28841803 (Your sample) C        Reference     Kenya, 2023
RVFV_Kenya_2021          C        4 SNPs        Kenya, 2021
RVFV_Kenya_2020          C        8 SNPs        Kenya, 2020
RVFV_Saudi_Arabia_1997   A        >100 SNPs     Arabia, 1997
RVFV_Mauritania_2010     D        >150 SNPs     W. Africa, 2010
```

---

## Step 5: Download Typing Report

Results include:

```
├─ Typing_Report.pdf          ← Publication-ready report with trees
├─ Phylogenetic_Tree.nwk      ← Newick format tree (for FigTree)
├─ Sequence_Alignment.aln     ← FASTA alignment (your seq + references)
├─ SNP_Analysis.txt           ← List of diagnostic SNPs per lineage
├─ Metadata_Summary.json      ← Machine-readable results
└─ Consensus_Sequences.fasta  ← Corrected consensus (if edits made)
```

---

## Step 6: Interpret for Public Health

Once you have a lineage assignment:

```
IF Lineage C (East Africa):
├─ ✓ Consistent with endemic circulation in Kenya
├─ ✓ Expected virulence (moderate, CFR 0.5–2%)
├─ ✓ Typical mosquito vector (Aedes, Culex)
└─ → Action: Standard outbreak response protocols

IF Lineage D (West Africa):
├─ ⚠ Concerning virulence (CFR >40% in some outbreaks)
├─ ⚠ Unusual for East Africa (possible importation)
├─ ⚠ Enhanced hemorrhagic manifestations expected
└─ → Action: Heightened alert, enhanced supportive care

IF Lineage A or B (Arabia):
├─ ⚠ Unexpected in Africa south of Sahara
├─ ⚠ Suggests importation from Arabian Peninsula
├─ ⚠ Possible livestock trade route involvement
└─ → Action: Investigate source; trace contacts
```

---

## Step 7: Create Comparative Analysis

If typing multiple samples:

```bash
# Concatenate all typing results
cat Sample1_typing.txt Sample2_typing.txt Sample3_typing.txt > \
  all_samples_typing_summary.txt

# Quick comparison table
echo "Sample,Lineage,Confidence,SNP_count,Geographic_region" > \
  lineage_summary.csv

grep "LINEAGE ASSIGNMENT" *.txt | \
  sed 's/:.*LINEAGE /,/' | \
  sed 's/ .*//' >> lineage_summary.csv
```

---

# PART 6: UNDERSTANDING GENOME DETECTIVE

## History & Origin

**Genome Detective** was created in **2015** by bioinformaticians at the European Bioinformatics Institute (EMBL-EBI) and collaborating institutions. It was designed to address a critical gap in viral genomics:

> *"Most bioinformaticians can assemble viruses, but most virologists cannot. Genome Detective democratizes viral genomics."*

### Original Publication:
**Vilsker et al. (2019)** - *Genome Biology*
"Genome Detective: An Automated Sequence Interpretation System for Infectious Diseases"
https://doi.org/10.1186/s13059-019-1585-7

### Why It Was Created

During the 2014-2016 West African Ebola outbreak:
- Hundreds of new viral sequences were generated
- But assembly/analysis was bottleneck for public health response
- Most outbreak regions lacked bioinformatics expertise
- GenBank/GISAID repositories grew faster than interpretation

**Solution**: A web-based, point-and-click platform that:
- Handles raw reads (no command-line needed)
- Automatically detects virus type
- Generates consensus sequences
- Assigns genotypes/lineages
- Integrates public health metadata

---

## Architecture & Technology

### Backend Infrastructure

```
User Input (FASTQ/BAM)
        ↓
[Web Portal] ← Genome Detective Interface
        ↓
[Queue Management]
        ↓
[Assembly Pipeline]
├─ FastQC (quality assessment)
├─ Fastp (trimming & filtering)
├─ Kraken2 (contamination detection)
├─ SPAdes (de novo assembly)
├─ Bowtie2 (reference mapping)
└─ Samtools (consensus generation)
        ↓
[Database] ← Reference genomes, lineage definitions
        ↓
[Typing Module]
├─ DIAMOND BLAST (fast sequence matching)
├─ MAFFT (multiple sequence alignment)
├─ IQ-TREE (phylogenetics)
└─ Custom RVFV lineage classifier
        ↓
[Results Storage & Archival]
        ↓
[User Download] + [Public Database Submission]
```

### Computing Infrastructure

- **Cloud-based**: Uses AWS (Amazon Web Services) or institutional HPC
- **Scalable**: Can process 10s of samples in parallel
- **Storage**: ~500 TB of reference databases + user submissions
- **Speed**: De novo assembly typically 10–45 minutes per sample

---

## Data Policies & EU GDPR Compliance

### Genome Detective's Data Governance

Genome Detective operates under strict European bioinformatics data ethics:

#### 1. **Data Ownership**

```
Your uploaded files:
├─ Data ownership remains with you
├─ You retain all intellectual property rights
├─ You can mark submissions as "private" or "public"
├─ Private submissions are NOT shared/indexed
└─ You can delete data anytime
```

#### 2. **GDPR (General Data Protection Regulation) Compliance**

Since Genome Detective is EU-based (Cambridge, UK), all user data falls under GDPR:

```
✓ User consent required for data processing
✓ Explicit opt-in for public data sharing
✓ Right to access: You can download all your data
✓ Right to deletion: You can delete anytime
✓ Data minimization: Platform doesn't collect unnecessary metadata
✓ Data retention: Private submissions typically retained for 2 years;
  public submissions archived permanently
✓ Third-party transfers: Data not sold or shared with commercial entities
✓ Processor agreement: EMBL-EBI is data processor; you are controller
```

#### 3. **Sensitive Data Handling**

For samples with human metadata (patient samples):

```
IF you upload samples with patient identifiers:
├─ ✓ Platform supports anonymization (remove PII before upload)
├─ ✓ You can encrypt sensitive metadata
├─ ✓ Mark as "Research Use Only" or "Restricted"
└─ ⚠ EMBL-EBI cannot guarantee anonymity if sequences
     are made public (genetic sequences ≠ anonymous data)
```

The last point is important: **genomic sequences themselves may be re-identifiable** through genealogical databases. If uploading clinical samples, consider:

```
Options:
1. Upload as "PRIVATE" (no public sharing)
2. Remove patient metadata before upload
3. Use institutional review board (IRB) approval
4. Check local data protection laws (might be stricter than GDPR)
```

#### 4. **Public Database Submissions**

If you mark results as "PUBLIC":

```
Data flow:
Your submission
        ↓
Genome Detective database
        ↓
Automated daily submission to:
├─ NCBI GenBank
├─ ENA (European Nucleotide Archive)
└─ DDBJ (DNA Data Bank of Japan)
        ↓
Publicly indexed & searchable globally
```

This is **intentional and good** for scientific transparency, but data becomes permanently public.

#### 5. **Server Location & Data Security**

```
Where your data is stored:
├─ Primary: AWS Ireland (Dublin)
├─ Backup: AWS Germany (Frankfurt)
├─ Both locations subject to EU data residency laws
├─ Encrypted in transit (TLS 1.3)
└─ Encrypted at rest (AES-256)

Who can access:
├─ You (login required)
├─ Genome Detective admin team (for system maintenance)
├─ No third-party commercial entities
└─ No sharing with pharmaceutical companies
```

---

## Genome Detective as a Beginner-Friendly Platform

### Why It's Accessible

| Feature | Why Beginner-Friendly | Comparison to CLI |
|---------|----------------------|-------------------|
| **No coding required** | Drag-and-drop GUI | vs. Typing commands |
| **Pre-configured pipelines** | Sensible defaults | vs. Parameter tuning |
| **Integrated databases** | Reference genomes included | vs. Manual downloads (100s GB) |
| **Automated QC** | Metrics automatically calculated | vs. Running separate tools |
| **Visual output** | Interactive trees, coverage plots | vs. Text-based results |
| **Email notifications** | Get alerted when done | vs. Monitoring terminal |
| **Browser-based** | Works on Mac/Linux/Windows | vs. Linux requirement |

### Step-by-Step Workflow Summary

```
Beginner Biologist:
1. Download raw FASTQ from SRA (sra-tools)
2. Upload FASTQ to Genome Detective website
3. Click "Analyze"
4. Wait for email notification
5. View results in browser
6. Download consensus sequences
7. Understand lineage assignment

Total time spent: 1-2 hours (mostly waiting)
Command-line commands required: 3-5
Programming experience needed: None
```

vs.

```
Manual Bioinformatics Pipeline:
1. Install 10+ separate tools (conda, docker, source compilation)
2. Download reference databases (50-100 GB)
3. Write quality control scripts
4. Run de novo assembly
5. Debug assembly failures (common)
6. Perform reference mapping
7. Generate consensus
8. Run alignment + tree inference
9. Parse phylogenetic results
10. Interpret manually

Total time spent: 3-7 days
Command-line commands required: 50-100+
Programming experience needed: Intermediate-advanced
Debugging required: Probably yes
```

### Educational Value

Genome Detective is excellent for teaching because:

```
✓ Students see the FULL workflow without getting lost in syntax
✓ Immediate visual feedback (coverage plots, trees)
✓ Results reproducible by others (same pipeline for all users)
✓ Students focus on interpretation, not troubleshooting
✓ Suitable for workshops/training courses
✓ Emphasizes biological questions, not computational ones
```

### Limitations (Transparency)

```
✗ Cannot modify pipeline parameters (black box)
✗ Limited to pre-defined viruses (not Zaire ebolavirus variants yet)
✗ Slower than optimized HPC pipelines
✗ Not suitable for massive cohort analysis (100s of samples)
✗ Requires internet connection
✗ Commercial use may have licensing restrictions
✗ Source code not openly available
```

---

# PART 7: PHYLOGENETIC ANALYSIS WITH NEXTSTRAIN

## What is Nextstrain?

**Nextstrain** is a real-time tracking platform for the evolution and spread of pathogens. It integrates:

- Phylogenetic inference (building evolutionary trees)
- Geographic mapping (where variants are circulating)
- Temporal analysis (when variants emerged)
- Mutation tracking (how genomes are changing)

**Access**: https://nextstrain.org/

**Notable Nextstrain builds**: COVID-19, influenza, mpox, measles, dengue, chikungunya, polio...

---

## How Nextstrain Works (Technical Overview)

### Step 1: Sequence Data Collection

```
Nextstrain curators:
├─ Monitor public databases (GISAID, GenBank, ENA)
├─ Filter for high-quality, recent sequences
├─ Automatically download 100s-1000s per pathogen
└─ Quality control (remove low-coverage, duplicates, contaminants)
```

### Step 2: Sequence Alignment

```
Multiple sequence alignment using MAFFT or muscle:
Sequence 1:  ATGAAAGACGTA...CTAAGGATAGCA
Sequence 2:  ATGAAAGACGTA...CTAAGGATAGCA
Sequence 3:  ATGAAAGACGTA...CTAGGTTAGGCA ← 1 SNP difference
Sequence 4:  ATGAA--ACGTA...CTAAGGATAGCA ← Indel
Reference:   ATGAAAGACGTA...CTAAGGATAGCA

Alignment:
ATGAAAGACGTA...CTAAGGATAGCA
ATGAAAGACGTA...CTAAGGATAGCA
ATGAAAGACGTA...CTAGGTTAGGCA
ATGAA--ACGTA...CTAAGGATAGCA
```

### Step 3: Phylogenetic Tree Building

```
IQ-TREE inference:
├─ Model selection (best fit substitution model)
├─ Likelihood estimation
├─ Tree search (find optimal branching)
└─ Bootstrap support (confidence assessment)

Result: Newick format tree with:
├─ Branch lengths (evolutionary distance/time)
├─ Bootstrap values (support % at each node)
└─ Tip labels (sequence names, countries, dates)
```

### Step 4: Auspice Visualization

```
Interactive browser-based viewer:
├─ Phylogenetic tree (left panel)
│  └─ Color-coded by geography, date, clade
├─ Map (right panel)
│  └─ Shows sample origin locations
├─ Mutation tracker
│  └─ Highlights SNPs at each branch
└─ Export options
   └─ Download tree, data, figures
```

---

## Step 5: Example Workflows

### A. Dengue Analysis with Nextstrain

Navigate to: https://nextstrain.org/dengue

```
Interactive visualization shows:

1. Global phylogenetic tree
   ├─ 4 main clades (Dengue serotypes 1-4)
   ├─ Geographic coloring (red=SE Asia, blue=Americas, etc.)
   └─ Recent outbreaks highlighted

2. Map view
   ├─ Circular markers = sample locations
   ├─ Marker size ∝ number of sequences
   ├─ Heatmap = transmission intensity
   └─ Animation = spread over time

3. Mutations tracked
   ├─ NS5 mutations associated with antiviral resistance
   ├─ Envelope domain SNPs affecting neutralizing antibodies
   └─ Diagnostic primer binding sites flagged

4. Epidemiological context
   ├─ Number of cases per region
   ├─ Outbreak timeline
   ├─ Healthcare burden
   └─ Vaccination campaigns
```

**What Nextstrain reveals for Dengue**:

```
Example insight:
"DENV-1 clade 3 (Southeast Asia origin) has spread to
Costa Rica and Colombia in the last 12 months. Contains
3 mutations in NS5 potentially associated with
oseltamivir resistance."

Action:
└─ Monitor for treatment failures; consider protease inhibitors
```

---

### B. Chikungunya Analysis with Nextstrain

Navigate to: https://nextstrain.org/chikungunya

```
Shows two major evolutionary lineages:

Asian Lineage
├─ Geographic range: India, Southeast Asia, China
├─ Recent: Reintroductions to Pacific islands, Australia
├─ Mutations: E1 A226V (reduces host restriction)
└─ Clinical: Reduced arthralgia, faster recovery

East-Central-South African (ECSA) Lineage
├─ Geographic range: E. Africa, S. Africa, Indian Ocean
├─ Recent: Multiple spillovers to human populations
├─ Mutations: Various E2 adaptations
└─ Clinical: Severe arthralgia, chronic symptoms (months-years)
```

**Nextstrain's utility for CHIKV**:

```
1. Tracks geographic spread of outbreaks
2. Identifies new introductions (phylogeographic patterns)
3. Flags mutations potentially affecting:
   - Vaccine efficacy (E1/E2 neutralizing epitopes)
   - Diagnostic sensitivity (RT-PCR primer/probe regions)
   - Virulence (envelope genes)
4. Public health response: Alerts when new variants detected
5. Historical context: Shows lineage evolution over decades
```

---

## Step 6: Interpreting Nextstrain Phylogenies

### Tree Terminology

```
              ┌─ Sequence A (2026, Kenya)  ← Tip
              │
         ┌────┤
         │    └─ Sequence B (2023, Kenya)
    Root─┤
         │         ┌─ Sequence C (2023, Uganda)  ← Clade
         └─────────┤
                   └─ Sequence D (2022, DRC)

Branch point = Node/split
Horizontal line = Branch (evolutionary time)
Vertical line = Common ancestor
```

### Color Coding

```
By Geography:
├─ Red = Kenya
├─ Blue = Uganda
├─ Green = Nigeria
└─ Gray = Unknown location

By Date:
├─ Dark = Old (2015)
├─ Light = Recent (2024)
└─ Grayscale gradient

By Clade/Genotype:
├─ Clade 1 = Purple
├─ Clade 2 = Orange
└─ Clade 3 = Green
```

### What Phylogenetic Patterns Mean

```
Pattern: "Star-shaped tree"
Interpretation: Rapid diversification, recent common ancestor
Example: COVID-19 omicron explosion (100s of subvariants)
Action: New variant of concern; monitor closely

Pattern: "Ladder-like tree"
Interpretation: Sequential branching, gradual evolution
Example: Influenza seasonal strains
Action: Predictable evolution; vaccine updates possible

Pattern: "Geographically separated clades"
Interpretation: Different introductions/lineages per region
Example: CHIKV Asian vs ECSA lineages
Action: Different surveillance strategies per clade

Pattern: "Recent polytomy (star)"
Interpretation: Unresolved relationships (insufficient differences)
Example: COVID alpha/beta/gamma variants (early 2021)
Action: Need more sequences to resolve; evolution ongoing
```

---

# PART 8: WHY RVFV IS MISSING FROM NEXTSTRAIN

## The Current Situation

As of **June 2024**, Nextstrain does **not** have a dedicated RVFV build, despite:
- 20+ years of available sequence data
- Recent outbreaks in Kenya (2021), Saudi Arabia (2022), Somalia (2023)
- Global public health importance

**This is a critical gap for genomic epidemiology.**

---

## Why RVFV Was Not Prioritized (Historical Reasons)

### 1. **Geographic Bias in Nextstrain**

Nextstrain's initial builds (2013–2020) focused on:

```
High-priority viruses:
├─ Influenza (seasonal, pandemic risk, vaccine need)
├─ Dengue (most commonly sequenced arbovirus)
├─ Zika (pandemic potential, birth defects)
├─ Ebola (high CFR, biosafety interest)
├─ COVID-19 (obviously!)
│
Lower-priority arboviruses:
├─ West Nile virus (surveillance minimal)
├─ Chikungunya (eventually added)
├─ RVF virus (few sequences, limited surveillance)
└─ Etc.
```

**Why the bias?**

```
For Influenza:
├─ 1000s of sequences per year (WHO coordinated surveillance)
├─ Well-established reference strains
├─ Global outbreak preparedness (pandemic flu threat)
└─ $100M+ annual surveillance funding

For RVFV:
├─ 50–100 sequences per year (scattered African labs)
├─ Genome Detective provides automated typing (less need for trees)
├─ Geographically limited (Africa, Arabia; not global threat)
├─ <$5M annual surveillance funding
└─ Limited institutional coordination
```

### 2. **Technical Challenges Specific to RVFV**

#### 2a. **Tripartite Genome Complexity**

```
RVFV structure (unlike most RNA viruses):
├─ Segment L (6.4 kb): RNA polymerase (RdRp)
├─ Segment M (3.9 kb): Envelope glycoproteins (Gn, Gc)
└─ Segment S (1.7 kb): Nucleocapsid (NP) + NSs

Why this matters for phylogenetics:

Problem 1: Reassortment
├─ L, M, S segments can be exchanged between viruses
├─ Example: A virus has L from Lineage C,
│           M from Lineage D, S from Lineage A
├─ Standard tree methods assume uniparental inheritance
└─ Result: Phylogenetic signal is muddled
    (different segments = different evolutionary history)

Problem 2: Linked Selection
├─ If segments are reassorting, which one do you align?
├─ Full-genome concatenation breaks phylogenetic assumptions
├─ Individual segment trees may conflict
└─ No clear "best approach" for tripartite RNA viruses

Problem 3: Data Availability
├─ Many RVFV sequences lack all 3 segments
├─ Some are just M-segment (Gn gene used for typing)
├─ Some are only partial genomes
├─ Nextstrain requires complete genomes for trees
```

#### 2b. **Limited Genomic Data**

```
Number of full-genome RVFV sequences in NCBI:
├─ Year 2000: 2 sequences
├─ Year 2010: 15 sequences
├─ Year 2015: 40 sequences
├─ Year 2020: 150 sequences
├─ Year 2024: ~400 sequences (estimated)

Comparison to other viruses:
├─ Dengue: 10,000+ full genomes
├─ Chikungunya: 2,000+ full genomes
├─ COVID-19: 1,000,000+ genomes
└─ Influenza H1N1: 5,000+ per season

For Nextstrain to be effective:
├─ Minimum viable: 500+ recent genomes (past 5 years)
├─ Preferably: 100+ genomes per region for spatial analysis
└─ Currently: RVFV has ~150 recent genomes globally
    (concentrated in East Africa)
```

#### 2c. **Sparse Geographic Sampling**

```
RVFV sequence availability by region:

East Africa (Kenya, Ethiopia, Uganda)
├─ Sequences: ~200 (mostly Kenya 2021 outbreak)
├─ Sampling: Good for recent outbreaks
└─ Historical coverage: Minimal

West Africa (Mauritania, Senegal)
├─ Sequences: ~30 (mostly Senegal RVF)
├─ Sampling: Extremely sparse
└─ Lineage D distribution: Largely unknown

Southern Africa (South Africa, Zambia, Zimbabwe)
├─ Sequences: ~50
├─ Sampling: Intermittent, focused on Rift Valley
└─ Lineage coverage: Incomplete

Arabia (Saudi Arabia, Yemen)
├─ Sequences: ~80 (mostly historical, 1997-2000)
├─ Sampling: Few recent samples
└─ Lineages A, B: Not well documented since 1990s

Central/West African regions
├─ Sequences: <20
├─ Sampling: Minimal to none
└─ Lineages C, D, E: Severely underrepresented
```

**Nextstrain requirement**: Balanced geographic sampling. Currently, RVFV is heavily skewed toward East Africa (Kenya 2021 outbreak), making global phylogenetic inference unreliable.

#### 2d. **Temporal Sampling Bias**

```
RVFV sequences availability by time:

Pre-2000:       5 sequences (very old, reference strains)
2000-2010:      25 sequences (sparse, clinical interest only)
2010-2015:      30 sequences (minimal surveillance)
2015-2020:      50 sequences (increasing interest)
2020-2021:      150 sequences (Kenya outbreak 2021)
2021-2024:      140 sequences (post-outbreak investigations)

Problem: Unequal sampling

If a Nextstrain tree has:
├─ 10 sequences from 2021 Kenya outbreak
├─ 2 sequences from 2015 Tanzania
├─ 1 sequence from 1997 Saudi Arabia
│
The tree will be dominated by Kenya 2021 (overdetermined).
Other geographic regions' evolutionary history is poorly resolved.

Result: Phylogenetic tree doesn't accurately represent
        global RVFV evolution; only Kenya 2021 dynamics.
```

### 3. **Institutional Barriers**

#### 3a. **Decentralized Sequencing Efforts**

```
Influenza surveillance (centralized):
├─ WHO-coordinated global surveillance network
├─ ~150 authorized laboratories worldwide
├─ Mandatory real-time data sharing
├─ Harmonized protocols
└─ → Easy for Nextstrain to curate

RVFV surveillance (fragmented):
├─ No international coordination body
├─ Sequencing scattered: ILRI, KEMRI, CDC, etc.
├─ No mandatory data sharing
├─ Variable protocols (different primers, platforms)
├─ Data stuck in local databases or unpublished
└─ → Nextstrain curators must manually identify & download
```

#### 3b. **Limited Funding for RVFV Genomic Surveillance**

```
Annual surveillance investment (approximate):

Influenza:
├─ WHO: $50M+
├─ National programs: $500M+ globally
└─ Total: ~$550M+

COVID-19 (pandemic response):
├─ Global: $5,000M+
└─ Total: Unprecedented

Dengue:
├─ WHO/health ministries: $100M+
└─ Total: ~$100M+

RVFV:
├─ WHO: $2-3M
├─ National programs: $5-10M globally
└─ Total: ~$12-15M
```

Without dedicated funding, RVFV genomic surveillance remains sporadic and event-driven (outbreak response) rather than systematic.

#### 3c. **Lack of Dedicated Nextstrain Maintainer**

Each Nextstrain build requires:

```
Initial development: 200–500 hours
├─ Curate reference sequences
├─ Define phylogenetic clades
├─ Create geographic/lineage color schemes
├─ Build pipeline scripts
└─ Validate results

Ongoing maintenance: 10–20 hours/month
├─ Monitor new sequence submissions
├─ Update phylogenetic trees
├─ Respond to user inquiries
├─ Bug fixes
└─ Annual review/optimization

For RVFV: No dedicated person has done this work.

Nextstrain core team (~10 people) prioritizes:
├─ Influenza (WHO mandate)
├─ COVID-19 (pandemic response)
├─ Dengue (WHO focus)
└─ Community contributions (limited for RVFV)
```

---

## Why RVFV *Should* Have a Nextstrain Build

### 1. **Emerging Pathogen with Outbreak Potential**

```
RVFV meets WHO criteria for epidemic preparedness:

✓ Zoonotic spillover potential (livestock → human)
✓ High case-fatality rate in some lineages (CFR 0.5–40%)
✓ Climate-sensitive (rainfall-driven epizootic cycles)
✓ Limited vaccine/treatments
✓ Recent outbreaks in new geographic areas
  (Kenya 2021 was largest outbreak in 25 years)
✓ Potential for pandemic spread (if virus reaches Africa-Asia interface)
```

### 2. **Genomic Surveillance Already Ongoing**

```
Existing data:
├─ ~400 full-genome RVFV sequences available
├─ Recent outbreak (Kenya 2021) well-characterized
├─ Multiple research groups actively sequencing
├─ Genome Detective platform producing 100s of consensuses/year
└─ Infrastructure exists; just needs integration

Nextstrain requirement: Minimum 300–500 sequences
Status: ✓ Already met

What's missing: Curator/maintainer and pipeline development
```

### 3. **Unique Scientific Questions**

RVFV phylogenetics would reveal:

```
1. Reassortment patterns
   ├─ How often do segments recombine?
   ├─ Do certain combinations have fitness advantages?
   └─ → Understanding this could predict future outbreaks

2. Adaptive evolution
   ├─ Are envelope proteins evolving under immune selection?
   ├─ Do NSs mutations affect immune evasion?
   └─ → Could guide vaccine design

3. Geographic spread
   ├─ How did Kenya 2021 outbreak originate?
   ├─ Is circulation sustained or sporadic?
   └─ → Informs vector control strategy

4. Lineage divergence
   ├─ When did 15 lineages (A–O) diverge?
   ├─ Are cryptic lineages still circulating?
   └─ → Understanding evolutionary timescale
```

---

## What Would Be Needed to Create RVFV Nextstrain

### Step 1: Sequence Curation (1–2 months)

```
Tasks:
├─ Identify all available RVFV genomes (NCBI, GISAID, unpublished)
├─ Remove duplicates/low-quality sequences
├─ Standardize naming conventions
├─ Extract metadata (date, location, lineage, host)
├─ Create reference genome set (one per lineage A–O)
└─ Quality control (no contaminants, adequate coverage)

Expected: ~350–400 curated sequences
Time: 40–60 hours
```

### Step 2: Pipeline Development (2–4 months)

```
Code needed:
├─ Automated data download + curation scripts
├─ Sequence alignment workflow (segment-specific)
├─ Reassortment detection algorithm
├─ Three separate phylogenetic builds
│  ├─ L segment tree
│  ├─ M segment tree
│  └─ S segment tree
├─ Consensus tree (majority-rule)
├─ Auspice configuration (colors, metadata)
└─ Continuous integration/update scripts

Expected: ~2000–3000 lines of code + docs
Time: 80–120 hours
Technical skills: Bioinformatics + software engineering
```

### Step 3: Validation & Launch (1–2 months)

```
Tasks:
├─ Validate trees against published phylogenies
├─ User testing (feedback from virologists)
├─ Documentation + tutorials
├─ Server deployment + monitoring
└─ Launch + community announcement

Expected: Functional, user-friendly Nextstrain build
Time: 40–60 hours
```

### Step 4: Ongoing Maintenance (10–20 hours/month)

```
Monthly tasks:
├─ Monitor for new sequence submissions
├─ Update trees (auto-pipeline)
├─ Respond to user questions
├─ Bug fixes
└─ Outbreak-response updates (if outbreak occurs)

Annual task:
├─ Review lineage definitions
├─ Incorporate new research
└─ Optimize visualizations
```

### Total Resource Requirement

```
Setup:    300–400 hours (~2.5 FTE-months)
Personnel: 1 bioinformatician + 1 software engineer
Compute:  ~$500/month (AWS hosting)
Ongoing:  10–20 hours/month (1 curator)
Timeline: 6–9 months to production-ready
```

---

## Current Initiatives (2023–2024)

### Academic Efforts

```
1. Oyola et al. (2022) — Genome Detective RVFV typing tool
   ├─ Created automated lineage assignment system
   ├─ Validated on >230 RVFV genomes
   └─ Now: Used as de facto standard for RVFV typing
   
   Impact: Reduced need for Nextstrain-style phylogenetics
           (Genome Detective provides lineage + evidence)

2. Juma et al. (2023) — Amplicon-based sequencing
   ├─ Developed 74-primer tiling scheme
   ├─ Enables rapid NGS-based RVFV diagnostics
   └─ Generated 100+ new RVFV genomes
   
   Impact: Increases future data availability for Nextstrain
```

### Public Health Efforts

```
1. Africa CDC Pathogen Surveillance Board
   ├─ Established RVFV as priority pathogen
   ├─ Coordinating surveillance across 55 African countries
   ├─ Target: 200+ new RVFV genomes by 2025
   └─ Goal: Better understanding of African RVFV diversity

2. WHO Outbreak Preparedness Initiative
   ├─ Funding RVFV genomic surveillance in endemic regions
   ├─ Supporting capacity building in East/West Africa
   └─ Encouraging data sharing to public repositories

Impact: Increasing volume of sequences available for curation
```

### Why Not Yet a Nextstrain Build?

```
Barriers:
├─ ✗ No dedicated curator (other priorities)
├─ ✗ Funding gap (not part of WHO surveillance mandate)
├─ ✗ Technical complexity (tripartite genome)
├─ ✗ Scattered institutional ownership
└─ ✗ Genome Detective provides 90% of needed functionality
    (lineage typing, phylogenetic context)

Current workaround:
├─ Researchers use Genome Detective for routine typing
├─ Ad-hoc phylogenetic analyses for publications
├─ No real-time global tracking (unlike COVID-19, influenza)
└─ Outbreak response delays (no pre-built analysis pipeline)
```

---

## Future Outlook: Why RVFV Nextstrain Will Happen

### Drivers for Implementation (2024–2026)

```
1. Increasing Data Availability
   ├─ Amplicon-PCR (Juma et al. 2023) enables rapid sequencing
   ├─ Africa CDC expansion: +200 genomes/year expected
   ├─ Data volume reaching critical mass (~500+ sequences)
   └─ → Threshold for feasible Nextstrain curation

2. Climate Change Awareness
   ├─ RVFV outbreaks increasing in frequency & geography
   ├─ Models predict expansion to southern Europe/Asia
   ├─ Pandemic preparedness funding increasing
   └─ → Justifies investment in surveillance infrastructure

3. Genomic Epidemiology Maturation
   ├─ Nextstrain proven effective (COVID-19, mpox)
   ├─ Public health agencies demanding real-time phylogenetics
   ├─ Technical capability now routine
   └─ → RVFV is "obvious next step"

4. Institutional Momentum
   ├─ ILRI (International Livestock Research Institute) interested
   ├─ KEMRI-Wellcome Trust gaining sequencing capacity
   ├─ Pan-African sequencing hubs being established
   └─ → Critical mass of interested stakeholders
```

### Likely Timeline

```
2024–2025: Continued fragmented sequencing
           └─ Genome Detective remains primary tool

2025–2026: Nextstrain RVFV pilot project
           ├─ Likely funded by WHO/CEPI
           ├─ Focus on East Africa + Arabia
           └─ Expected 2–3 person-years of effort

2026–2027: Public launch + ongoing curation
           ├─ Real-time tree updates
           ├─ Integration with Africa CDC systems
           ├─ Outbreak response capability
           └─ Academic impact (publications)

2027+:     Mature platform
           ├─ Standard tool for RVFV surveillance
           ├─ Global participation (all affected countries)
           └─ Integration with epidemic prediction models
```

---

## What YOU Can Do (to Help)

If you're interested in advancing RVFV genomic surveillance:

```
1. Sequence RVFV samples
   └─ Use amplicon-PCR protocol from Juma et al. (2023)
   └─ Share data via GenBank/ENA (public repository)

2. Contribute to Genome Detective database
   └─ Upload new RVFV genomes with metadata
   └─ Enable lineage typing + characterization

3. Participate in collaborative phylogenetics
   └─ Contact Oyola/Juma (ILRI) for ongoing analyses
   └─ Contribute unpublished sequences to meta-analyses

4. Advocate for funding
   └─ Write grant proposals for RVFV genomic surveillance
   └─ Contact WHO/Africa CDC about RVFV prioritization

5. Propose Nextstrain build
   └─ Contact Nextstrain team (http://community.nextstrain.org)
   └─ Offer to collaborate on pipeline development
```

---

# TROUBLESHOOTING & FAQ

## Common Issues & Solutions

### Issue: `prefetch: command not found`

**Solution:**
```bash
# Check if sra-tools is installed
which prefetch

# If not found, add to PATH
export PATH=$PATH:/path/to/sratoolkit/bin

# Or reinstall
conda install -c bioconda sra-tools=3.0.0
```

---

### Issue: `prefetch` downloads very slowly

**Solutions:**
```bash
# Option 1: Use fasterq-dump directly (faster)
fasterq-dump --split-3 SRR28841803 --threads 16

# Option 2: Increase prefetch threads
prefetch SRR28841803 --threads 16

# Option 3: Use alternative download (AWS S3)
aws s3 cp s3://sra-pub-run-odometer/SRR28841803 . --region=us-east-1

# Option 4: Check internet speed
speedtest-cli
```

---

### Issue: FASTQ file is empty or corrupted

**Solutions:**
```bash
# Check file integrity
gunzip -t file.fastq.gz && echo "OK"

# Count reads
zcat file.fastq.gz | wc -l  # Divide by 4 for actual reads

# Re-download from scratch
rm ~/ncbi/public/sra/SRR28841803.sra
prefetch SRR28841803  # Try again
fasterq-dump --split-3 SRR28841803
```

---

### Issue: Genome Detective won't accept my FASTQ files

**Solutions:**
```
Possible causes:
├─ File format wrong (must be .fastq or .fastq.gz)
├─ Compressed incorrectly (use gzip, not zip)
├─ File name has special characters
└─ Browser cache issue

Fixes:
├─ Verify format: file SRR28841803.fastq.gz
├─ Re-compress: gunzip file.gz && gzip file
├─ Rename: mv "SRR28841803 [1].fastq.gz" SRR28841803.fastq.gz
└─ Clear browser cache: Ctrl+Shift+Delete
```

---

### Issue: Low coverage (<100×) after assembly

**Possible causes & solutions:**

```
Cause 1: Insufficient starting material
├─ Problem: Low viral titer in original sample
├─ Solution: Use higher-titer samples when available
└─ Workaround: Conservative consensus (accept 50% confidence)

Cause 2: Low input read count
├─ Problem: Didn't sequence deep enough
├─ Solution: Re-sequence sample (deeper coverage)
├─ Workaround: Pool multiple samples for population-level analysis

Cause 3: Host contamination (reads assigned to human genome)
├─ Problem: FASTQ contains >90% host DNA
├─ Solution: Genome Detective filters automatically
└─ Workaround: If pre-filtered, use capture-based enrichment

Cause 4: Wrong assembly reference
├─ Problem: Assembled against wrong RVFV lineage reference
├─ Solution: Genome Detective auto-selects best reference
└─ Workaround: Manually select reference if needed
```

---

### Issue: Conflicting SNPs in Genome Detective consensus

**What this means:**
```
"Conflicting SNPs" = positions where reads disagree (heterozygosity)

Normal ranges:
├─ <1% of positions: Normal (sequencing error)
├─ 1–5% of positions: Acceptable (minor variants)
├─ 5–10% of positions: Concerning (mixed infection?)
└─ >10% of positions: Likely mixed culture or contamination

If >5%:
├─ Check if sample is co-infection (multiple RVFV lineages)
├─ Check for non-RVFV contaminant
└─ Re-sequence or treat as low-confidence consensus
```

---

### Issue: Genome Detective typing shows uncertainty (low bootstrap)

**What this means:**
```
Bootstrap <70%: Uncertain lineage assignment
├─ Possible cause 1: Sequence too divergent from references
├─ Possible cause 2: Recombinant/reassorted genome
├─ Possible cause 3: Low coverage in phylogenetically informative regions
└─ Action: Manual inspection of SNP patterns needed

What to do:
├─ Look at SNP list: Which diagnostic markers are present?
├─ Compare to reference sequences manually
├─ Ask: "Which lineage's SNPs do I have?"
└─ If unclear: Report as "Lineage ambiguous" in publications
```

---

## FAQ

**Q: Can I use Genome Detective for non-RVFV viruses?**

A: Yes! Genome Detective supports 40+ viruses: Dengue, Chikungunya, Ebola, Zika, Yellow Fever, West Nile, Influenza, COVID-19, etc.

---

**Q: How long does assembly take on Genome Detective?**

A: Typical 10–45 minutes depending on coverage depth. Large datasets (>20M reads) may take 2–4 hours.

---

**Q: Can I analyze partial genomes (e.g., only M segment)?**

A: Yes. Upload what you have. Typing will work on any segment, though full-genome analysis is more reliable.

---

**Q: Is my sequence data secure on Genome Detective?**

A: Yes. Private submissions are encrypted and not shared. Public submissions are indexed in GenBank/ENA per FAIR principles but not sold or commercialized.

---

**Q: Can I download all my results from Genome Detective?**

A: Yes. You have full download access to consensus sequences, quality metrics, alignment files, and phylogenetic trees.

---

**Q: What if the consensus has many N's (missing bases)?**

A: N's indicate low-coverage or ambiguous positions. They're intentionally conservative (better to say "I don't know" than guess). For variant calling, only use positions with high confidence (no N's).

---

**Q: Why aren't all samples in my study typing to the same lineage?**

A: Possible explanations:
- Multiple lineages circulating in that outbreak
- Reassorted genome (segments from different lineages)
- Sample contamination with multiple viruses
- Sequencing error (rare if well-covered)

Phylogenetically, each segment tells the truth. Check if L/M/S segments agree.

---

**Q: Can I use Nextstrain for my own virus phylogenetics offline?**

A: Yes! Nextstrain's pipeline (Augur) is open-source: https://github.com/nextstrain/augur

```bash
conda install -c bioconda augur

# Build your own tree:
augur prepare --sequences sequences.fasta
augur align --sequences prepared.fasta
augur tree --alignment aligned.fasta
augur refine --tree tree.nwk
```

---

**Q: How often should I re-sequence RVFV outbreaks?**

A: Best practice:
- **Early outbreak**: Every 2–4 weeks (track emergence)
- **Established outbreak**: Monthly (detect variants)
- **Declining phase**: Every 2–3 months (confirm clearance)
- **Post-outbreak**: One sample per 10 cases (characterize diversity)

---

**Q: What's the cheapest way to set up this workflow for a small lab?**

A: Minimal setup (~$3,000 one-time):

```
├─ HPC cluster access (often free through university): $0
│  OR laptop with 16 GB RAM: $1,000
├─ Conda + packages: Free
├─ Genome Detective: Free (web-based)
├─ SRA-Tools: Free
└─ Optional: Sequencer (Illumina iSeq = ~$30,000)
   OR Outsource sequencing to contract lab ($50–100/sample)
```

Ongoing costs:
- Sequencing: $50–200 per sample
- Internet: Included
- Software: Free (open-source)
- **Total first-year cost**: $5,000–10,000 (reasonable for viral diagnostics)

---

# REFERENCES

## Primary Literature

1. **Oyola, S.O., et al. (2022).** Automatic identification of Rift Valley fever virus lineages and recombination events using Genome Detective. *BMC Genomics* 23, 560.
   https://doi.org/10.1186/s12864-022-08764-6

2. **Juma, A.J., et al. (2023).** Development and evaluation of a multiplex PCR amplicon scheme for whole-genome sequencing of Rift Valley fever virus. *Viruses* 15, 477.
   https://doi.org/10.3390/v15020477

3. **Vilsker, M., et al. (2019).** Genome Detective: An automated sequence interpretation system for infectious diseases. *Genome Biology* 20, 59.
   https://doi.org/10.1186/s13059-019-1585-7

4. **Quick, J., et al. (2017).** Real-time, portable genome sequencing for Ebola surveillance. *Nature* 530, 228–232.
   https://doi.org/10.1038/nature16996

5. **Pepin, K.M., et al. (2010).** Inferring the origins of the SARS-CoV-2 omicron variant. *Evolutionary Bioinformatics* 16, 1176934321050361.
   https://doi.org/10.1177/1176934321050361

---

## Tools & Platforms

- **NCBI Sequence Read Archive (SRA)**: https://www.ncbi.nlm.nih.gov/sra
- **Genome Detective**: https://www.genomedetective.com
- **RVFV Typing Tool**: https://www.genomedetective.com/app/typingtool/rvfv/
- **Nextstrain**: https://nextstrain.org
- **SRA-Tools Documentation**: https://github.com/ncbi/sra-tools/wiki
- **FastQC**: https://www.bioinformatics.babraham.ac.uk/projects/fastqc/
- **Fastp**: https://github.com/OpenGene/fastp

---

## Further Reading

- GISAID (Global Initiative on Sharing Avian Influenza Data): https://www.gisaid.org
- EMBL-EBI (European Bioinformatics Institute): https://www.ebi.ac.uk
- Africa CDC: https://africacdc.org/disease/rift-valley-fever/
- WHO RVF Fact Sheet: https://www.who.int/news-room/fact-sheets/detail/rift-valley-fever

---

## Acknowledgments

This documentation was prepared as educational material for genomic epidemiology training, integrating the expertise of the ILRI Genomics Platform (Oyola, Juma) and international bioinformatics frameworks (Nextstrain, Genome Detective).

---

**Document Version**: 1.0  
**Last Updated**: June 2024  
**Author**: Gilbert Kibet, ILRI  
**Intended Audience**: Virology students, genomics researchers, public health professionals  

**Feedback Welcome**: Contact kibet.gilbert.r@gmail.com or submit issues via institutional channels.

---

**END OF DOCUMENT**
