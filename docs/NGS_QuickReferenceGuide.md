# Amplicon PCR for Viral Surveillance: Quick Reference Guide

## FOR RAPID CONSULTATION DURING PRESENTATION

---

## QUICK FACTS AT A GLANCE

### RIFT VALLEY FEVER (RVFV)
- **First reported:** 1931, Kenya
- **Vectors:** Aedes & Culex mosquitoes
- **Geographic range:** Sub-Saharan Africa, Arabian Peninsula
- **Genome:** Tripartite RNA (L: 6.4kb | M: 3.9kb | S: 1.7kb)
- **Lineages:** 15 identified (C most common)
- **Case fatality:** 0.5-2.0%
- **Livestock impact:** 50-100% neonatal mortality

### DENGUE VIRUS (DENV)
- **Serotypes:** 4 (DENV-1 to DENV-4)
- **Genome:** Single-stranded RNA ~11 kb
- **Global burden:** WHO top 10 public health threat
- **Vectors:** Aedes aegypti, A. albopictus

### CHIKUNGUNYA VIRUS (CHIKV)
- **Genome:** ~11.8 kb single-stranded RNA
- **Geographic spread:** Now in >110 countries
- **Clinical hallmark:** Debilitating arthralgia (weeks-months)
- **Vectors:** Aedes aegypti, A. albopictus

---

## DECISION TREE: WHICH NGS METHOD?

```
START: Sample with unknown virus?
│
├─ YES → Use METAGENOMICS (unbiased discovery)
│
└─ NO → Known pathogen. What's the Ct value?
    │
    ├─ Ct <25 (HIGH titer)
    │   └─ → METAGENOMICS (cost-effective, straightforward)
    │
    ├─ Ct 25-30 (MODERATE)
    │   └─ → AMPLICON PCR (recommended)
    │
    ├─ Ct 30-35 (LOW)
    │   └─ → AMPLICON PCR (only viable option)
    │
    └─ Ct >35 (VERY LOW)
        └─ → Consider cell culture OR use higher RNA volume
```

---

## AMPLICON PCR WORKFLOW (4 EASY STEPS)

| Step | Time | Materials | Output |
|------|------|-----------|--------|
| 1. RNA Extract | 2 hrs | Sample → Extraction kit → RNA | High-quality RNA |
| 2. cDNA Synthesis | 1.5 hrs | RNA → Reverse transcriptase → cDNA | cDNA template |
| 3. Amplicon PCR | 3.5 hrs | cDNA + Primers (2 pools) → PCR → Amplicons | ~400 bp products |
| 4. Sequence & Analyze | 24-36 hrs | Amplicons → Library → Sequencer → Bioinformatics | Consensus genome |
| **TOTAL** | **24-48 hours** | Standard equipment | **Complete genome** |

---

## RVFV PRIMER SCHEME SUMMARY

| Segment | Size | Primer Pairs | Target Coverage |
|---------|------|--------------|-----------------|
| L (RNA polymerase) | 6,404 bp | 20 pairs | 97-99% |
| M (Glycoproteins) | 3,885 bp | 12 pairs | 97-99% |
| S (Nucleocapsid) | 1,690 bp | 6 pairs | 97-99% |
| **TOTAL** | **11,979 bp** | **74 primers** | **~98% overall** |

**Two-pool strategy:**
- Pool 1: Odd-numbered primers (38 total)
- Pool 2: Even-numbered primers (36 total)
- Run separately to prevent primer interference

---

## REAL COVERAGE DATA (Kenya RVF Outbreak Study)

### Amplicon PCR Enrichment (amPCRe) - OUR APPROACH
```
Ct Value Range:     Coverage:       Time:              Cost:
<25                 >90%            24-48 hours        $50-100
25-30               85-95%          24-48 hours        $50-100
30-35               80-90%          24-48 hours        $50-100
>35                 <80%            24-48 hours        $50-100
```

### Compared to Alternatives
```
Method              Coverage:       Time:              Cost:
Cell Culture       >99%             7-14 days          $200-500
Metagenomics       25-70%           1-2 weeks          $500-1000
Amplicon PCR       80-97%           24-48 hours        $50-100
```

---

## QUALITY CONTROL METRICS

### Minimum Acceptable Standards
- **Genome coverage:** >80% (can have gaps, but most sequence recovered)
- **Read depth:** ≥100× at each position (ensures accuracy)
- **SNP concordance:** >90% agreement with reference standard
- **Mapping quality:** >95% of reads map to reference

### Red Flags (RESEQUENCE if observed)
- Coverage <50%
- Ct value >35 at collection
- Highly non-uniform coverage pattern
- SNP concordance <85%

---

## PHYLOGENETIC INTERPRETATION GUIDE

### What is "Lineage" Assignment?
- Grouping RVFV sequences based on defining genetic markers
- NOT the same as "serotypes" (dengue)
- Based on phylogenetic clustering + SNP patterns

### How to Read Results
```
Your sequence ── Phylogenetic Analysis ──→ Clustering outcome
                                              │
                                              ├─ Clusters with lineage C
                                              │  + Bootstrap >70%
                                              │  = LINEAGE C ✓
                                              │
                                              ├─ Clusters between C & H
                                              │  = Ambiguous/recombinant
                                              │
                                              └─ No clear cluster
                                                 = Unassigned
```

### Bootstrap Values
- **>90%:** Very confident
- **70-90%:** Confident
- **50-70%:** Moderately confident
- **<50%:** Low confidence, don't use

---

## RVFV LINEAGE QUICK REFERENCE

| Lineage | Geographic Distribution | Frequency | Notes |
|---------|------------------------|-----------|-------|
| **C** | Kenya, widespread Africa | Most common (since 1976) | Expected in Kenya outbreaks |
| **H** | Various African countries | 2nd most common | Associated with recent outbreaks |
| **A** | Limited distribution | Rare | |
| **B-G** | Various | Rare-moderate | Geographically restricted |
| **I-O** | Limited | Very rare | |

**Interpretation:** If your sample is lineage C and collected in Kenya → consistent with local epidemiology

---

## COMMON PROBLEMS & SOLUTIONS

| Problem | Likely Cause | Solution |
|---------|--------------|----------|
| Very low coverage (<50%) | Poor RNA quality OR low viral titer | Increase RNA input, check extraction |
| No amplification (no PCR product) | Template degradation OR contamination | Extract fresh RNA, use positive control |
| Uneven coverage (specific gaps) | Primer failure in that region | Design alternative primers for region |
| All L segment good, M segment poor | Different viral load at specimen collection | Common; not usually a problem |
| Lineage assignment fails | Sequence divergent from reference | Sequence high-quality; may be new lineage |
| Huge number of mutations compared to reference | May be sequencing error artifact | Validate by Sanger sequencing; check base quality |

---

## SNP CONCORDANCE INTERPRETATION

**What it means:** % of mutations identified in your sample that match results from gold-standard method

```
Your amplicon PCR results vs. Cell Culture results (gold standard):
│
├─ 99% concordance ──→ Excellent, highly reliable
├─ 95% concordance ──→ Good, suitable for most uses
├─ 90% concordance ──→ Acceptable, but note discrepancies
└─ <90% concordance ──→ Investigate; possible sequencing errors
```

**Our RVFV study results:**
- L segment: 98.42% (excellent)
- M segment: 99.31% (excellent)
- S segment: 91.04% (acceptable; homopolymer region challenge)

---

## OUTBREAK RESPONSE TIMELINE

```
DAY 1 (Outbreak Detected)
│
├─ 8am: Cases reported
├─ 10am: Samples collected from affected livestock
├─ 12pm: Transport to lab on ice
├─ 2pm: Arrive at lab, RT-qPCR screening
├─ 4pm: Rapid diagnosis confirmation
└─ 5pm: Samples frozen for sequencing

DAY 2 (Sequencing Preparation)
│
├─ 8am: RNA extraction
├─ 10am: cDNA synthesis
├─ 12pm: Amplicon PCR (Pool 1 & 2)
├─ 3pm: Library preparation
├─ 5pm: Quality check, quantify libraries
└─ 6pm: Load onto sequencer

DAY 3-4 (Sequencing & Initial Analysis)
│
├─ Sequencing in progress (MiSeq: 12-24 hrs)
├─ Real-time base calling
└─ Preliminary fastQC checks

DAY 5 (Analysis Complete)
│
├─ 8am: Bioinformatic pipeline runs
├─ 12pm: Consensus genomes generated
├─ 2pm: Lineage assignment (RVFV typing tool)
├─ 3pm: Phylogenetic analysis
└─ 4pm: REPORT TO HEALTH AUTHORITIES

TOTAL: 5 CALENDAR DAYS
(vs. 7-14 days for cell culture approach)
```

---

## WHAT TO DO WITH GENOMIC RESULTS

### Lineage C Detected in Kenya Outbreak
```
Result: Lineage C ✓
│
└─→ Interpretation: Consistent with endemic Kenya RVF
    │
    ├─ LIKELY LOCAL TRANSMISSION
    │   (not import from other region)
    │
    ├─ Action: Activate vector control
    │   (not quarantine-focused)
    │
    └─ Implication: Ongoing circulation,
       expect additional cases in same area
```

### New Lineage or Unexpected Result
```
Result: Lineage H OR Lineage not assigned
│
└─→ Interpretation: Possible new introduction
    │
    ├─ INVESTIGATE FURTHER
    │   - Resequence to confirm
    │   - Compare to global database
    │   - Assess phylogenetic certainty
    │
    ├─ Action: Enhanced surveillance
    │   (check for linked cases)
    │
    └─ Implication: May indicate
       geographic spread of new lineage
```

---

## FREQUENTLY ASKED TEACHING QUESTIONS

### Q1: "Why not just use metagenomics for everything?"
**A:** Metagenomics fails for low-titer samples (>99.9% host DNA). At Ct >30, you'd need millions of reads to get sufficient pathogen coverage. Amplicon PCR enriches for pathogen → cheaper, faster.

### Q2: "Can we detect drug resistance?"
**A:** Not yet for RVFV (no approved antivirals). But for dengue/other viruses with antivirals, yes - sequencing detects any mutation in resistance-associated genes.

### Q3: "What if we have a mixture of two lineages?"
**A:** Phylogenetic tree will show your sequence between the two clades. SNP calls will show two different alleles at sites where lineages differ. This is informative - tells you about outbreak transmission!

### Q4: "How do we handle data privacy?"
**A:** Anonymize sequences (remove patient identifiers) before sharing. All public sequences stripped of personal data. Consult local regulations on data sharing.

### Q5: "Is this scalable to many samples?"
**A:** Yes! Can run 8-96 samples simultaneously on MiSeq/NextSeq using barcoding. Cost scales linearly. Cell culture is sequential (one week minimum even for 2 samples).

---

## SAMPLE CHECKLIST FOR SUCCESSFUL SEQUENCING

Before sending sample to lab:

- ☐ Ct value documented (ideally Ct <35)
- ☐ Sample type recorded (serum, tissue, swab)
- ☐ Collection date and location noted
- ☐ Temperature maintained at -20°C minimum during transport
- ☐ Packed with dry ice or blue ice packs
- ☐ Shipped overnight/fast courier
- ☐ Tracking number provided
- ☐ Lab notified of expected arrival
- ☐ No repeated freeze-thaw cycles
- ☐ Label on tube clearly identifies sample

---

## REFERENCE SEQUENCES & KEY TOOLS

### Must-Know Web Resources
1. **RVFV Typing Tool** (web): genomedetective.com/app/typingtool/rvfv/
2. **NCBI GenBank**: ncbi.nlm.nih.gov/genbank/ (sequence database)
3. **Primal Scheme**: github.com/artic-network/primal_schemes (primer design)
4. **IQ-TREE**: iqtree.org (phylogenetic analysis)

### Must-Read Papers (in priority order)
1. Oyola et al. 2022 (RVFV lineage assignment tool)
2. Juma et al. 2023 (Amplicon PCR methodology - YOUR WORK)
3. Pepin et al. 2010 (RVF comprehensive review)
4. Quick et al. 2017 (Amplicon sequencing pioneering work - Ebola)

---

## NOTES FOR FACILITATORS

### Time Management
- Full presentation: 60 minutes
- With discussion: 90 minutes
- Abbreviated version: 45 minutes (omit slides 11-18 if pressed for time)

### Interactive Elements to Engage Audience
1. Show phylogenetic tree on slide 27 → Ask "Where would a new sequence from your country cluster?"
2. Poll: "How many have used NGS in your work?" (gauge technical level)
3. Case study: Ask participants to interpret RT-qPCR data and choose NGS method
4. Live demo (optional): Show RVFV typing tool interface

### Common Misconceptions to Address
- "Amplicon PCR can't give complete genomes" → Actually achieves 80-97% coverage
- "Cell culture is always better" → Slower, more expensive, introduces mutations
- "Phylogenetics tells you disease severity" → It doesn't (different issue)
- "Lineage = serotype" → Not the same thing!

### Discussion Starters
1. "What's the biggest barrier to implementing NGS in your lab?"
2. "How would you integrate genomics into your current surveillance?"
3. "What diseases beyond RVF should we prioritize for genomic surveillance?"

---

## POST-PRESENTATION FOLLOW-UP

### Immediate (Day 1)
- Share slide deck and speaker notes
- Provide all hyperlinks to tools/databases
- Collect contact information for interested participants

### Short-term (Week 1-2)
- Send full comprehensive guide (this document)
- Provide access to RVFV typing tool tutorial
- Establish communication channel (email list, WhatsApp group)

### Medium-term (Month 1)
- Virtual office hours (weekly) for technical questions
- Facilitate lab-to-lab connections (regional hubs)
- Share primer design tutorial for other pathogens

### Long-term (Month 3+)
- Support first outbreak investigation using amplicon PCR
- Publish joint manuscript with participant success stories
- Annual workshop to update protocols and share new developments

---

*Last updated: June 2026*   
*Designed for: National Doctoral Programme in Virus Infections and Pandemics (NDP-VIP)*   
*Prepared by: Gilbert Kibet, ILRI Genomics Platform*
