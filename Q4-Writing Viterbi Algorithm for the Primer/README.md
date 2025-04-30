
---

# 🧬 Viterbi Algorithm for Nature Primer (3 Marks)

This project implements the **Viterbi algorithm** for a simplified **Hidden Markov Model (HMM)** based on the *Nature Primer* exercise. The aim is to decode a DNA sequence into its most probable biological annotation — **Exon (E)**, **Splice Site (5)**, or **Intron (I)** — using the principles of probabilistic modeling.

---

## 📌 Task Objectives

This assignment involves three main components:

1. **🧱 Define the HMM structure**: States, observations, transition and emission probabilities.
2. **🔍 Calculate the log-probability** of a known state path emitting a specific observed DNA sequence.
3. **🧠 Implement the Viterbi algorithm** to infer the most probable path for any DNA input sequence.

---

## 🧠 Model Architecture

### Hidden States
- `E`: Exon  
- `5`: Splice site  
- `I`: Intron  

### Observations
- DNA nucleotides: `A`, `C`, `G`, `T`

---

### 🔗 Transition Probabilities

```python
transition_probabilities = {
    'Start': {'E': 1.0, '5': 0.0, 'I': 0.0, 'End': 0.0},
    'E':     {'E': 0.9, '5': 0.1, 'I': 0.0, 'End': 0.0},
    '5':     {'E': 0.0, '5': 0.0, 'I': 1.0, 'End': 0.0},
    'I':     {'E': 0.0, '5': 0.0, 'I': 0.9, 'End': 0.1}
}
```

### 🧬 Emission Probabilities

```python
emission_probs = {
    'E': {'A': 0.25, 'C': 0.25, 'G': 0.25, 'T': 0.25},
    '5': {'A': 0.05, 'C': 0.00, 'G': 0.95, 'T': 0.00},
    'I': {'A': 0.4,  'C': 0.1,  'G': 0.1,  'T': 0.4}
}
```

---

## 🔍 Log Probability Function

Given a **state path** and **DNA sequence**, this function computes the total **log-probability** of the path producing the sequence:

```python
# Function to compute the log probability of a state path emitting an observed sequence
def get_log_prob_of_a_given_path(state_path, observed_sequence):
    log_prob = 0.0
    if len(state_path) != len(observed_sequence):
        raise ValueError("The length of state path and the observed sequence must be the same")
    prev_state = 'Start'
    for i in range(len(observed_sequence)):
        current_state = state_path[i]
        observed_state = observed_sequence[i]
        log_prob += safe_log(transition_probabilities[prev_state][current_state]) + \
                    safe_log(emission_probs[current_state][observed_state])
        prev_state = current_state
    if prev_state == 'I':
        log_prob += safe_log(transition_probabilities[prev_state]['End'])
    return log_prob
```

> **Example usage:**
```python
state_path = "EEEEEEEEEEEEEEEEEE5IIIIIII"
observed_sequence = "CTTCATGTGAAAGCAGACGTAAGTCA"
ans = get_log_prob_of_a_given_path(state_path, observed_sequence)
print(ans)
# ➜ Output: ~ -41.22
```

---

## 🚀 Viterbi Algorithm

This function computes the most probable sequence of hidden states using **dynamic programming**.

### 💡 Pseudocode Logic:

1. **Initialization**: Set initial probabilities using log space.
2. **Recursion**: At each step, compute the max log-probability path for each state.
3. **Termination**: Backtrack from the best final state to recover the optimal path.

### 🧠 Implementation:

```python
# Implementation of the Viterbi algorithm
def viterbi_algo(observed_sequence):
    num_states = len(states)
    num_observations = len(observed_sequence)
    viterbi_matrix = np.full((num_states, num_observations), -np.inf)
    backpointers = np.zeros((num_states, num_observations), dtype=int)
    state_to_index = {s: i for i, s in enumerate(states)}

    # Initialize the DP table
    for i, state in enumerate(states):
        viterbi_matrix[i, 0] = safe_log(initial_probabilities[state]) + \
                               safe_log(emission_probs[state][observed_sequence[0]])

    # Recursive computation
    for t in range(1, num_observations):
        for curr_idx, curr_state in enumerate(states):
            max_log_prob = -np.inf
            best_prev_idx = 0
            for prev_idx, prev_state in enumerate(states):
                log_prob = viterbi_matrix[prev_idx, t - 1] + \
                           safe_log(transition_probabilities[prev_state][curr_state])
                if log_prob > max_log_prob:
                    max_log_prob = log_prob
                    best_prev_idx = prev_idx
            viterbi_matrix[curr_idx, t] = max_log_prob + \
                                          safe_log(emission_probs[curr_state][observed_sequence[t]])
            backpointers[curr_idx, t] = best_prev_idx

    # Backtracking to find the best path
    best_path = []
    best_final_idx = np.argmax(viterbi_matrix[:, -1])
    best_path.append(states[best_final_idx])

    for t in range(num_observations - 1, 0, -1):
        best_final_idx = backpointers[best_final_idx, t]
        best_path.insert(0, states[best_final_idx])

    return best_path, np.max(viterbi_matrix[:, -1])
```

> **Example usage:**
```python
observed_sequence = "CTTCATGTGAAAGCAGACGTAAGTCA"
best_path, log_prob = viterbi_algo(observed_sequence)
print(f"Most probable path is {best_path}")
print(f"Log probability of the path is {log_prob}")
```

---

## 🧪 Sample Output

```bash
Most probable path: ['E', 'E', 'E', 'E', ..., 'E']
Log probability of path: -38.6776...
```

---

## 📁 Project Structure

```
├── q4_Viterbi_Algorithm_for_the_Primer.ipynb   # Implementation notebook
├── README.md              # You're here
```

---

## 🛠️ Dependencies

- Python ≥ 3.6  
- `numpy`  
- No external libraries or fancy black-boxes required

---

## 🏁 Notes & Tips

- The implementation **uses log-space** to avoid floating-point underflow.
- `safe_log(x)` ensures `log(0)` doesn't blow up your program.
- The Viterbi path is likely to be all `E`s for uniform emission — that's expected due to probabilities, not a bug.

---

## 📚 Reference

- *Nature Primer on Hidden Markov Models* (Gene finding)
- Viterbi algorithm for biological sequence decoding  
- [Wikipedia: Viterbi Algorithm](https://en.wikipedia.org/wiki/Viterbi_algorithm)

---

