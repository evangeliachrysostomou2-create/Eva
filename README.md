# Zeng Aging Mouse 10Xv3 – Differential Expression Analysis

## Project overview

This project analyzes the Allen Institute Zeng Aging Mouse 10Xv3 single-cell RNA-seq dataset.

The aim is to generate a catalog of differentially expressed genes between different donor age/time points, without restricting the analysis to a specific brain region.

## Dataset

The dataset used is the **Zeng Aging Mouse 10Xv3** dataset from the Allen Institute.

Dataset documentation:
https://alleninstitute.github.io/abc_atlas_access/descriptions/Zeng_Aging_Mouse_10Xv3.html

Tutorial:
https://alleninstitute.github.io/abc_atlas_access/notebooks/Zeng_Aging_Mouse_10x_snRNASeq_tutorial.html

The expression matrix contains:

- 1,162,565 cells
- 32,285 genes

The metadata contain information about individual donors, including `donor_age` and `donor_label`.

## Software and libraries

The analysis was performed in Google Colab using Python.

Main software and libraries:

- Python
- scanpy 1.10.4
- pandas 2.2.3
- numpy
- scipy
- statsmodels
- abc_atlas_access

The Allen Institute package was installed from GitHub using:

pip install -q "abc_atlas_access[notebooks] @ git+https://github.com/AllenInstitute/abc_atlas_access.git"

The main analysis packages were installed using:

pip install -q --no-deps scanpy==1.10.4 pandas==2.2.3

## Data access

The dataset was accessed using the Allen Institute `abc_atlas_access` Python package.

The raw expression matrix used in the analysis was:

Zeng-Aging-Mouse-10Xv3/20241130/Zeng-Aging-Mouse-10Xv3-raw.h5ad

The data were loaded in backed mode to avoid loading the entire expression matrix into memory at once.

## Analysis workflow

The analysis followed these steps:

1. Load the Allen Institute cell metadata.
2. Load the raw expression matrix.
3. Match cells between the expression matrix and metadata using cell labels.
4. Identify individual donors using the donor metadata.
5. Group cells by donor.
6. Aggregate gene counts for each donor.
7. Normalize donor-level counts to counts per million (CPM).
8. Apply log1p transformation to obtain log-CPM expression values.
9. Compare gene expression between different donor age/time-point groups.
10. Perform pairwise Welch's t-tests at the donor level for each gene.
11. Apply Benjamini-Hochberg false discovery rate (FDR) correction.
12. Create a differential-expression catalog containing the age comparison, gene information, expression difference, p-value and FDR.

## Age and time-point information

The `age_1` and `age_2` columns in the differential-expression catalog correspond to values of the metadata field `donor_age`.

Examples of age/time-point labels in the dataset include:

- P53
- P54
- P55
- P56
- P57
- P58
- P59
- P60
- P61
- P62
- P64
- P65
- P66
- P67
- P68
- P69
- 9 wks
- 18M

These labels represent donor age/time-point information from the dataset.

The individual donor/sample is identified separately by `donor_label`.

Age groups with only one donor were excluded from inferential pairwise testing because a statistical comparison requires replication.

## Differential-expression catalog

The main output table contains the following columns:

age_1
age_2
gene_identifier
gene_symbol
mean_log1pCPM_difference
p_value
FDR

Each row represents one gene in one pairwise age/time-point comparison.

The `mean_log1pCPM_difference` represents the difference in mean donor-level log-CPM expression between the two compared age groups.

The `p_value` is obtained from the Welch's t-test.

The `FDR` is the Benjamini-Hochberg adjusted p-value.

## Important note about the 668-gene result

An intermediate analysis of the comparison between P53 and P54 produced 668 genes with an unadjusted p-value below 0.05.

This 668-gene result should therefore not be interpreted as the complete differential-expression catalog across all age/time points.

The final analysis contains pairwise comparisons between the available age/time-point groups, with multiple-testing correction applied separately within each comparison.

## Repository contents

- `Evangelia.ipynb` — analysis notebook containing the data processing and differential-expression analysis.
- `README.md` — documentation describing the dataset, software requirements, data access and analysis workflow.

## Reproducibility

The analysis notebook should be run in Google Colab or another Python environment with the required packages installed.

The raw Allen Institute dataset is accessed programmatically through `abc_atlas_access`; the large raw expression matrix is not stored directly in this GitHub repository.

## References

Allen Institute ABC Atlas:

https://alleninstitute.github.io/abc_atlas_access/

Allen Institute `abc_atlas_access` GitHub repository:

https://github.com/AllenInstitute/abc_atlas_access

Zeng Aging Mouse 10Xv3 dataset documentation:

https://alleninstitute.github.io/abc_atlas_access/descriptions/Zeng_Aging_Mouse_10Xv3.html
