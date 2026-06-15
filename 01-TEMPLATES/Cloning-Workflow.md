---
type: workflow
tags: cloning, workflow
---

# 🧬 Cloning Workflow Guide

## Overview
This is a generalized molecular cloning workflow that can be adapted to your specific needs.

## Step 1: Planning & Design
- [ ] Define your target insert
- [ ] Design primers for PCR amplification
- [ ] Choose cloning method:
  - [ ] Restriction enzyme digestion
  - [ ] Gateway cloning
  - [ ] Gibson assembly
  - [ ] TOPO cloning
- [ ] Select vector plasmid
- [ ] Check for compatible restriction sites

### Design Considerations
| Factor | Consideration | Choice |
|--------|---------------|--------|
| Insert Size | < 100 bp, 100-5kb, > 5kb | — |
| Accuracy | Standard PCR vs. HiFi | — |
| Efficiency | Single vs. multiple fragments | — |

## Step 2: PCR Amplification
- [ ] Design primers (see [[Primer-Template]])
- [ ] Set up PCR reaction
  - Template DNA: 1-10 ng
  - Forward primer: 10 μM
  - Reverse primer: 10 μM
  - PCR polymerase: HiFi
  - dNTPs: 200 μM each
- [ ] Run PCR program
- [ ] Analyze PCR product on agarose gel
- [ ] Purify PCR product

```
PCR Cycling Program:
├─ Initial Denaturation: 95°C, 3 min
├─ [Repeat 30-35 cycles]:
│  ├─ Denaturation: 95°C, 15-30 sec
│  ├─ Annealing: [Tm-5°C], 15-30 sec
│  └─ Extension: 72°C, [1 min/kb]
└─ Final Extension: 72°C, 5 min
```

## Step 3: Vector Preparation
- [ ] Select vector plasmid
- [ ] Digest vector with appropriate restriction enzymes
- [ ] **Optional:** Phosphatase treatment to prevent religation
- [ ] Run on agarose gel
- [ ] Purify vector DNA

## Step 4: Ligation
- [ ] Calculate molar ratio (insert:vector = 3:1 typically)
- [ ] Prepare ligation reaction:
  - Vector DNA: [X] ng
  - Insert DNA: [Y] ng
  - T4 DNA Ligase: 1-2 units
  - Ligation buffer: 1x
- [ ] Incubate 16°C overnight or room temperature for 1-2 hours
- [ ] Optional: Inactivate ligase at 65°C for 10 min

## Step 5: Transformation
- [ ] Choose competent cells (chemically competent or electrocompetent)
- [ ] Thaw competent cells on ice
- [ ] Add ligation product (1-50 μL)
- [ ] Incubate on ice for 20-30 min
- [ ] Heat shock: 42°C for 90 seconds
- [ ] Recovery: ice for 2 min, then media for 1 hour at 37°C
- [ ] Plate on selective medium

### Competent Cells Guide
| Type | Storage | Recovery Time |
|------|---------|----------------|
| Chemical (CaCl₂) | -80°C | 1 hour at 37°C |
| Electrocompetent | -80°C | 1 hour at 37°C |
| Commercial | -80°C | As per protocol |

## Step 6: Colony Screening
- [ ] Pick 3-6 colonies
- [ ] Grow overnight in selective medium (5 mL)
- [ ] Isolate plasmid DNA (miniprep)
- [ ] Digest with diagnostic enzymes
- [ ] Run on agarose gel to confirm insert size
- [ ] **Optional:** Verify by Sanger sequencing

## Step 7: Verification
- [ ] Send positive clones for sequencing
- [ ] Confirm correct insert sequence
- [ ] Verify no unwanted mutations
- [ ] Document in [[Molecular-Database]]

## Step 8: Storage & Documentation
- [ ] Grow positive clone in large scale (if needed)
- [ ] Prepare glycerol stocks (15-50%)
- [ ] Store at -80°C
- [ ] Document all details in plasmid note
- [ ] Update molecular database

## Troubleshooting

| Issue | Likely Cause | Solution |
|-------|--------------|----------|
| No PCR product | Low Tm, primer design issue | Check primer Tm, optimize PCR program |
| Few/no colonies | Ligation inefficient, poor transformation | Check insert:vector ratio, use fresh competent cells |
| Wrong size colonies | Self-ligation of vector | Phosphatase treat vector |
| Sequence incorrect | PCR error, contamination | Use HiFi polymerase, verify template |

## Alternative Cloning Methods

### Gibson Assembly
- [ ] Design primers with 15-20 bp overlaps
- [ ] PCR amplify all fragments
- [ ] Mix all fragments + Gibson Mix
- [ ] Incubate 15-60 min at 50°C
- [ ] Transform directly

### Gateway Cloning
- [ ] PCR with attB flanked primers
- [ ] BP reaction: PCR product + pDONR
- [ ] LR reaction: entry clone + destination vector
- [ ] Select on appropriate antibiotic

### TOPO Cloning
- [ ] Use TOPO polymerase for PCR
- [ ] Mix PCR product + TOPO vector
- [ ] Incubate room temperature, 5 min
- [ ] Transform directly

## Quick Reference: Enzyme Selection

| Enzyme | Recognition | Frequency | Notes |
|--------|-------------|-----------|-------|
| EcoRI | GAATTC | ~200-250 bp | Sticky ends |
| BamHI | GGATCC | ~200-250 bp | Compatible with EcoRI |
| PstI | CTGCAG | ~200 bp | Blunt compatibility |
| HindIII | AAGCTT | ~250 bp | Sticky ends |

---

## Related Notes
- [[Plasmid-Template]]
- [[Primer-Template]]
- [[Experiment-Template]]
- [[Molecular-Database]]
