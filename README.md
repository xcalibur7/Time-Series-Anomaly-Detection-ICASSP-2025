# ICASSP 2025: Multivariate Time Series Data Mining for Failure Prediction & Root Cause Analysis

Supportive repository for the following paper accepted in ICASSP 2025.

**Paper Title:** Multivariate Time Series Data Mining for Failure Prediction \& Root Cause Analysis

## Abstract

This paper presents an analysis of a large-scale,
high-dimensional industrial dataset containing over 2 million
data points collected over several months. The dataset includes
more than 200 failures of various types, each resulting from
complex causes. Utilizing state-of-the-art unsupervised multivariate anomaly detection algorithms, a system was developed
that predicts failures several minutes in advance, introducing
a new evaluation metric termed lead time. This system also
identifies the location and potential causes of these failures.
Initial application of current anomaly detection algorithms
yielded low F1 scores, due to either low recall or low precision.
To address this issue, a visual reasoning framework was created
to reduce false positives by analyzing motifs and discords
in the top-N signals contributing to the anomaly. We find
that the human-in-the-loop approach enhances the precision
and F1 score of multivariate algorithms by leveraging human
judgment for final decision-making. After several weeks of deployment, the system has been delivering highly accurate real-time alerts, resulting in enhanced productivity and efficiency

Here we will describe our experiments and results.

## Installation \& Usage of this Repository

1. Clone the repo

    ```sh
    git clone https://github.com/xcalibur7/Time-Series-Anomaly-Detection-ICASSP-2025.git
    ```

2. Navigate to the project directory

    ```sh
    cd Time-Series-Anomaly-Detection-ICASSP-2025
    ```

3. Install dependencies

    ```sh
    pip install -r requirements.txt
    ```


Project Link: [https://github.com/xcalibur7/Time-Series-Anomaly-Detection-ICASSP-2025](https://github.com/xcalibur7/Time-Series-Anomaly-Detection-ICASSP-2025)

## Experiments

### Dataset Used

We collected the real-time multivariate signals from the IBA acquisition server located at the Bhilai Steel Plant.

We use the following notations to identify the signals along with its type: **[Signal_ID]_Signal_Name**. Signal_ID is denoted by [xTy], where {x} defines the location of the signal in the mill, **y** defines a unique id, and T defines the type of signals. For digital signal, we use T as "." whereas for the analog signal, we use T as ':'. Signal_Name provides a descriptive identifier for the signal.

### Types of Failure

The bar and rod mill of the BSP encounters many failures, most of the failures fall into the following 4 different categories:

- **Cobble**: During this failure, metal accumulates in the process line or metal coming out of line due to equipment failure or metal encountered any hindrance during the rolling process.
- **Undershoot**: This failure occurs when the rolling metal is not discharged onto the cooling bed. The metal is stopped in twin-channel or tail-braker due to bar finding resistance, failure of equipment, or anomaly in rod dimension.
- **Overshoot**: Bar is discharged either at the extreme end of the cooling bed or outside the cooling bed entirely, leading to accumulation. This typically occurs due to a braking anomaly or when the bar length is excessively long.
- **Guide loose**: Guides become loose due to metal hitting or rubbing against them, or due to the failure of guide roller bearings. This results in resistance to the metal flow and, if not tightened periodically, the guides can be knocked out by the metal.

### Anomaly Detection Techniques

To efficiently predict the failures and understand the root-cause analysis, we first evaluate the effectiveness of various multivariate time series anomaly detection algorithms on the top 2-3 regions, which encounter many failures in the system. For this, we perform experiments on the following algorithms: PCA, GAN/LSTM, TSG, USAD. Then, we choose the best performing algorithm to identify the root-cause analysis of the failure. Finally, we remove the false positives using univariate analysis.

### Hardware and Software Platform

We used a machine equipped with an Intel(R) Xeon(R) Gold 5218 CPU @ 2.30GHz and an NVIDIA RTX A6000 GPU.

The software implementation was done using Python v3.11.4, PyTorch v2.1.2, Tensorflow v2.16.1, PostgreSQL v15.6, Metaflow v2.11.10, InfluxDb v2.7.0, and Grafana v10.0.2.

## Results

This section demonstrates impact region-wise classification of signals and shows the effectiveness of various multivariate time series anomaly detection with various metrics. Subsequently, we analyze our results in detail.

### Importance of Regionalisation

Table 1 shows the results of training and testing data sets using the PCA algorithm when used with a 10mm profile configuration. The overall training dataset has 846,720 rows and 430 columns and the testing dataset has 362,880 rows and 430 columns. The first row in the table shows the metrics for training/testing data when the algorithms are executed on _all the signals_ located across the mill. While the subsequent rows Stand_1-16, Stand_13-18, and HMD show the results of running the algorithm for each region respectively. We chose these 3 regions as they are most prone to failures. Similarly, Table 2 presents the results when used with the LSTM algorithm.

From the results, we observe that for both PCA and LSTM models, the region-wise F1-scores for Stand_1-6, Stand_13-18, and HMD region are better than that of the overall. This indicates that region-wise anomaly detection not only improves the quality of failure prediction but also helps the operator in easily identifying the failure of the region. This improves the efficiency of the operator and the productivity of maintenance. The tables also show significant reductions in training and inference times for the training and testing sets, respectively.

#### PCA 10mm Profile

| Region      | Precision | Recall | F1-Score | Training Time (s) |
|-------------|-----------|--------|----------|-------------------|
| Overall     | 0.277     | 0.383  | 0.322    | 5700              |
| Stand_1-6   | 0.333     | 0.683  | 0.448    | 3900              |
| Stand_13-18 | 0.230     | 0.783  | 0.356    | 4080              |
| HMD         | 0.243     | 0.817  | 0.374    | 3480              |

| Region      | Precision | Recall | F1-Score | Inference Time (s) |
|-------------|-----------|--------|----------|--------------------|
| Overall     | 0.287     | 0.524  | 0.371    | 60                 |
| Stand_1-6   | 0.289     | 0.652  | 0.401    | 45                 |
| Stand_13-18 | 0.270     | 0.718  | 0.393    | 50                 |
| HMD         | 0.288     | 0.603  | 0.390    | 43                 |

#### LSTM 10mm Profile

| Region      | Precision | Recall | F1-Score | Training Time (s) |
|-------------|-----------|--------|----------|-------------------|
| Overall     | 0.123     | 0.163  | 0.140    | 9000              |
| Stand_1-6   | 0.439     | 0.254  | 0.322    | 5700              |
| Stand_13-18 | 0.250     | 0.873  | 0.388    | 5880              |
| HMD         | 0.344     | 0.818  | 0.484    | 4200              |

| Region      | Precision | Recall | F1-Score | Inference Time (s) |
|-------------|-----------|--------|----------|--------------------|
| Overall     | 0.278     | 0.250  | 0.263    | 240                |
| Stand_1-6   | 0.387     | 0.218  | 0.279    | 203                |
| Stand_13-18 | 0.275     | 0.856  | 0.417    | 224                |
| HMD         | 0.352     | 0.593  | 0.441    | 220                |

### Comparison of Different Algorithms

Tables 3 and 4 show the performance comparison of the multivariate time series anomaly detection algorithms PCA, LSTM, TSG, and USAD. The results are shown for Stand_13-18 and CVAH_L1 using a 10mm profile configuration. Similarly, Tables 5 and 6 show the results considering both 10mm and 16mm configurations. The results show that LSTM has better F1-scores and lead time compared to the remaining algorithms in most cases, while PCA also performs close to LSTM.

#### Region: Stand_13-18, Profile: 10mm

| Algorithm | Precision | Recall | F1 Score | Lead Time (s) | Inference Time (s) |
|-----------|-----------|--------|----------|---------------|--------------------|
| PCA       | 0.270     | 0.717  | 0.392    | 321           | 50                 |
| LSTM      | 0.275     | 0.856  | 0.417    | 325           | 224                |
| TSG       | 0.040     | 0.100  | 0.060    | 300           | 40                 |
| USAD      | 0.250     | 0.330  | 0.280    | 180           | 120                |

#### Region: CVAH_L1, Profile: 10mm

| Algorithm | Precision | Recall | F1 Score | Lead Time (s) | Inference Time (s) |
|-----------|-----------|--------|----------|---------------|--------------------|
| PCA       | 0.223     | 0.477  | 0.304    | 295           | 55                 |
| LSTM      | 0.322     | 0.468  | 0.382    | 333           | 228                |
| TSG       | 0.150     | 0.210  | 0.170    | 300           | 50                 |
| USAD      | 0.200     | 0.230  | 0.210    | 180           | 120                |

#### Region: Stand_13-18, Profile: 10mm & 16mm combined

| Algorithm | Precision | Recall | F1 Score | Lead Time (s) | Inference Time (s) |
|-----------|-----------|--------|----------|---------------|--------------------|
| PCA       | 0.333     | 0.680  | 0.446    | 315           | 55                 |
| LSTM      | 0.392     | 0.788  | 0.523    | 345           | 224                |
| TSG       | 0.030     | 0.120  | 0.050    | 300           | 40                 |
| USAD      | 0.250     | 0.330  | 0.280    | 180           | 120                |

#### Region: CVAH_L1, Profile: 10mm & 16mm combined

| Algorithm | Precision | Recall | F1 Score | Lead Time (s) | Inference Time (s) |
|-----------|-----------|--------|----------|---------------|--------------------|
| PCA       | 0.323     | 0.677  | 0.438    | 290           | 55                 |
| LSTM      | 0.392     | 0.720  | 0.510    | 335           | 228                |
| TSG       | 0.080     | 0.210  | 0.120    | 300           | 50                 |
| USAD      | 0.200     | 0.230  | 0.210    | 180           | 120                |

### Metrics at Different Regions

Since LSTM has better F1-scores compared to other algorithms, followed by PCA, we present the results of failure prediction by comparing the top 10 regions based on the occurrence of most failures. Region-wise anomaly prediction results for both PCA and LSTM algorithms achieve better F1-scores. Region-wise failure prediction helps in early prediction of failure with good lead time, making it easier for operators to identify the fault. This reduces the cost of maintenance and helps in root-cause analysis.

#### PCA results for 10mm Profile

| Region      | Precision | Recall | F1-Score | Lead Time (s) |
|-------------|-----------|--------|----------|---------------|
| CVAH_L1     | 0.223     | 0.477  | 0.303    | 295           |
| CVAH_L2     | 0.440     | 0.202  | 0.277    | 263           |
| CVR_L1      | 0.224     | 0.588  | 0.325    | 151           |
| CVR_L2      | 0.212     | 0.750  | 0.330    | 162           |
| HMD         | 0.288     | 0.603  | 0.390    | 120           |
| PR_L1       | 0.199     | 0.743  | 0.315    | 181           |
| PR_L2       | 0.244     | 0.654  | 0.355    | 225           |
| Stand_1-6   | 0.289     | 0.652  | 0.401    | 138           |
| Stand_7-12  | 0.219     | 0.600  | 0.321    | 246           |
| Stand_13-18 | 0.270     | 0.717  | 0.392    | 321           |

#### LSTM results for 10mm Profile

| Region      | Precision | Recall | F1-Score | Lead Time (s) |
|-------------|-----------|--------|----------|---------------|
| CVAH_L1     | 0.322     | 0.468  | 0.382    | 333           |
| CVAH_L2     | 0.392     | 0.256  | 0.310    | 293           |
| CVR_L1      | 0.388     | 0.906  | 0.543    | 225           |
| CVR_L2      | 0.333     | 0.406  | 0.366    | 505           |
| HMD         | 0.352     | 0.593  | 0.441    | 153           |
| PR_L1       | 0.382     | 0.665  | 0.486    | 176           |
| PR_L2       | 0.452     | 0.845  | 0.589    | 263           |
| Stand_1-6   | 0.387     | 0.218  | 0.279    | 273           |
| Stand_7-12  | 0.393     | 0.812  | 0.530    | 740           |
| Stand_13-18 | 0.275     | 0.856  | 0.417    | 385           |

## Conclusion

This paper demonstrates that region-wise anomaly detection improves the quality of failure prediction in industrial environments. The results presented in this paper show that LSTM has better metrics compared to PCA and other algorithms. Region-wise failure prediction helps in early prediction of failure with good lead time, making it easier for operators to identify the fault. This reduces the cost of maintenance and helps in root-cause analysis.
