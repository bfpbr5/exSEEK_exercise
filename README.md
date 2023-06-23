# exSEEK Project README

## Overview
This project is about exSEEK, a bioinformatics tool used for RNA-sequencing data processing, mapping, expression matrix generation, and further data analysis, normalization and machine learning applications. The project pipeline mainly involves the steps of preparing a data matrix, handling expression matrix data, and then applying feature engineering and machine learning methods.

## Prerequisites
- Docker
- fastp tool (for FASTQ preprocessor)
- hisat2 (for genomic mapping)
- samtools (for file conversion and data filtering)
- edgeR (for normalization)
- PCAtools (for PCA)
- matrixStats (for order matrix stats)
- limma (for batch effect removal)
- scikit-learn (for machine learning model)
- LIME (for explanation of machine learning models)

## Instructions

### Part-I: Prepare Data Matrix

#### Reads Processing and Mapping

1. Load Docker image for bioinformatics.

```
docker exec -it bioinfo bash
```

2. Install fastp tool.

```
wget [http://opengene.org/fastp/fastp](http://opengene.org/fastp/fastp)
chmod a+x ./fastp
```

3. Run fastp for each downloaded fq file and export quality control reports.

4. Install hisat2 tool from http://daehwankimlab.github.io/hisat2/download/

5. Build genomic index files for hisat2.

6. Map quality-controlled sequence files to the indexes files and convert .sam files to .bam files using samtools.

7. Filter unmapped reads from bam files.

### Part-II: Expression Matrix Data Processing

#### Normalization and Batch effect

1. Import data and apply normalization using TMM, TMMwsp, RLE methods, and then check normalization effect via Venn diagrams.

2. Use ComBat and limma's removeBatchEffect methods for batch effect processing.

### Part-III: Feature Engineering and Machine Learning

#### Feature Selection Based on Recursive Feature Elimination (RFE)

RFE was used as a method of feature selection, where the RFE model uses the SGDClassifier. Kernel approximation was initially attempted to improve learning accuracy, but this method did not facilitate feature selection and was subsequently abandoned. 

#### Feature Importance Based on Coefficients

LassoCV was initially used to estimate feature coefficients in order to calculate feature importance. However, in practical operation, the coefficients of most features were zero, making this method unusable.

#### Selecting Features Based on Importance

Two most important features were selected based on importance, i.e., the absolute value of the coefficients. For this purpose, the `SelectFromModel` class was used, which accepts either the threshold parameter or the number of features to be selected, and selects features whose importance (as defined by the coefficients) is higher than this threshold.

### Part-IV: Machine Learning

#### Model Construction

A variety of feature selection methods were trained on different models using a RandomForest model. 

#### Visualizing Training Results

Finally, the ROC curve for the training results was plotted. The overall result is relatively good, with the model AUC all above 0.8. However, the effect of feature selection is generally lower than that of full feature training, which may be due to the unsuitability of the selected number of features and overfitting. Further checks can be performed by comparing with the accuracy of the training set and changing the number of selected features.
