# BEE-102-2025-Assignment

# CSE Assignment - Genomics Programming

This repository contains the Python implementations for tasks assigned to CSE students as part of the Genomics Programming module. Each task is implemented in a modular and memory-efficient manner, using appropriate data structures and visualization tools.

---

## 1. Fragment Length Frequency and Rescaling (3 marks)

**Files**: `query.bed.gz`, `reference.hist`

- **Fragment Length Frequency**: Computed by subtracting column 2 from column 3 in the BED file. Normalized frequencies were calculated and visualized with fragment length on the X-axis and "Normalized Frequency [A.U.]" on the Y-axis.
- **Rescaling**: The query fragment distribution was resampled to match the reference histogram using subsampling techniques. Both original and rescaled frequency plots are included for comparison.

---

## 2. Markov Transition Matrix (1 mark)

- A first-order Markov model was constructed from a DNA sequence.
- Transition probabilities between nucleotides (A, C, G, T) were computed and stored in a matrix.
- Output is formatted for readability and comparison with expected transition behavior.

---

## 3. Multi-line FASTA to Single-line Conversion (1 mark)

**File**: `multiline_input.fasta`

- FASTA sequences were parsed and converted such that each sequence appears on a single line directly after its header.
- The implementation avoids in-memory sequence lists to remain scalable for large genomic sequences.

---

## 4. Viterbi Algorithm (3 marks)

- The Viterbi algorithm was implemented to decode the most likely sequence of hidden states (e.g., exons/introns) for a given observed DNA sequence.
- Parameters such as emission and transition matrices were initialized per Nature Primer.
- A helper function `get_log_prob_of_a_given_path` calculates the log-probability of a given path.
- Final output includes both the most probable path and its probability.

---

## 5. V-Plot Visualization (1 mark)

**File**: `data`

- For each line in the data, fragment and binding site centers were computed.
- X-axis: difference in centers (C2 - C1); Y-axis: fragment length; Z-axis: count.
- Output is a 2D heatmap highlighting V-shaped protection pattern from DNA-protein binding events.

---

## 6. Principal Component Analysis (2 marks)

**Files**:  
- `data/class.tsv`  
- `data/filtered.tsv.gz`  
- `data/columns.tsv.gz`

- Expression levels of XBP1 and GATA3 were extracted and visualized by patient class.
- PCA was implemented from scratch and applied to the full dataset.
- Results reproduce patterns seen in Figure 1 of the Nature Primer, including projection onto PC1.

---

All tasks use Python 3 with standard packages (`numpy`, `pandas`, `matplotlib`, `seaborn`, etc.) and are tested on datasets provided in the assignment. Each script includes comments and is designed for readability and reproducibility.

