## Overview

This project investigates genetic variants on chromosome 18 associated with early-onset pancreatic cancer (EOPC) using germline variant data from 473 EOPC patients and 891 cancer-free controls. The goal was to identify genetic risk factors, perform quality control (QC), conduct logistic regression–based association analysis, and visualize significant signals.

## Imports

All required Python libraries for analysis were loaded, including pandas, numpy, seaborn, matplotlib, statsmodels, and scipy.

## Data Description

Two datasets were provided:

Germline Variant File (candidate_EOPC_variants.allele_counts.tsv.gz)
Contains genotype information for the top 3,364 variants.

Phenotype File (phenotypes.tsv.gz)
Includes participant metadata: sample_id, pancreas_cancer, age, sex, bmi, ancestry, smoking_history, and principal components (PC1–PC5).

## Data QC & Preprocessing
PCA (Population Structure)

PCA was performed to visualize genetic population structure and ensure ancestry differences do not confound associations.
PC1–PC5 were included as covariates to adjust for population stratification and reduce false associations.

Missing Values

Age, BMI, and smoking history contained some missing values.
Missingness was low, distributions were reasonable, and no samples were removed.
If categorical covariates were missing, an “unknown” category would be introduced.

Outlier Detection

Boxplots were used to inspect continuous covariates.
No biologically implausible outliers were detected.
No samples were excluded—logistic regression is robust to moderate deviations.

## Genotype Data QC
Missing Rate

Variant-level missingness was calculated using call-rate thresholds.
Variants with high missingness were removed.
3,178 high-quality variants remained after filtering.

Rare Variants

Minor allele frequency (MAF) was calculated for all variants.
Variants with MAF < 1% were removed to maintain power.
57.8% of variants with MAF ≥ 1% were retained.

Hardy–Weinberg Equilibrium (HWE)

HWE filtering was not applied because these variants came from a targeted CRISPR-based screen and not a genome-wide panel.

Merging Genotypes & Phenotypes

Genotype data and phenotype data were merged on sample_id.
The final dataset includes genotype counts (0/1/2), phenotype labels, and covariates (age, sex, BMI, smoking history, ancestry PCs).

## Association Analysis

Logistic regression was run per variant on chromosome 18 to evaluate associations with EOPC.
Covariates included:age, sex, BMI, smoking history, PC1–PC5
Each model provided: β coefficient, Standard error, Odds ratio, p-value, Multiple testing correction used FDR (Benjamini–Hochberg)

Two variants showed strong associations: chr18_26157915_C_T — genome-wide significant, OR = 2.85, p = 6.9 × 10⁻¹³

chr18_45877886_C_T — near genome-wide significance, OR = 1.70, p = 5.5 × 10⁻⁸

Variants with β > 0 (OR > 1) indicate increased risk; β < 0 (OR < 1) indicate protective effects.

## Results Summary

For each variant, the output table includes: Chromosome 18 position and alleles, β coefficient, Standard error, Odds ratio (OR), Effect direction, Raw p-value, FDR-adjusted p-value, −log10(p) for visualization

## Visualization & Interpretation
Manhattan Plot

Displays signal strength across chromosome 18.
Two major peaks correspond to the strongest association signals.

QQ Plot

Observed p-values follow the expected null line closely, demonstrating good control of population structure and minimal inflation.
