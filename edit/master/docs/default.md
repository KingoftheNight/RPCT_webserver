# Default Value

In this page, you will get informations of RPCT default parameters. If you want to change them or develop personal projects, you should read this page carefully.

## Support Vector Machines (SVM)

SVM is a machine learning tool used for classification prediction, regression analysis and other research. In the study of protein function classification, we can easily construct and predict the classification model by giving the feature set of different proteins and setting the kernel function and hyperparameters. In RPCT toolkit, we integrated the [LIBSVM](https://www.csie.ntu.edu.tw/~cjlin/libsvm/) developed by professor **Lin Zhiren** of National Taiwan University, the version is **LIBSVM-3.24**.

In RPCT toolkit, we set the type of SVM is **C-SVC**, and the kernel is **RBF** function. And the **source codes** located in *TrainFP.py* program in rpct folder.

* `-s 0 -t 2`

In addition, for hyperparameter optimization, we use the *grid.py* program based on grid search provided by **LIBSVM**.

 When you evaluate your models by k-fold cross-validation, we use the uniform data splitting method by default instead of the data splitting method provided by SVM. Because it can make the data more harmonious and get better prediction results.

## BLAST+

Blast is one of the important tools of NCBI. It can quickly compare similar protein sequences, and its subroutine psi-blast can score the disability mutation of each amino acid according to the conservativeness of the site, and generate a **Position-Specific Scoring Matrix** (PSSM). Blast provides users with a local version of the program BLAST+, allowing users to freely use the functions of blast. In RPCT toolkit, we integrated the [BLAST+](https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/), the version is **BLAST-2.11.0**.

In RPCT toolkit, we only use the makeblastdb program and psiblast program, and we set the input file format to default to prot (protein).

## ROC

In RPCT toolkit, we use sklearn package to draw the ROC-curve. We alse set the type of SVM is **C-SVC**, and the kernel is **RBF** function. And we set the k-fold cross-validation to default to 5.

## Other

In RPCT toolkit, except for model output, all of our function's output files have default output formats. You only need to enter the file name without adding a format suffix.