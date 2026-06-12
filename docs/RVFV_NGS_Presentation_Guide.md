# Pathogen Identification & Surveillance through NGS:
## Rift Valley Fever Virus and Beyond
### Comprehensive Speaker Notes & Educational Guide

**Presentation Date:** June 12, 2026  
**Venue:** Umeå University, Umeå, Sweden  
**Course:** National Doctoral Programme in Virus Infections and Pandemics (NDP-VIP)  
**Author:** Gilbert Kibet, ILRI Genomics Platform  

---

## 1. LEARNING OBJECTIVES

By the end of this presentation, learners will be able to:

1. **Understand** the importance of genomic surveillance for emerging infectious diseases
2. **Explain** the specific challenges in recovering viral genomes from clinical samples
3. **Differentiate** between NGS approaches (metagenomics vs. targeted enrichment)
4. **Describe** the tiled amplicon PCR methodology for viral genome recovery
5. **Apply** phylogenetic and lineage assignment methods to characterize pathogens
6. **Evaluate** the practical implementation of NGS for outbreak response
7. **Critically assess** the advantages and limitations of different sequencing approaches

---

## 2. RIFT VALLEY FEVER (RVF): BACKGROUND & CONTEXT

### 2.1 Disease Epidemiology

**What is RVF?**
- Acute febrile vector-borne zoonotic disease
- First described in 1931 in Kenya's Rift Valley following sheep abortion epidemic
- Endemic in Africa; sporadic cases in Arabian Peninsula (Saudi Arabia, Yemen)
- Linked to climate variability (abnormal rainfall, environmental shifts)

**Key Statistics:**
- Case fatality ratio: 0.5-2.0% in humans
- Livestock: High abortion rates ("abortion storms") and neonatal mortality (50-100%)
- Economically devastating in livestock-dependent regions
- WHO Priority List for epidemic preparedness (post-2018 Ebola emergency framework)

**Geographic Distribution:**
- Primarily sub-Saharan Africa (Kenya, Tanzania, South Africa, Zimbabwe, Egypt, etc.)
- 15 distinct RVFV lineages identified circulating in Africa
- Lineage C is the most predominant (documented since 1976)
- Multiple lineages can co-circulate in single countries

### 2.2 The Virus

**RVFV Characteristics:**
- **Family:** Phenuiviridae
- **Genus:** Phlebovirus
- **Genome:** Tripartite RNA (negative-sense, single-stranded)
  - L (Large) segment: 6,404 bp → RNA-dependent polymerase
  - M (Medium) segment: 3,885 bp → Glycoproteins (Gn, Gc)
  - S (Small) segment: 1,690 bp → Nucleocapsid protein (NP) & Non-structural protein (NSs)

**Genetic Diversity:**
- Low genomic diversity overall: 4% at nucleotide level, 1% at amino acid level
- M-segment slightly more diverse: 5% nucleotide, 2% amino acid level
- No well-defined serotypes (unlike dengue)
- Lineages distinguished by specific SNP patterns, not serological differences

### 2.3 Transmission & Vectors

**Mosquito Vectors:**
- **Primary:** Aedes and Culex species
- Infected vectors can transmit during viremia periods
- Vertical transmission in mosquitoes during rainy seasons amplifies transmission
- Climate-driven rainfall patterns trigger major outbreaks

**Human Infection Routes:**
1. Mosquito bite (most common)
2. Contact with infected animal tissue/blood (occupational exposure)
3. Inhalation of aerosolized particles (slaughterhouses, labs)
4. Inoculation via contaminated instruments

**Viremia Dynamics:**
- Peak viremia in first 5-8 days post-infection
- After 8 days, viremia drops dramatically
- RT-qPCR Ct value >30 indicates low viral load
- Critical window for sequence recovery from clinical samples

---

## 3. RELATED PATHOGENS: DENGUE & CHIKUNGUNYA

### 3.1 Dengue Virus (DENV)

**Disease Context:**
- Four distinct serotypes (DENV-1 to DENV-4)
- Single-stranded RNA genome (~10.7-11 kb)
- WHO category: Top 10 global public health threats
- Clinical spectrum: Asymptomatic → Dengue Hemorrhagic Fever (DHF) → Dengue Shock Syndrome (DSS)
- Transmitted by Aedes aegypti & A. albopictus
- Rising incidence due to urbanization, climate change, travel

**Surveillance Relevance:**
- Multiple co-circulating serotypes in endemic regions
- Serotype replacement important for vaccine strategy
- Phylogenetic analysis tracks introduction/spread of lineages

### 3.2 Chikungunya Virus (CHIKV)

**Disease Context:**
- Febrile illness with characteristic debilitating joint pain (arthralgia)
- Genome: ~11.8 kb single-stranded RNA
- Transmitted by Aedes aegypti & A. albopictus
- Rapid global spread; now endemic in >110 countries
- Significant transmission in Africa (Kenya, Mauritius) and Asia
- No vaccine currently available; prevention through vector control

**Clinical Features:**
- Sudden onset high fever + severe joint pain (often persists weeks/months)
- Rash, muscle aches, headache, fatigue
- Low mortality but high morbidity
- Post-infection chronic arthralgia affects quality of life

**Surveillance Relevance:**
- Genomic analysis tracks geographic spread and lineage evolution
- Detection of mutations potentially associated with enhanced virulence
- Co-circulation with dengue in tropical regions complicates clinical diagnosis

---

## 4. NGS CHALLENGES IN VIRAL DISEASE SURVEILLANCE

### 4.1 The Low Viral Titer Problem

**Definition:**
- Viral titers in clinical samples often <10,000 copies/μL
- Measured by RT-qPCR cycle threshold (Ct): High Ct = low viral load
- Ct >30 considered "low titer" for sequence recovery

**Why This Matters:**
- Metagenomics approaches generate millions of reads
- Of these, >99.9% may be host DNA/RNA
- Insufficient viral reads for accurate sequence assembly
- Results in fragmented, low-confidence genomes

**Real-world Example:**
- Zika virus outbreak studies (Quick et al.): Metagenomics alone failed to recover sufficient reads at <1,000 copies/μL viremia
- Solution: Developed amplicon-based enrichment approach

### 4.2 Host Background Contamination

**The Issue:**
- Clinical samples contain predominantly human DNA/RNA
- Tissue culture cells contain their own genomic material
- Pathogen DNA/RNA is minority component
- Standard sequencing "wastes" expensive reads on host material

**Impact on Results:**
- Incomplete genome coverage
- Uncertain variant calling (low coverage regions unreliable)
- Higher sequencing costs (need more total reads to reach pathogen coverage)
- Longer analysis time (filtering host reads computationally intensive)

### 4.3 RNA Instability & Sample Degradation

**RNA Characteristics:**
- Single-stranded RNA highly susceptible to degradation
- RNases ubiquitous in environment
- Temperature fluctuations accelerate degradation
- Repeated freeze-thaw cycles damage RNA

**Cold Chain Requirements:**
- Transport samples in -20°C freezer or dry ice
- Store at -80°C immediately upon receipt
- Minimize time at room temperature
- Use sterile, RNase-free tubes and pipette tips

**Clinical Implication:**
- Samples collected from remote outbreak sites may have degraded RNA
- Timing of collection relative to symptom onset critical
- Delayed shipment reduces recovery probability

### 4.4 Cell Culture Limitations

**Advantages:**
- High-titer virus replication (Ct values ~10-15)
- Pure viral genomes (minimal host contamination)
- Suitable for complete genome recovery

**Disadvantages:**
- Time-consuming: 7-14 days for viral isolation/propagation
- Labor-intensive: Requires specialized BSL-3 facilities
- Expensive: Equipment, reagents, trained personnel
- Genetic drift: Viral passaging introduces mutations not in clinical sample
- Ineffective for some viruses (don't grow well in standard cell lines)
- Biosafety concerns (RVFV requires BSL-3 containment)

**Mutation Problem:**
- Cell culture adaptation selects for variants favored in vitro
- Mutations introduced ≠ natural evolutionary mutations
- Phylogenetic inference compromised if passage history unknown
- Variant calling unreliable for low-abundance variants

---

## 5. NEXT-GENERATION SEQUENCING APPROACHES

### 5.1 Metagenomics (Unbiased Sequencing)

**What It Is:**
- Direct sequencing of all nucleic acids in sample
- No prior knowledge of pathogen required
- Provides complete picture of sample contents

**Advantages:**
- ✓ Unbiased (discovers unexpected pathogens)
- ✓ Useful when etiology unknown
- ✓ Can identify co-infections
- ✓ Comprehensive host-pathogen interaction data

**Disadvantages:**
- ✗ Low sensitivity for low-abundance pathogens
- ✗ High sequencing cost (many reads wasted on host)
- ✗ Requires high coverage (100,000+ reads) to detect virus
- ✗ Slow analysis (extensive bioinformatic processing)
- ✗ Results depend on reference databases (misclassification risk)

**When to Use:**
- Unknown pathogen cause of outbreak
- Novel virus suspected
- Samples with high viral titer (Ct <25)

### 5.2 Targeted Enrichment: Capture Probes

**What It Is:**
- Biotinylated oligonucleotide probes targeting pathogen sequence
- Probes hybridize to target sequences in sample
- Magnetic bead capture selects enriched molecules
- Remaining DNA/RNA sequenced

**Advantages:**
- ✓ Highly specific targeting
- ✓ Can design for multiple pathogens
- ✓ Good for very low titer samples

**Disadvantages:**
- ✗ Expensive probe synthesis (high upfront cost)
- ✗ Slow workflow (hybridization steps)
- ✗ Requires sequence knowledge (design probes ahead of time)
- ✗ Less cost-effective for rapid response

**When to Use:**
- Established, well-characterized pathogens
- High-throughput surveillance (multiple samples)

### 5.3 Targeted Enrichment: Amplicon PCR (RECOMMENDED)

**What It Is:**
- Design primers covering entire pathogen genome
- Multiple primer pairs used simultaneously (multiplex PCR)
- All target regions amplified in parallel
- Amplicons sequenced instead of genomic DNA/RNA

**Advantages:**
- ✓ **High specificity**: Mostly pathogen reads (cost-effective)
- ✓ **High sensitivity**: Works with low titer samples (Ct >30)
- ✓ **Fast**: PCR takes ~3 hours; entire workflow 24-48 hours
- ✓ **Cost-effective**: Cheap reagents, standard lab equipment
- ✓ **Flexible**: Design primers for new variants quickly
- ✓ **Scalable**: Easy to process 10-100s of samples

**Disadvantages:**
- ✗ Requires sequence knowledge (design primers ahead)
- ✗ Primer bias (non-binding regions poorly covered)
- ✗ Cannot discover novel pathogens
- ✗ Coverage uneven if primers poorly designed

**When to Use:**
- RECOMMENDED for rapid outbreak response
- Known pathogens with good reference sequences
- Resource-limited settings
- Time-sensitive surveillance

**Real Cost Comparison:**
- Metagenomics: $500-1000 per sample (high read cost)
- Amplicon PCR: $50-100 per sample (minimal enrichment cost)
- Cell culture: $200-500 per isolate + 14 days

---

## 6. TILED AMPLICON SEQUENCING: METHODOLOGY

### 6.1 Primer Design Principles

**Concept of "Tiling":**
- Overlapping amplicon pairs cover target genome with no gaps
- Each amplicon ~400 bp (standard for short-read sequencing)
- Amplicons overlap by ~50-100 bp with neighbors
- Complete target sequence covered multiple times

**Two-Pool Strategy:**
- **Pool 1**: Odd-numbered primers (1, 3, 5, 7, ...)
- **Pool 2**: Even-numbered primers (2, 4, 6, 8, ...)
- Run two separate PCR reactions
- Prevents primer competition and primer dimer formation

**Primer Design for RVFV:**
- **74 total primers** (38 in Pool 1, 36 in Pool 2)
- Coverage by segment:
  - L segment: 20 primer pairs
  - M segment: 12 primer pairs
  - S segment: 6 primer pairs
- In silico coverage predicted: ~97-99%

### 6.2 Workflow Overview

**Step 1: Sample Preparation (Day 0)**
- Isolate viral RNA from clinical sample
- Use automated systems (e.g., TANBead Technology) for high-throughput processing
- Quantify RNA concentration
- Screen for virus presence (optional RT-qPCR)

**Step 2: cDNA Synthesis (Day 0)**
- Convert RNA to complementary DNA (cDNA)
- Use LunaScript or similar reverse transcriptase
- cDNA is DNA template for PCR

**Step 3: Amplicon PCR (Day 1)**
- Two separate multiplex PCR reactions (Pool 1 & Pool 2)
- Reaction conditions:
  - Denaturation: 98°C, 30 sec
  - 35 cycles: 95°C, 15 sec | 63°C, 5 min (long annealing favors specificity)
  - Hold: 4°C
- Low primer concentration minimizes background
- Long annealing times increase specificity

**Step 4: PCR Product Cleanup (Day 1)**
- Remove excess primers, nucleotides
- Use magnetic bead-based purification (AMPure beads)
- Pool amplicons from both primer pools

**Step 5: Library Preparation (Day 1-2)**
- Add sequencing adapters (indexing barcodes)
- Prepare for loading onto sequencer
- Uses NEBNext Ultra II or equivalent kit

**Step 6: Sequencing (Day 2)**
- Load library onto Illumina sequencer (MiSeq, NextSeq, etc.)
- Sequencing depth typically 1-10 million reads per sample
- Run takes 12-24 hours depending on platform

**Step 7: Bioinformatic Analysis (Day 2-3)**
- Quality control (FastQC)
- Read trimming (remove adapters, low-quality bases)
- Alignment to reference genome (BWA-mem)
- Primer trimming (remove PCR primer regions)
- Variant calling and consensus generation
- Coverage analysis

### 6.3 Real-World Data: RVFV Sample Results

**Sample Dataset:**
- 5 cattle samples from recent RVF outbreaks (Kenya & Rwanda)
- Compared three enrichment approaches:
  1. **amPCRe** (Amplicon multiplex PCR enrichment) - OUR METHOD
  2. **CCE** (Cell culture enrichment) - GOLD STANDARD
  3. **Direct** (No enrichment, metagenomics) - BASELINE

**Key Findings:**

| Approach | Ct Range | Genome Coverage | Time | Cost |
|----------|----------|-----------------|------|------|
| amPCRe | >30 (low titer) | 80-97% | 24-48 hrs | $50-100 |
| CCE | >25 | >99% | 7-14 days | $200-500 |
| Direct | <25 (high titer) | 25-70% | 12 hrs | $500+ |

**Critical Result:**
- **Amplicon enrichment successfully recovered near-complete RVFV genomes even from samples with Ct >30** (low viral titer)
- Direct metagenomics failed for these same low-titer samples
- Time savings: 7× faster than cell culture
- Cost savings: 2-5× cheaper than alternatives

### 6.4 Coverage Analysis Details

**Genome Coverage Patterns:**

1. **Coverage Dropouts at 5' and 3' Ends:**
   - First and last primer pairs showed reduced coverage
   - PCR efficiency lower at terminal regions
   - Solution: Design extra primers at genome termini

2. **Homopolymer Regions:**
   - S segment position 709-739: Stretch of C nucleotides
   - Sequencing fails in long homopolymers (technical limitation)
   - Appears as coverage dropout regardless of viral titer
   - Solution: Use different sequencer platform (e.g., nanopore) for problematic regions

3. **High Ct Samples (>30):**
   - Uneven amplicon primer performance
   - Some primers failed to amplify (primer design issue)
   - Amplicon coverage correlated inversely with Ct value
   - Threshold for reliable coverage: Ct <35

4. **Comparison to Alternatives:**
   - Cell culture enrichment: ~99% coverage, uniform distribution
   - Direct sequencing: 25-70% coverage, concentrated in high-abundance regions
   - **Amplicon PCR: 80-97% coverage, evenly distributed** ← BEST BALANCE

---

## 7. QUALITY ASSURANCE & VARIANT CALLING

### 7.1 SNP Concordance Analysis

**What It Measures:**
- Accuracy of variant calling across different sample preparations
- Do all three methods identify the same mutations?
- Confidence in results from amplicon-enriched samples

**Methodology:**
- Compare identified SNPs (Single Nucleotide Polymorphisms) between:
  - Cell culture enrichment (reference standard)
  - Amplicon PCR enrichment
  - Direct metagenomics
- Only samples with >90% coverage analyzed (high confidence)

**Real Results:**
- **L segment: 98.42% concordance** (191 SNPs identified)
- **M segment: 99.31% concordance** (147 SNPs identified)
- **S segment: 91.04% concordance** (67 SNPs identified)
- Overall: Amplicon PCR variant calls highly reliable

**Mutation Categories:**
- **Synonymous mutations**: Change DNA but not protein
- **Non-synonymous mutations**: Change protein sequence (missense)
- **Finding**: High proportion synonymous >> non-synonymous (expected for under-selection)

**Shared Mutations Across Lineages:**
- Identified 146 SNPs in L segment shared across all samples
- 89 in M segment, 39 in S segment
- These define lineage-specific signatures

---

## 8. PHYLOGENETIC ANALYSIS & LINEAGE ASSIGNMENT

### 8.1 Why Phylogenetics Matter

**In Outbreak Investigation:**
- Determine source of outbreak (local vs. imported)
- Identify transmission chains
- Assess genetic relationship of isolates
- Detect zoonotic spillover events
- Distinguish natural evolution from vaccine escape

**In Surveillance:**
- Monitor temporal and spatial spread
- Detect introduction of new lineages
- Understand evolutionary dynamics
- Predict future spread patterns

### 8.2 RVFV Lineage System

**Current RVFV Classification:**
- **15 lineages identified** (A through O)
- Based on glycoprotein (Gn) gene sequences and whole genome analysis
- Reference classification by Grobbelaar et al. (2011) - gold standard

**Lineage Distribution in Africa:**
- **Lineage C**: Most predominant (since 1976)
- **Lineage H**: Second most common
- Other lineages (A, B, D-G, I-O): Geographically or temporally restricted
- Multiple lineages can co-circulate in single country

**Geographic Patterns:**
- No clear geographic clustering of lineages
- Suggests widespread transmission/dispersal
- Most countries experience >1 lineage over time

### 8.3 Phylogenetic Tree Construction

**Methods Used:**
1. **Maximum Likelihood (ML)** - statistically rigorous, assumes evolutionary model
2. **Bayesian** - probabilistic, includes uncertainty estimates
3. **Neighbor-Joining (NJ)** - computationally fast, good for large datasets

**Software Tools:**
- **IQ-TREE**: Fast ML trees, bootstrap support
- **MrBayes**: Bayesian inference, posterior probabilities
- **PAUP**: Classic phylogenetics software

**Reference Sequences:**
- 51 unique representative sequences (glycoprotein gene)
- 47 unique sequences (whole genome segments)
- Chosen for bootstrap support >70% and phylogenetic clarity

### 8.4 Lineage Assignment Validation

**Tool: RVFV Typing Tool**
- Web application: https://www.genomedetective.com/app/typingtool/rvfv/
- Command-line: https://github.com/ajodeh-juma/rvfvtyping

**Methodology:**
1. User submits sequence (partial Gn gene or whole genome)
2. DIAMOND BLASTX search against protein database (species classification)
3. MAFFT alignment to reference sequences
4. Maximum likelihood tree construction (IQ-TREE)
5. Query sequence assigned to lineage if clusters monophyletically with clade (bootstrap >70%)

**Validation Results (Oyola et al. 2022):**
- **Gn gene classification**: 100% accuracy on 129 sequences
- **Whole genome classification**: 99-100% accuracy on 234 sequences
- **Sensitivity**: 100% for most lineages
- **Specificity**: 100% for correctly assigned lineages
- Only lineage G and J showed slightly lower support (61-68% bootstrap)

**Real Outbreak Application:**
- 5 samples from Kenya RVF outbreak (2021)
- All assigned to **Lineage C** (confirmed by both Gn and whole genome classifiers)
- Demonstrates tool reliability in outbreak investigations

---

## 9. PRACTICAL IMPLEMENTATION FOR SURVEILLANCE

### 9.1 Rapid Outbreak Response Protocol

**Timeline for NGS-based Identification:**

**Day 1 (Outbreak Detection):**
- Sample collection from suspected cases
- RT-qPCR screening for rapid confirmation
- RNA extraction
- Immediate shipping to reference lab

**Day 2-3 (Sequencing):**
- cDNA synthesis
- Amplicon PCR (two multiplexes)
- Library preparation
- Load sequencer

**Day 3-4 (Analysis):**
- Real-time base calling
- FastQC quality control
- Read alignment & coverage check
- Preliminary consensus generated

**Day 5-6 (Results):**
- Final consensus genomes
- Lineage assignment
- Initial phylogenetic trees
- Report to health authorities

**Total Time: 5-6 days** (vs. 7-14 days cell culture + sequencing)

### 9.2 Cost-Benefit Analysis

**Amplicon PCR Approach:**
- Equipment: $20,000-50,000 (standard lab equipment)
- Per-sample cost: $50-100
- Allows parallel processing of 8-96 samples
- Scalable with number of samples

**Cell Culture Approach:**
- Equipment: $100,000+ (BSL-3 facility, incubators, etc.)
- Per-isolate cost: $200-500
- Sequential processing (one week minimum)
- Requires specialized facility and trained staff

**Cost Equation:**
- Small outbreak (5 samples):
  - Amplicon: 5 × $100 = $500
  - Cell culture: 5 × $500 = $2,500 ✗
  
- Large outbreak (50 samples):
  - Amplicon: 50 × $100 = $5,000
  - Cell culture: 50 × $500 = $25,000 ✗

### 9.3 Regional Implementation: Africa CDC Context

**Africa CDC Bioinformatic Training Goals:**
1. Build capacity in African labs for NGS-based pathogen surveillance
2. Reduce dependence on international reference centers
3. Enable rapid outbreak response across African countries
4. Create standardized protocols for multiple pathogens

**Adaptation to Resource-Limited Settings:**
- Use open-source bioinformatic tools (free)
- Cloud-based analysis (low upfront infrastructure cost)
- Training materials in multiple languages
- Mentorship networks between established and emerging labs

**Multi-Pathogen Applicability:**
- Primer design pipeline (Primal Scheme) works for any RNA virus
- Same laboratory workflow for dengue, chikungunya, Zika, etc.
- Bioinformatic pipeline adaptable to different pathogens
- Standardized quality control metrics

---

## 10. DISCUSSION QUESTIONS & TALKING POINTS

### 10.1 Technical Questions

**Q1: Why does CT value matter so much for amplicon sequencing?**
*Speaker Talking Point:* CT value reflects viral load - lower CT means more viral RNA in sample. Ct >30 (low titer) is challenging because PCR must amplify from very few starting template molecules. Even with 35 cycles, some amplicons fail to reach sufficient concentration for sequencing.

**Q2: How do we know which lineage an unknown sample belongs to?**
*Speaker Talking Point:* We use phylogenetic tree clustering - if your sequence groups with all lineage C sequences in the tree with high statistical support (bootstrap >70%), we confidently call it lineage C. We validate by comparing glycoprotein gene region, which shows highest accuracy (100% in our validation).

**Q3: What's the advantage of two primer pools instead of one?**
*Speaker Talking Point:* In a single multiplex PCR with 74 primers, some complementary regions would hybridize to each other forming primer-dimers (false products) rather than amplifying target DNA. By separating odd/even primers into two reactions, we reduce competition and dimer formation, improving coverage uniformity.

**Q4: Can we detect low-frequency variants (e.g., drug resistance mutations) at 1% abundance?**
*Speaker Talking Point:* With deep sequencing (>10,000× coverage per position), yes. However, amplicon sequencing at standard depths (500-2,000×) typically detects variants >2-5% frequency. Cell culture may bias against low-frequency variants present in original sample.

### 10.2 Methodological Questions

**Q5: What happens if primers don't bind to all RVFV lineages?**
*Speaker Talking Point:* This is a real risk - if lineage C primers are used on lineage H virus with different sequence at primer binding sites, amplification fails. Solution: Validate primers against all known lineages in silico (predict binding), then test on reference strains.

**Q6: How do we handle samples that are mixed (co-infected with two lineages)?**
*Speaker Talking Point:* Phylogenetic analysis becomes ambiguous - the tree may show a sequence between two lineages rather than within one clade. Variant calls will show two different alleles at sites where lineages differ. This is actually valuable information - telling us about outbreak transmission!

**Q7: Can you do amplicon sequencing with degraded RNA samples?**
*Speaker Talking Point:* Partially. If RNA is fragmented but >200 bp average, amplicon PCR can still work (our ~400 bp amplicons are larger than degradation fragments). However, coverage will be uneven. Better solution is starting with higher sample concentration to overcome degradation.

### 10.3 Implementation & Training Questions

**Q8: What's the biggest barrier to implementing this in African labs?**
*Speaker Talking Point:* Three challenges: (1) **Equipment cost** - sequencers are expensive, though some labs share MiSeq machines; (2) **Bioinformatic expertise** - analyzing NGS data requires programming skills, but pipelines are becoming more user-friendly; (3) **Quality data** - consistent sample collection and cold chain crucial.

**Q9: How do we validate NGS results before sharing with authorities?**
*Speaker Talking Point:* Multiple QC steps: (1) Coverage - ensure >80% of genome sequenced; (2) SNP concordance - compare to cell culture gold standard when possible; (3) Phylogenetic consistency - does lineage assignment match expected origin?; (4) Resequencing - independently validate high-impact findings.

**Q10: Can this workflow be adapted for other mosquito-borne viruses?**
*Speaker Talking Point:* Absolutely! Same approach works for dengue, chikungunya, Zika, West Nile, yellow fever. Use Primal Scheme to design primers for each pathogen. Main difference: segmented viruses (like RVFV, Bunyaviruses) need segment-specific primers; unsegmented viruses (like dengue) need single set.

### 10.4 Clinical & Epidemiological Questions

**Q11: How does genomic data change outbreak management?**
*Speaker Talking Point:* Genomic surveillance answers critical questions: *Is this outbreak locally-amplified (multiple transmission chains) or imported single event?* *Are we dealing with one or multiple lineages?* *Is there evidence of reassortment (segmented viruses)?* This guides control strategies - quarantine vs. vector control priorities.

**Q12: What's the relationship between RVFV lineage and disease severity?**
*Speaker Talking Point:* This is an open question! Some lineages (like H) have been associated with larger outbreaks, but whether this reflects inherent virulence or transmission dynamics is unclear. That's why surveillance data is valuable - comparing genomics with epidemiological outcomes.

**Q13: Can we predict outbreak spread using phylogenetic trees?**
*Speaker Talking Point:* Partially. Time-scaled phylogenetic trees (molecular clocks) estimate when lineages diverged and spread geographically. If tree shows lineage C spreading from Kenya to Tanzania in 2020, we predict similar geographic spread patterns for new introductions. But environmental conditions (rainfall, vector abundance) ultimately determine spread.

---

## 11. LEARNING ACTIVITIES & ENGAGEMENT

### 11.1 Interactive Elements (Live Demonstration Ideas)

**Activity 1: RT-qPCR Cycle Threshold Interpretation**
- Show sample data: Ct values ranging 12-40
- Ask: Which samples are suitable for metagenomics? Amplicon PCR? 
- Challenge: Design sampling strategy for outbreak with mixed titers
- Discussion: How do you prioritize resources when cases have different viral loads?

**Activity 2: Phylogenetic Tree Reading**
- Display RVFV phylogenetic tree with labeled lineages (A-O)
- Ask: Where would a new unknown sequence cluster?
- Teach: How to read bootstrap values, interpret confidence
- Challenge: Given two sequences, determine if they're from same outbreak

**Activity 3: Primer Design Challenge**
- Show RVFV reference genome sequence
- Task: Identify 4 regions for primer placement (non-conserved regions)
- Discussion: What design failures could occur? (off-target binding, secondary structure)

**Activity 4: Case Study Analysis**
- Present real outbreak scenario (e.g., 2018 Kenya RVF outbreak)
- Provide timeline, geographic data, genomic results
- Question: How would you interpret results? What's the transmission chain?
- Application: Design surveillance strategy for future outbreaks

### 11.2 Suggested Group Discussions

**Discussion Topic 1: Surveillance Priorities**
- Should limited resources focus on endemic pathogens (RVF, dengue) or emerging threats?
- How do genomics inform priority-setting for surveillance budgets?
- Trade-offs: Depth (monitor few pathogens intensively) vs. breadth (screen many pathogens)

**Discussion Topic 2: Ethics of Rapid Sequencing**
- Does faster sequencing change outbreak response ethics?
- Is isolation more justified with genomic evidence of transmission chains?
- How do privacy concerns affect data sharing in genomic surveillance?

**Discussion Topic 3: Climate Change & Vector Surveillance**
- How does climate change shift RVFV endemic regions?
- What genomic signatures predict spillover events?
- Integration of climate models + genomic surveillance for prediction

---

## 12. REFERENCE MATERIALS & FURTHER READING

### 12.1 Key Publications

**Primary References (Your Work):**

1. **Oyola SO, et al. (2022).** "Genomic surveillance of Rift Valley fever virus: from sequencing to lineage assignment." *BMC Genomics* 23:520.
   - DOI: 10.1186/s12864-022-08764-6
   - Key insight: Development and validation of RVFV typing tool
   - Demonstrates 100% accuracy on glycoprotein gene classification

2. **Juma J, et al. (2023).** "Using Multiplex Amplicon PCR Technology to Efficiently and Timely Generate Rift Valley Fever Virus Sequence Data for Genomic Surveillance." *Viruses* 15(2):477.
   - DOI: 10.3390/v15020477
   - Key insight: Amplicon PCR workflow from sample to phylogenetics
   - Demonstrates near-complete genome recovery from low-titer samples

**RVFV Background:**

3. **Pepin M, et al. (2010).** "Rift Valley fever virus (Bunyaviridae: Phlebovirus): an update on pathogenesis, molecular epidemiology, vectors, diagnostics and prevention." *Veterinary Research* 41:61.
   - Comprehensive review of RVF biology and epidemiology

4. **Mehand MS, et al. (2018).** "The WHO R&D blueprint: 2018 review of emerging infectious diseases requiring urgent research and development efforts." *Antiviral Research* 159:63-67.
   - WHO priority pathogen designation for RVF

**NGS Methodology:**

5. **Fonseca V, et al. (2019).** "A computational method for the identification of dengue, Zika and chikungunya virus species and genotypes." *PLOS Neglected Tropical Diseases* 13(6):e0007231.
   - Demonstrates amplicon-based genotyping for multiple pathogens

6. **Quick J, et al. (2017).** "Real-time, portable genome sequencing for Ebola surveillance." *Nature* 530:228-232.
   - Seminal amplicon sequencing paper (ARTIC protocol)
   - Inspired development of RVFV amplicon primer schemes

### 12.2 Online Resources & Tools

**Phylogenetic Analysis:**
- **RVFV Typing Tool**: https://www.genomedetective.com/app/typingtool/rvfv/
- **Genome Detective**: Automated virus identification platform
- **IQ-TREE**: http://www.iqtree.org/ - Maximum likelihood phylogenetics
- **MrBayes**: https://nbisweden.github.io/MrBayes/ - Bayesian phylogenetics

**Bioinformatic Pipelines:**
- **RVFV-Amplicon-Seq**: https://github.com/ajodeh-juma/rvfvtyping
- **Primal Scheme**: https://github.com/artic-network/primal_schemes - Primer design
- **ARTIC-VISC**: Amplicon pipeline for various viruses

**Sequence Databases:**
- **NCBI GenBank**: https://www.ncbi.nlm.nih.gov/genbank/
- **NCBI Virus**: https://www.ncbi.nlm.nih.gov/labs/virus/
- **FluSurver**: Global influenza surveillance (similar approach)

**Educational Resources:**
- **Coursera**: NGS analysis courses
- **Bioconductor**: https://www.bioconductor.org/ - R packages for sequence analysis
- **EMBL-EBI**: Bioinformatic training workshops

### 12.3 Standard Protocols & Guidelines

**Sample Collection & Processing:**
- CDC Biosafety in Microbiological and Biomedical Laboratories (BMBL)
- WHO laboratory safety manual
- RVFV-specific BSL-3 requirements

**RT-qPCR for RVFV:**
- CDC Rift Valley Fever diagnostic protocols
- Published assays targeting L-segment conserved regions

**Quality Control Standards:**
- SEQC Consortium guidelines for RNA-seq QC
- NGS QC metrics (mapping rate >80%, coverage depth >100×)

---

## 13. SPEAKER NOTES BY SLIDE

### Slide 1: Title Slide
**Key Message:** Genomic surveillance is transforming pathogen identification and outbreak response.

**Talking Points:**
- This training series by Africa CDC aims to build surveillance capacity across African countries
- Using RVFV as model system because: (1) endemic in Africa, (2) multiple genetic lineages, (3) outbreak-prone
- Same principles apply to dengue, chikungunya, and emerging pathogens
- Genomics enables rapid, accurate pathogen tracking

**Audience Engagement:** Poll - "How many have used NGS in your research?" This helps gauge technical level.

---

### Slide 2-4: Background & Context
**Key Message:** Understand the epidemiology and genetic nature of pathogens you're surveilling.

**Critical Points to Emphasize:**
- RVFV genetic lineages (15 distinct) are not serotypes - we can't use serology alone for classification
- Climate linkage to RVFV outbreaks - abnormal rainfall drives mosquito amplification
- Genome organization (L, M, S segments) important because affects sequencing strategy
- RVF affects both animal and human health (One Health relevance)

**Interactive Element:** 
- Show map of RVFV geographic distribution across Africa
- Ask: "If you detect RVFV in your country, what's its likely source region based on lineage data?"

---

### Slide 5-7: NGS Challenges
**Key Message:** Low viral titers, host contamination, and time/cost constraints drive method selection.

**Make It Real:**
- ">99.9% host DNA means we sequence 1 million reads to get 1,000 pathogen reads. That's expensive!"
- "RNA degrades. A sample collected today but not processed for 5 days may fail sequencing."
- "Ct value >30 means <1000 viral copies/microliter - very difficult for unbiased metagenomics"

**Practical Example:**
- Show case: Zika outbreak, low viremia, metagenomics fails → amplicon PCR succeeds
- Timeline comparison: Metagenomics can take weeks; amplicon is days

---

### Slide 8-10: NGS Methods Comparison
**Key Message:** Choose method based on: viral titer, time available, cost, sample count.

**Decision Tree (Use as Visual):**
```
KNOWN PATHOGEN? 
  ├─ YES → HIGH TITER (Ct<25)?
  │         ├─ YES → METAGENOMICS (fast, cost-effective if >100M reads available)
  │         └─ NO  → AMPLICON PCR (recommended for Ct 25-35)
  └─ NO  → METAGENOMICS (unbiased discovery)
```

**Cost-Benefit:**
- For 10-50 samples: Amplicon PCR $500-5,000 total
- Same scale cell culture: $2,000-25,000 + 14 days
- Metagenomics (high coverage): $1,000-5,000 for low-abundance pathogens

---

### Slide 11-14: Tiled Amplicon PCR Workflow
**Key Message:** Multiplex PCR + two-pool strategy ensures complete, even genome coverage from low-titer samples.

**Visual Teaching Points:**
- Show primer pool diagram - emphasize 74 primers covering 11.9 kb tripartite genome
- Explain "tiling" - each position covered 2-3× by overlapping amplicons
- Two-pool separation prevents primer interference

**Real Numbers:**
- RVFV genome total: 6,404 bp (L) + 3,885 bp (M) + 1,690 bp (S) = 11,979 bp
- 74 primers ÷ 11,979 bp = 1 primer pair per ~162 bp → high density
- Expected coverage: 97-99% in silico

**Workflow Timing:**
- Day 0: RNA extraction
- Day 1: cDNA + PCR + library prep
- Day 2-3: Sequencing + base calling
- Day 4-5: Analysis + results

---

### Slide 15-18: Real Data & Validation
**Key Message:** Our amplicon approach produces high-quality results comparable to gold-standard cell culture.

**Data Interpretation:**
- Show coverage comparison graph
- Highlight: amPCRe = 80-97%, CCE = 99%, Direct = 25-70%
- **Message:** Amplicon PCR a sweet spot - fast, cheap, reliable coverage

**SNP Concordance:**
- 98-99% agreement with cell culture for most segments
- Only S segment slightly lower (91%) due to homopolymer problem
- **Message:** Variant calls reliable - suitable for phylogenetics

**Clinical Application:**
- 5 samples from RVF outbreak → all assigned Lineage C
- Phylogenetic placement consistent across different sequencing methods
- **Message:** Can confidently make surveillance decisions based on amplicon data

---

### Slide 19-22: Phylogenetics & Lineage Assignment
**Key Message:** Phylogenetic analysis converts sequences into epidemiologically useful information.

**Teaching Moments:**
1. Show RVFV tree with 15 labeled lineages
2. Highlight lineage C (most common)
3. Explain: Monophyletic = all sequences share common ancestor
4. Bootstrap value = statistical confidence (>70% = confident)

**Interactive Element:**
- "If my new sequence clusters here (point to tree) with lineage D, what does this tell me?"
  - Answer: This sample belongs to lineage D; likely from geographic region where D circulates
  - Implications: Different evolutionary history, possibly imported case

**Validation Results:**
- RVFV typing tool: 100% accuracy on glycoprotein gene
- Useful for routine surveillance
- Available as web app (no programming needed)

---

### Slide 23-25: Implementation & Outbreak Response
**Key Message:** Genomic surveillance accelerates outbreak investigation and control.

**Outbreak Scenario (Real Example):**
- Day 1: 5 cattle cases reported in Samburu, Kenya
- Day 2-3: Samples collected, shipped to reference lab
- Day 5: Genomic results: "Lineage C, phylogenetically related to 2021 outbreak"
- **Decision:** Likely ongoing transmission (not new introduction) → activate vector control

**Why This Matters:**
- Confirms RVFV diagnosis (no need for import hypothesis)
- Identifies likely source/transmission pattern
- Guides resource allocation (vector control vs. quarantine)

**Scale-Up for Africa CDC:**
- Establish 5-10 regional NGS hubs
- Train lab technicians in amplicon protocol
- Establish bioinformatic pipeline
- Create lineage database for all African RVFV sequences
- Share data regionally for better pathogen tracking

---

### Slide 26-27: Dengue & Chikungunya Applications
**Key Message:** Same methodology works for other arboviruses endemic in Africa.

**Dengue Relevance:**
- 4 co-circulating serotypes in many African countries
- Phylogenetic analysis distinguishes serotypes + genotypes
- Sequencing reveals whether case is imported or locally-amplified

**Chikungunya Relevance:**
- Rapid geographic spread documented since 2006
- Genomic tracking shows introduction routes (e.g., Asia → Africa)
- Emerging resistance/virulence variants detectable by sequencing

**Unified Approach:**
- Design dengue amplicon primers (similar ~400 bp scheme)
- Adapt bioinformatic pipeline
- Same lab workflow and equipment
- Enables integrated arbovirus surveillance

---

## 14. KNOWLEDGE CHECK & ASSESSMENT QUESTIONS

### For Real-Time Assessment (During Presentation)

**Question 1 (Multiple Choice):**
A sample has Ct value of 32. Which is most suitable for sequence recovery?
- A) Metagenomics only
- B) Amplicon PCR [CORRECT]
- C) Cell culture only
- D) Direct sequencing

**Question 2 (True/False):**
Two primer pools in amplicon PCR are used to prevent primer dimer formation. [TRUE]

**Question 3 (Short Answer):**
Name three challenges in recovering viral genomes from clinical samples.
- Answer: Low viral titer, host contamination, RNA degradation
- (Accept any three of: cost, time, sensitivity, specificity, etc.)

**Question 4 (Application):**
You discover RVFV sequences that cluster phylogenetically between lineages C and H. What might this represent?
- Answer: Possible recombination, sample mixture, or sequencing error
- (Excellent discussion point about data interpretation)

### For Post-Training Assessment

**Competency Checklist:**
Participant can:
- ☐ Explain when to use amplicon PCR vs. metagenomics
- ☐ Interpret RT-qPCR Ct values for NGS method selection
- ☐ Describe two-pool primer strategy and its purpose
- ☐ Analyze phylogenetic tree and determine lineage placement
- ☐ Design amplicon scheme for novel RNA virus
- ☐ Interpret SNP concordance results
- ☐ Apply genomic surveillance data to outbreak investigation

---

## 15. COMMON QUESTIONS FROM PARTICIPANTS

**Q: What if my sequencer breaks down during a critical outbreak?**
A: Pre-sequencing steps (RNA extraction, cDNA synthesis, PCR) are equipment-independent. Libraries can be frozen and sequenced later, or shipped to reference lab. Amplicons themselves can be screened by gel electrophoresis if needed quickly.

**Q: Can we detect mutations related to drug resistance in RVFV?**
A: There are no approved antivirals specifically for RVFV yet. However, sequencing can identify changes in interferon antagonist (NSs protein) that might affect immune escape.

**Q: How do we handle data privacy in surveillance?**
A: All sequence data can be anonymized (remove patient identifiers) before sharing globally. Consult local biosecurity/GAIN agreements. Some sequences kept in local databases, only summaries shared internationally.

**Q: Is amplicon PCR suitable for follow-up sequencing in clinical trials?**
A: Yes, excellent for monitoring viral evolution during treatment. Paired samples (pre/post-treatment) can show genetic changes.

**Q: What's the minimum sample size for reliable phylogenetics?**
A: Technically, even 1 sequence can be placed on existing tree. But >5 sequences needed for robust clade detection. For outbreak investigation, 10-20 isolates gives good transmission chain resolution.

---

## 16. CLOSING SUMMARY & CALL TO ACTION

### Key Takeaways:

1. **Genomic surveillance is transforming** our ability to understand and respond to emerging infectious disease outbreaks

2. **Rift Valley fever serves as excellent model** for applying NGS in resource-limited African settings due to endemic status, genetic diversity, and outbreak-prone nature

3. **Amplicon-based sequencing (not metagenomics)** is recommended approach for rapid outbreak response - balances speed, cost, sensitivity, and accuracy

4. **Phylogenetic analysis converts sequences into actionable intelligence** for outbreak investigation and control decisions

5. **Same methodology scales to dengue, chikungunya**, and other arboviruses - enabling integrated surveillance

6. **Africa CDC leadership important** for building distributed capacity and enabling real-time data sharing

### Call to Action:

- **Immediate:** Take home these protocols and discuss with your laboratory managers
- **Short-term:** Identify one outbreak pathogen where you could implement amplicon PCR
- **Medium-term:** Submit proposal for capacity building through Africa CDC program
- **Long-term:** Join regional genomic surveillance network for real-time data integration

### Contact & Further Support:

- ILRI Genomics Platform: S.Oyola@cgiar.org
- Africa CDC: [Regional office contact]
- Genome Detective: https://www.genomedetective.com
- GitHub/Primal Scheme: Training materials and code

---

## APPENDIX A: TECHNICAL GLOSSARY

**Amplicon:** PCR product; DNA segment between primer binding sites
**Bootstrap:** Statistical measure of confidence in phylogenetic tree topology (0-100%; >70% = confident)
**cDNA:** Complementary DNA synthesized from RNA template via reverse transcription
**Ct (Cycle Threshold):** RT-qPCR cycle at which signal exceeds background (inversely proportional to viral load)
**ELISA:** Enzyme-linked immunosorbent assay; antibody-based diagnostic
**Lineage:** Group of related viral sequences sharing defining genetic markers
**Metagenomics:** Sequencing all nucleic acids in sample without prior enrichment
**Monophyletic:** Group of sequences descending from single common ancestor
**Multiplex PCR:** PCR with multiple primer pairs in single reaction
**Phylogenetic Tree:** Branching diagram showing evolutionary relationships between sequences
**Primer:** Short DNA sequence that binds to specific genomic region; enables PCR amplification
**SNP:** Single nucleotide polymorphism; position where DNA sequence varies between individuals
**RT-qPCR:** Reverse transcription quantitative PCR; detects and quantifies RNA
**Tiling:** Overlapping amplicons covering target sequence with no gaps
**Viralemia:** Presence of virus particles in bloodstream

---

## APPENDIX B: TROUBLESHOOTING COMMON ISSUES

| Problem | Likely Cause | Solution |
|---------|--------------|----------|
| Very low genome coverage (<50%) | Low viral titer + poor RNA quality | Increase RNA input; check extraction method; avoid freeze-thaw |
| Uneven coverage (gaps in specific regions) | Primer binding failure in that region | Design alternative primers for problematic region; check secondary structure |
| No PCR product | Contamination/degradation of template | Extract new RNA; use positive control virus |
| Consensus sequences differ significantly between replicates | Sequencing depth too low (~10× coverage) | Increase sequencing depth or PCR product concentration |
| Lineage assignment fails (bootstrap <70%) | Sequence divergent from reference sequences | Validate sequence quality; consider sequencing additional segment |
| SNP concordance low (< 90%) | Sequencing errors or low coverage in specific regions | Validate variants by Sanger sequencing; increase coverage in problem regions |

