# IDS-based-on-Lakhina-Entropy-and-FLAGS-Methods
This repository presents a project on network anomaly detection, focusing on identifying botnet activity in NetFlow data. Two complementary approaches are implemented: Lakhina Entropy and  FLAGS 


---

##  Methods

### 1. Lakhina Entropy-Based PCA

**Concept:**  
This method  uses Shannon entropy to capture irregularities in network traffic patterns. The core idea is that under normal conditions, the distribution of ports and IPs remains stable. However, botnets and intrusions disturb these distributions, leading to detectable spikes or drops in entropy values.

**Implementation:**  
We compute entropy metrics on source ports, destination ports, destination IPs, and TCP flags for each IP. These entropy values are then projected into a lower-dimensional space using Principal Component Analysis (PCA). Anomaly scores are calculated by analyzing the projections in major and minor PCA subspaces, allowing us to flag suspicious IPs.

---

### 2. FLAGS Detection Approach

**Concept:**  
FLAGS extends the Lakhina method by focusing on **TCP flag behavior** rather than just entropy. Each type of attack (e.g., SYN flood, port scan) leaves a unique fingerprint in the combination of TCP flags. By analyzing the frequency and sequencing of these flags, we can uncover subtle patterns that indicate botnet activity.

**Implementation:**  
We build normalized histograms of TCP flags per source IP and extract additional behavioral features such as SYN/FIN ratios, flag combinations, flow duration, and temporal flag entropy. PCA is used for dimensionality reduction, followed by logistic regression to classify flows as benign or malicious. A threshold optimization step is used to fine-tune detection sensitivity based on F1-score.

---

## Evaluation Strategy

To assess our system, we compare model predictions with the labeled ground truth from CTU-13. Each IP is evaluated and categorized using classic classification metrics:

- **True Positives (TP):** Correctly detected malicious IPs  
- **True Negatives (TN):** Correctly detected normal IPs  
- **False Positives (FP):** Normal IPs wrongly flagged as malicious  
- **False Negatives (FN):** Missed malicious IPs  

From these, we derive:
- **Precision**
- **Recall (TPR)**
- **False Positive Rate (FPR)**
- **Accuracy**
- **F1-Score**

---

## Dataset

The **CTU-13** dataset, developed by the Stratosphere IPS Project, contains 13 real-world-like scenarios of mixed normal and botnet traffic captured in a controlled environment.

**Key features:**
- `.binetflow` format, representing summarized NetFlow-like data
- Each record corresponds to a single network flow
- Labeled with ground-truth classes: e.g., `From-Botnet`, `To-Normal`, `From-Normal`
- Main fields:  
  - `StartTime`, `Dur` – temporal features  
  - `SrcAddr`, `DstAddr`, `Sport`, `Dport` – IP and ports  
  - `Proto` – protocol (focus on TCP)  
  - `State` – TCP flag summary  
  - `Label` – flow classification  

We mainly used **Scenarios 9 and 12** due to their rich mix of botnet behaviors (e.g., Neris botnet with port scans and click fraud) and a balanced duration of traffic.

---

## References

- García, S., Zunino, A., & Campo, M. (2014).  
  *An empirical comparison of botnet detection methods.*  
  Computers & Security, 45, 100–123.  
  [ScienceDirect Link](https://www.sciencedirect.com/science/article/pii/S0167404814000923)

- CTU-13 Dataset – [Stratosphere IPS Project](https://www.stratosphereips.org/datasets-ctu13)

---
