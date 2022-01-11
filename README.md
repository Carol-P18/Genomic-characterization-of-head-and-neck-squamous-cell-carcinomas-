# Genomic-characterization-of-head-and-neck-squamous-cell-carcinomas-
Analysis of the TCGA Head and Neck Squamous Carcinoma (HNSC) dataset of RNA-seq expression profiles partially described in Lawrence et al., Nature 2015


#1. Data Load & Preprocessing
We first load the CPM (count per million)-normalized data

#2.Pathway Enrichment Analysis
carry out geneset enrichment analysis with respect to the phenotype “g1 vs. g3” using the hallmark geneset compendium from MSigDB. Save the results object in the file final.exercise1.RDS

#3.read.gmt function
Let us define a simple read.gmt function to … read a ‘.gmt’ file

#4.Preparing the data
Next, extract the samples corresponding to grades g1 and g3 only. Also log2-transform the data

#5.Compute gene ranking based on t.test
Finally, sort the genes by the t.test statistic and test for enrichment with respect to each of the 50 Hallmark genesets by ksGenescore
Note that the hallmark gene set names are formatted as HGNC symbols, while the default names in the expression set and subsequent t-test output are Ensembl identifiers. The HGNC symbols in the expression set can be found in the feature data of the expression set, fData(g1vsg3).

#The final steps of the analysis (assuming you saved the output of the multiple calls to ksGenescore into an object named KShall) are as follows. Note that it is note necessary to replicate the results exactly, just make sure the results are formatted similarly.

#7.Gene Filtering
Next, filter out genes with mostly zero counts. We perform zero filtering by ‘recycling’ the script defined in the Rmodule_DiffanalRNAseq module

#8.Exercise 2: Testing for Normality of Gene Distributions
Next, we test the normality of the CPM-normalized data using ks.test (remember that the data is log2-transformed).

In this analysis, you are asked to test each gene for its deviation from normality by ks.test, and store the results in a data.frame to be saved to final.exercise2.RDS

#9. Clustering
Hierarchical Clustering
Next perform hierarchical clustering of the log2-transformed, CPM-normalized data (CPM1) on both samples/columns and genes/rows (see Rmodule_hclust.Rmd).

For this task, let us first drastically filter the genes, say, to ~2000, to make the clustering faster
Next perform hierarchical clustering based on hclust with ward.D as the agglomeration rule. You will have to choose the proper distance for each dimension

#10. plot the clustered heatmap

#11. Exercise 3: Testing for Sample Cluster Enrichment
You are now asked to answer the following question: if we split the sample dendrogram into three (3) main clusters, are any of these enriched with respect to grade or stage?

To do so, you will be asked to cut the dendrogram using the cutree function, and to compare the resulting cluster membership with stage and grade by fisher.test.

Below, we show the same heatmap with the addition of a sample color coding based on cluster membership

You now are asked to test for association (or enrichment) of C3 membership with grade and stage by Fisher test

Below, we show the contingency table, and the summary of the corresponding Fisher test for grade. Notice that the enrichment is highly significant (although not perfect), and that each cluster has an over-representation of one of the three grade categories (“AE”, “g1”, or “g3”)

Perform the same test for stage.

Then, for both grade and stage, determine which cluster is most enriched for which grade and stage category, and store your answers in the following two lists (to be filled manually), to be saved as ‘.RDS’ objects.

#12. Exercise 4: Compare Clustering Results w/ and w/o Optimal Leaf Ordering
As discussed, hierarchical clustering induces a partial ordering of the dendogram leaves (i.e., of the clustered items), modulo the ‘flipping’ of any of the sub-trees. However, one can obtain a total ordering by using the leaf-ordering algorithm developed by Bar-Joseph et al., (2001), which minimizes the distance betwees adjacent items in two distinct sub-trees (see also Rmodule_hclust.Rmd).

Now, perform hierarchical clustering based on hcopt instead, which is a simple wrapper to add optimal leaf ordering to the hclust <<<<<<< HEAD output

#13.In this exercise you are asked to:

compute the distance between every pair of adjacent samples in the ordered dendrogram returned by hclust.
compute the distance between every pair of adjacent samples in the ordered dendrogram returned by hcopt.
compare the distribution of distances between the two

#14. Classification 
Exercise 5: Build and Compare Classifiers
Finally, you will be asked to build a classifier of grade (g1 vs. g3). Use CPM2 (the 2000-gene dataset defined above) for this task.

Feature Selection

For this task we will further narrow down the set of genes to use as predictors to those most highly differentially expressed between cancer grades. To this end, we define a function, featureSelect, which makes use of differential gene expression analysis performed by the limma package. This will determine the most differentially expressed genes via a two-sample t-test (balanced or unbalanced). The function returns a list with the first item corresponding to the expressionSet limited to the selected genes/features

Armed with this function, you are next asked to build and evaluate classifiers of “g1 vs. g3”. In particular,

Split the dataset into a 60/40 stratified pair of datasets. Use the 60% portion as the discovery (or training) set and the 40% portion as the validation (or test) set. Set the random number generator seed before performing the split, so as to be able to replicate it, if necessary.

Evaluate, by 10-fold cross validation, classifiers based on 20, 50, 100, and 500 balanced features in the training (using the function featureSelect defined above). Use featureSelect to select features on the training set, prior to running cross validation. Note that this method of feature selection prior to performing cross-validation is biased, why? Use a classifier of your choice between RandomForest, Elastic Net, and SVM. You may use the caret package, as shown in class. Feel free to test more than one of the three classifiers, if you wish. Report the summary table with the performance of the different number-of-features/classifiers in terms of area under the ROC curve (AUC), sensitivity, and specificity.

Select the best classifier/number-of-features based on the AUC and apply it to the validation set. Report AUC, accuracy, sensitivity, specificity, and confusion matrix.
