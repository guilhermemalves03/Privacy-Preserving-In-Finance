## Privacy-Preserving Computation in Finance: Homomorphic Encryption & Secure Multiparty Computation

This repository contains two complementary research projects in **Privacy-Preserving Financial Computation**:

1. **Homomorphic Encryption (HE)** - Secure portfolio analytics using **CKKS** and **BFV** schemes.
2. **Secure Multiparty Computation (SMC)** - Private Set Intersection (PSI) protocols for confidential client matching.

Together, these projects demonstrate how modern cryptographic techniques enable meaningful financial computation **without exposing sensitive data**.

---

##  Repository Overview

Financial institutions increasingly require collaboration and analytics over private data while respecting strict privacy regulations. This repository addresses that need by exploring:

- **Encrypted computation on financial values** (portfolio averages, risk metrics).
- **Private matching of sensitive identifiers** (client IDs across institutions).

Both projects focus on real-world financial use cases and provide **empirical benchmarks** on performance, scalability, and privacy guarantees.

---

##  Project 1 — Homomorphic Encryption in Finance (CKKS vs BFV)

###  Problem Statement

An investor (Data Holder) wants to compute financial statistics (e.g., average portfolio returns) using an external analytics service (Data Analyzer) **without revealing** asset values.

This is achieved using **Fully Homomorphic Encryption (FHE)**, allowing computation directly on encrypted data.

---

###  Schemes Evaluated

| Scheme | Arithmetic Type | Precision | Performance | Best Use Case |
|--------|------------------|-----------|-------------|----------------|
| **CKKS** | Approximate (real numbers) |  Approximate |  Fast | Financial analytics, ML inference, statistics |
| **BFV** | Exact (integers) |  Exact |  Slow | Small-scale integer workloads requiring exactness |

---

### 📊 Performance Results — CKKS

| Dataset Size (N) | Encryption (s) | Analysis (s) | Decryption (s) | Difference (%) | Total Time (s) |
|------------------|----------------|---------------|-----------------|----------------|----------------|
| 100              | 0.0074         | 0.0159        | 0.0025          | 0.000013       | 0.0258         |
| 1,000            | 0.0075         | 0.0401        | 0.0017          | 0.000013       | 0.0493         |
| 5,000            | 0.0175         | 0.0309        | 0.0015          | 0.000013       | 0.0499         |
| 10,000           | 0.0185         | 0.0387        | 0.0022          | 0.000013       | 0.0594         |

---

###  Performance Results — BFV

| Dataset Size (N) | Encryption (s) | Analysis (s) | Decryption (s) | Difference (%) | Total Time (s) |
|------------------|----------------|---------------|-----------------|----------------|----------------|
| 100              | 0.1600         | 0.0242        | 0.0014          | 0              | 0.1856         |
| 1,000            | 0.2010         | 0.0622        | 0.0010          | 0              | 0.2642         |
| 5,000            | 0.1340         | 0.1380        | 0.0007          | 0              | 0.2727         |
| 10,000           | 0.1963         | 0.2395        | 0.0010          | 0              | 0.4368         |

---

###  Key Insight — Homomorphic Encryption

- **CKKS** consistently outperforms BFV due to native floating-point support and vector packing.
- **BFV** introduces significant overhead due to integer scaling and chunking.
- Despite approximation, CKKS maintains error in the **fourth decimal place**, which is acceptable for financial reporting.

 **Conclusion:** CKKS is the preferred scheme for privacy-preserving financial analytics.

---

##  Project 2 — Secure Multiparty Computation (PSI in Finance)

###  Problem Statement

Two financial institutions want to identify **common clients** (e.g., for fraud detection, joint marketing, or merger analysis) **without revealing** their full client lists.

This is solved using **Private Set Intersection (PSI)** protocols under the Secure Multiparty Computation (SMC) paradigm.

---

###  Protocols Evaluated

| Protocol | Security Level | Execution Time | Data Exchange | Trust Assumptions |
|----------|----------------|----------------|----------------|-------------------|
| **Naive Hashing** |  Insecure |  Fastest |  Low | None |
| **Server-Aided** |  Weak |  Fast |  Low | Requires trusted server |
| **Diffie-Hellman-based** |  Strong |  Slowest |  High | None |
| **OT-based** |  Strong |  Moderate |  High | None |

---

###  Performance Results (Mean of 5 Runs)

####  Naive Hashing

| Dataset Size (N) | µ Time (s) | σ Time (s) | µ Data (kB) | σ Data (kB) |
|------------------|------------|------------|-------------|-------------|
| 5,000            | 0.019      | 0.002      | 90.4        | 0.894       |
| 10,000           | 0.025      | 0.003      | 182.0       | 0.000       |
| 25,000           | 0.047      | 0.012      | 452.0       | 0.000       |
| 50,000           | 0.090      | 0.015      | 904.0       | 0.000       |
| 100,000          | 0.110      | 0.027      | 1947.0      | 57.814      |

---

####  Server-Aided

| Dataset Size (N) | µ Time (s) | σ Time (s) | µ Data (kB) | σ Data (kB) |
|------------------|------------|------------|-------------|-------------|
| 5,000            | 0.180      | 0.034      | 166.0       | 0.000       |
| 10,000           | 0.118      | 0.056      | 327.2       | 1.304       |
| 25,000           | 0.133      | 0.049      | 810.6       | 2.074       |
| 50,000           | 0.255      | 0.037      | 1619.7      | 1.633       |
| 100,000          | 0.497      | 0.132      | 2557.0      | 6.403       |

---

####  Diffie-Hellman-based

| Dataset Size (N) | µ Time (s) | σ Time (s) | µ Data (kB) | σ Data (kB) |
|------------------|------------|------------|-------------|-------------|
| 5,000            | 3.682      | 0.059      | 558.4       | 27.610      |
| 10,000           | 7.177      | 0.178      | 1083.8      | 18.075      |
| 25,000           | 17.434     | 0.093      | 2582.6      | 163.344     |
| 50,000           | 34.487     | 0.144      | 4544.2      | 485.524     |
| 100,000          | 68.537     | 0.245      | 7434.2      | 1066.057    |

---

####  OT-based

| Dataset Size (N) | µ Time (s) | σ Time (s) | µ Data (kB) | σ Data (kB) |
|------------------|------------|------------|-------------|-------------|
| 5,000            | 0.339      | 0.043      | 549.6       | 0.548       |
| 10,000           | 0.351      | 0.029      | 1080.0      | 0.707       |
| 25,000           | 0.398      | 0.004      | 2634.8      | 0.447       |
| 50,000           | 0.487      | 0.018      | 4670.8      | 653.893     |
| 100,000          | 0.658      | 0.027      | 8785.0      | 1266.402    |

---

###  Key Insight — Secure Multiparty Computation

- **Naive Hashing** and **Server-Aided** are fast but fail privacy guarantees.
- **Diffie-Hellman-based** is cryptographically strong but impractically slow.
- **OT-based PSI** offers the **best compromise** between:
  - Strong privacy guarantees,
  - Moderate execution time,
  - Acceptable stability (low σ).

 **Conclusion:** OT-based PSI is the most suitable protocol for real-world financial collaboration.

---

##  Unified Perspective: Privacy-Preserving Finance

| Problem Type | Solution | Best Method |
|--------------|----------|-------------|
| Secure financial analytics | Homomorphic Encryption | **CKKS** |
| Private client matching | Secure Multiparty Computation | **OT-based PSI** |

Together, these two cryptographic paradigms provide a comprehensive toolkit for **trustworthy, compliant, and scalable financial computation**.

---

##  Installation 
- **Python 3.9+**
- TenSEAL
- NumPy, Pandas, Matplotlib



