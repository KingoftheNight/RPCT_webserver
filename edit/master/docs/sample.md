# Sample

In this page, we will give a complete analysis process guidance, and demonstrate the use of the RPCT toolkit through a little sample dataset.

## Linux

### 1. View RPCT-GUI and Structure

RPCT toolkit consists of two startup programs, an instruction file and a program folder. 

    $ ls

<center>![linux-1](img/linux-1.png)</center>
<center>**Fig1.1.** RPCT toolkit</center>

The rpct folder is the core folder of RPCT toolkit, and you'd better not change it arbitrarily to avoid program bugs. All programs and database were saved in rpct folder. 

    $ cd rpct
    $ ls

![linux-2](img/linux-2.png)
<center>**Fig1.2.** rpct folder</center>

***/aaindexDB*** : *contains the latest AAindex database information*;
***/bin*** : *contains windows icons, software history, etc*;
***/blastDB*** : *contains blast databases (before you build the database, it contains nothing)*;
***/libsvm-3.24*** : *contains the core programs of libsvm-3.24*;
***/raacDB*** : *contains the book of reduce amino acid codes (raaCODE : contains 627 different raacs)*.

![downloadunzip-1-sup](img/downloadunzip-1-sup.png)
<center>**Fig1.3.** RPCT toolkit Structure</center>

You can search RPCT function through following command.

    $ cd ../
    $ python RPCT_linux.py -h

![linux-3](img/linux-3.png)
<center>**Fig1.4.** Search RPCT functions</center>

Or you can view Precautions for all commands and their parameters through following command (Press Q to return).

    $ less README.md

![linux-4](img/linux-4.png)
<center>**Fig1.5.** View Precautions</center>

### 2. Start a new project

#### Datasets

You can download the test datasets on [GitHub](https://github.com/KingoftheNight/Test-40/archive/refs/heads/main.zip) or by following commands. We provide a pair of small protein datasets which contain 20 DNA-binding proteins and 20 non-binding proteins. You should save them in RPCT main program directory.

    $ wget https://github.com/KingoftheNight/Test-40/archive/refs/heads/main.zip --no-check-certificate
    $ unzip main.zip
    $ ls

![linux-5](img/linux-5.png)
![linux-6](img/linux-6.png)
<center>**Fig1.6.** Download test datasets</center>

#### Read

Choose read function, input the dataset name and out folder name as follow. Then you will get a Reads folder contains your fasta folder.

    $ python RPCT_linux.py read Test-40-main/test-p.txt -o tp
    $ ls
    $ cd Reads
    $ ls
    $ cd tp
    $ ls

![linux-read-1](img/linux-read-1.png)
<center>**Fig1.7.** Read test-p.txt</center>

In the same way, read test-n.txt and output tn subfolder.

    $ python RPCT_linux.py read Test-40-main/test-p.txt -o tp
    $ ls
    $ cd Reads
    $ ls

![linux-read-2](img/linux-read-2.png)
<center>**Fig1.8.** Read test-n.txt</center>

#### Blast

Choose blast function, input the fasta folder name, blast database, number, evaluate and out folder name as follow (You need to confirm that your database exists, and if you haven’t added any database yet, you need to view Additional functions-Check Blast Database). Then you will get a PSSMs folder contains your pssm folder. The blast result is affected by the blast database, and sometimes it may not be possible to get the target PSSM-profile.

    $ python RPCT_linux.py blast tp -db pdbaa -n 3 -ev 0.001 -o pssm-tp

![linux-blast-1](img/linux-blast-1.png)
<center>**Fig1.9.** Blast</center>

![linux-blast-2](img/linux-blast-2.png)
<center>**Fig1.10.** /PSSMs/pssm-tp</center>

In the same way, blast tn and output pssm-tn subfolder.

    $ python RPCT_linux.py blast tn -db pdbaa -n 3 -ev 0.001 -o pssm-tn

![linux-blast-3](img/linux-blast-3.png)
<center>**Fig1.11.** Blast completed</center>

#### Extract

Choose extract function, input the pssm folder name, lmda, raaCODE and out folder name as follow (**Folder mode**). Then you will get a Feature folder contains feature files with different RAACs.

    $ python RPCT_linux.py extract pssm-tp pssm-tn -raa raaCODE -o Feature -l 5
    $ cd Feature
    $ ls

![linux-extract-1](img/linux-extract-1.png)
<center>**Fig1.12.** Extract (Folder mode)</center>

![linux-extract-2](img/linux-extract-2.png)
<center>**Fig1.13.** /Feature</center>

And you can also input the pssm folder name, lmda and selfraac as follow (**Document mode**). Then you will get a feature file with your RAAC. The name of feature file default to **t1sn_rpct.fs** (n is the size of your RAAC).

    $ python RPCT_linux.py extract pssm-tp pssm-tn -l 5 -r AGST-RK-ND-C-QE-H-ILMV-FY-P-W

![linux-extract-3](img/linux-extract-3.png)
<center>**Fig1.14.** Extract (Document mode)</center>

#### Search

Choose search function, input the folder name as follow (**Folder mode**). Then you will get a Hyperparameters file. That might take some time, but you can get all Hyperparameters of your target feature files folder.

    $ python RPCT_linux.py search -f Feature

<center>![linux-search-2](img/linux-search-2.png)</center>
<center>**Fig1.15.** Search (Folder mode)</center>

If you want to search only one file, you can also input the feature file nameas follow (**Document mode**). Then you will get the C_number and Gamma.

    $ python RPCT_linux.py search -d t1s10_rpct.fs

<center>![linux-search-1](img/linux-search-1.png)</center>
<center>**Fig1.16.** Search (Document mode)</center>

#### Evaluate

Choose eval function, input the folder name, cross-validation fold, out folder name and cg file name as follow (**Folder mode**). Then you will get a evaluate-result folder.

    $ python RPCT_linux.py eval -f Feature -cg Hyperparameters.txt -cv 5 -o Eval_fs

![linux-eval-1](img/linux-eval-1.png)
<center>**Fig1.17.** Evaluate (Folder mode)</center>

    $ ls
    $ cd Eval_fs
    $ ls

![linux-eval-2](img/linux-eval-2.png)
<center>**Fig1.18.** /Eval_fs</center>

And you can also input the single feature file name, C_number, Gamma, cross-validation fold and out file name as follow (**Document mode**). Then you will get a evaluate result file with your target feature model.

    $ python RPCT_linux.py eval -d t1s10_rpct.fs -c 0.03125 -g 0.0078125 -cv 5 -o t1s10

![linux-eval-3](img/linux-eval-3.png)
<center>**Fig1.19.** Evaluate (Document mode)</center>

    $ less t1s10-eval.txt
    $ q

![linux-eval-4](img/linux-eval-4.png)
<center>**Fig1.20.** t1s10-eval.txt</center>

#### Filter

Choose filter function, input the feature file name, C_number, Gamma, cross-validation fold, random sampling times, out file name, RAAC book name and reduce  ass follow. Then you will get a filter-result folder.

    $ python RPCT_linux.py filter Feature/t19s8_rpct.fs -c 32 -g 0.001953125 -cv 5 -o t19s8 -r 10 -raac raaCODE -t t19s8

![linux-filter-1](img/linux-filter-1.png)
<center>**Fig1.21.** Filter</center>

![linux-filter-2](img/linux-filter-2.png)
<center>**Fig1.22.** /t19s8</center>

#### Filter-feature-file set

Choose fffs function, input the feature file name, IFS file name, end feature number and out file name as follow. Then you will get a filter-feature-file.

    $ python RPCT_linux.py fffs Feature/t19s8_rpct.fs -f t19s8/Fsort-ifs.txt -n 436 -o t19s8_436
    $ ls

![linux-fffs-1](img/linux-fffs-1.png)
<center>**Fig1.23.** FFFs</center>

#### ROC

Choose roc function, input the feature file name, out file name, C_number and Gamma as follow. Then you will get a roc curve graph.

    $ python RPCT_linux.py roc Feature/t19s8_rpct.fs -c 32 -g 0.001953125 -o t19s8

![linux-roc-1](img/linux-roc-1.png)
<center>**Fig1.24.** ROC</center>


#### Train

Choose train function, input the folder name, out folder name and cg file name as follow (**Folder mode**). Then you will get a model folder.

    $ python RPCT_linux.py train -f Feature -cg Hyperparameters.txt -o Model

![linux-train-2](img/linux-train-2.png)

![linux-train-1](img/linux-train-1.png)
<center>**Fig1.25.** Train (Folder mode)</center>

You can also input the feature file name, C_number, Gamma and out file name as follow (**Document mode**). Then you will get a feature model.

    $ python RPCT_linux.py train -d Feature/t19s8_rpct.fs -c 32 -g 0.001953125 -o t19s8.model

![linux-train-3](img/linux-train-3.png)
<center>**Fig1.26.** Train (Document mode)</center>

#### Predict

Choose predict function, input the feature file name, model name and out file name as follow. Then you will get a predict-result file and Accuracy of prediction on CMD.

    $ python RPCT_linux.py predict Feature/t19s8_rpct.fs -m t19s8.model -o t19s8

![linux-predict-1](img/linux-predict-1.png)
<center>**Fig1.27.** Predict</center>

### 3. Additional functions

#### Check Blast Database

For a new RAAC toolkit, we don't provide any blast database (Considering the inconvenience of downloading). Therefore you should download public database from [BLAST](https://ftp.ncbi.nlm.nih.gov/blast/db/) or create your own blast database. We provide two functions to help you, **Check Blast Database** and **Make Blast Database**. If you download the formatted database, you can pass these two steps. If you download the initial database (FASTA format), you should check your database before you make it.

    $ wget https://ftp.ncbi.nlm.nih.gov/blast/db/FASTA/pdbaa.gz --no-check-certificate
    $ ls
    $ gunzip pdbaa.gz
    $ ls

![linux-cbd-1](img/linux-cbd-1.png)
<center>**Fig1.28.** Download blast database</center>

    $ python RPCT_linux.py checkdb pdbaa
    $ ls

![linux-cbd-2](img/linux-cbd-2.png)
<center>**Fig1.29.** Check Blast Database</center>

#### Make Blast Database

You can make your own blast database by this function. And you should check it before you make.

    $ python RPCT_linux.py makedb ND_pdbaa -o pdbaa

<center>![linux-makedb-1](img/linux-makedb-1.png)</center>
<center>**Fig1.30.** Make Blast Database</center>

<center>![linux-makedb-2](img/linux-makedb-2.png)</center>
<center>**Fig1.31.** /rpct/blastDB</center>

#### Set Hyperparameters File

You can set the Hyperparameters file according to your experience (Considering that not all grid hyperparameters can get the best prediction model, and it takes too long time to grid the hyperparameters in folder mode).

    $ python RPCT_linux.py mhys Feature -c 8 -g 0.125 -o Hys_1.txt
    $ ls

![linux-hys-1](img/linux-hys-1.png)
<center>**Fig1.32.** Set Hyperparameters File</center>

#### Integrated Learning

You can use the integrated learning function to make simple protein ensemble predictions. You need input the train features folder, predict features folder, evaluate file, hyperparameters file and model members. And the program will choose the best n feature models to join in the majority vote. If more than half of the members predict that it is a binding protein, it is finally predicted to be a binding protein. In this sample, we use the same features folder to show this function, and you should set the features folder of predict-datasets as same as train-datasets.

    $ python RPCT_linux.py intlen -tf Feature -pf Feature -ef Eval_fs/Features_eval.csv -cg Hyperparameter.txt -m 5
    $ ls

![linux-intlen-1](img/linux-intlen-1.png)
<center>**Fig1.33.** Integrated Learning</center>

#### Principal Component Analysis

You can obtain feature filder results through the principal component analysis function. We use it as a backup option for IFS-RF. It can usually get a lower feature dimension, but the heighest prediction accuracy is slightly lower than IFS-RF.

    $ python RPCT_linux.py pca Feature/t19s8_rpct.fs -c 32 -g 0.001953125 -cv 5 -o t19s8
    $ ls
    $ cd t19s8
    $ ls

![linux-pca-1](img/linux-pca-1.png)
<center>**Fig1.34.** Principal Component Analysis</center>

![linux-pca-2](img/linux-pca-2.png)
<center>**Fig1.35.** /t19s8</center>

#### Self Reduce Amino Acids Code

You can generate custom reduced amino acid codes through the self reduce amino acids code function. You need input an AAindex ID as follow (If you don't know how to get ID, please view the Additional functions-AAindex). The program will search AAindex database, which was saved in aaindexDB folder in rpct folder, to calculate the Euclidean distance of amino acid residues and out put the RAACs of your rules.

    $ python RPCT_linux.py res ARGP820103

![linux-res-1.png](img/linux-res-1.png)
<center>**Fig1.36.** Self Reduce Amino Acids Code</center>

#### View RAAC Map Of Different Type

You can view the reduced maps of raac in different types through the view RAAC map of different type function.

    $ python RPCT_linux.py view raaCODE -t 19

![linux-view-1](img/linux-view-1.png)
<center>**Fig1.37.** View RAAC Map Of Different Type</center>

#### View Sequence Reduce Weblogo

You can view the reduced weblogo of PSSM scores through the view sequence reduce weblogo function.

    $ python RPCT_linux.py weblogo PSSMs/pssm-tp/1 -raa raaCODE -r t19s8 -o t19s8

![linux-weblogo-1](img/linux-weblogo-1.png)
<center>**Fig1.38.** View Sequence Reduce Weblogo</center>

## Windows

### 1. View RPCT-GUI and Structure

RPCT toolkit consists of two startup programs, an instruction file and a program folder. 

![downloadunzip](img/downloadunzip.png)
<center>**Fig2.1.** RPCT toolkit</center>

You can open RPCT-GUI by both CMD and Anaconda Prompt through following command.

    python RPCT_windows.py

![windows-home](img/windows-home.png)
<center>**Fig2.2.** Open RPCT-GUI</center>

<center>![windows-blast-1](img/windows-blast-1.png)</center>
<center>**Fig2.3.** RPCT-GUI</center>

The RPCT-GUI contains four parts. They are Menu-Line, Title-Line, Functions-Box and Process-Box. You can complete common protein prediction tasks in Functions-Box, and get more functions and services in Menu-Line. The Process-Box on the bottom of GUI can help you view commands you have completed, and you can save them before you finish your project. 

The rpct folder is the core folder of RPCT toolkit, and you'd better not change it arbitrarily to avoid program bugs. All programs and database were saved in rpct folder. 

![downloadunzip-1](img/downloadunzip-1.png)
<center>**Fig2.4.** rpct folder</center>

***/aaindexDB*** : *contains the latest AAindex database information*;
***/bin*** : *contains windows icons, software history, etc*;
***/blastDB*** : *contains blast databases (before you build the database, it contains nothing)*;
***/libsvm-3.24*** : *contains the core programs of libsvm-3.24*;
***/raacDB*** : *contains the book of reduce amino acid codes (raaCODE : contains 627 different raacs)*.

![downloadunzip-1-sup](img/downloadunzip-1-sup.png)
<center>**Fig2.5.** RPCT toolkit Structure</center>

### 2. Start a new project

#### Datasets

You can download the test datasets on [GitHub](https://github.com/KingoftheNight/Test-40/archive/refs/heads/main.zip). We provide a pair of small protein datasets which contain 20 DNA-binding proteins and 20 non-binding proteins. You should save them in RPCT main program directory.

![windows-test](img/windows-test.png)
<center>**Fig2.6.** Download test datasets</center>

#### Read

Choose read function, input the dataset name and out folder name, click run to import test datasets. Then you will get a Reads folder contains your fasta folder.

* file name : `test-p.txt`
* out name : `tp`

![windows-read-1](img/windows-read-1.png)
<center>**Fig2.7.** Read</center>

![windows-read-2](img/windows-read-2.png)
<center>**Fig2.8.** /Reads/tp</center>

In the same way, read test-n.txt and output tn subfolder.

* file name : `test-n.txt`
* out name : `tn`

![windows-read-3](img/windows-read-3.png)
<center>**Fig2.9.** Read completed</center>

#### Blast

Choose blast function, input the fasta folder name, blast database, number, evaluate and out folder name, click run to psiblast (You need to confirm that your database exists, and if you haven’t added any database yet, you need to view Additional functions-Check Blast Database). Then you will get a PSSMs folder contains your pssm folder. The blast result is affected by the blast database, and sometimes it may not be possible to get the target PSSM-profile.

* folder : `tp`
* database : `pdbaa`
* number : `3`
* evaluate : `0.001`
* out : `pssm-tp`

![windows-blast-3](img/windows-blast-3.png)
<center>**Fig2.10.** Blast</center>

![windows-blast-4](img/windows-blast-4.png)
<center>**Fig2.11.** /PSSMs/pssm-tp</center>

In the same way, blast tn and output pssm-tn subfolder.

* folder : `tn`
* database : `pdbaa`
* number : `3`
* evaluate : `0.001`
* out : `pssm-tn`

![windows-blast-5](img/windows-blast-5.png)
<center>**Fig2.12.** Blast completed</center>

#### Extract

Choose extract function, input the pssm folder name, lmda, raaCODE and out folder name, click run to extract (**Folder mode**). Then you will get a Feature folder contains feature files with different RAACs.

* positive folder : `pssm-tp`
* negative folder : `pssm-tn`
* lmda : `5`
* raaCODE : `raaCODE`
* out : `Feature`
* selfraac : `-`

![windows-extract-1](img/windows-extract-1.png)
<center>**Fig2.13.** Extract (Folder mode)</center>

![windows-extract-2](img/windows-extract-2.png)
<center>**Fig2.14.** /Feature</center>

And you can also input the pssm folder name, lmda and selfraac, click run to extract (**Document mode**). Then you will get a feature file with your RAAC. The name of feature file default to **t1sn_rpct.fs** (n is the size of your RAAC).

* positive folder : `pssm-tp`
* negative folder : `pssm-tn`
* lmda : `5`
* raaCODE : `-`
* out : `-`
* selfraac : `AGST-RK-ND-C-QE-H-ILMV-FY-P-W`

![windows-extract-3](img/windows-extract-3.png)
<center>**Fig2.15.** Extract (Document mode)</center>

#### Search

Choose search function, input the folder name, click run to search Hyperparameters (**Folder mode**). Then you will get a Hyperparameters file. That might take some time, but you can get all Hyperparameters of your target feature files folder.

* document name : `-`
* folder name : `Feature`

![windows-search-2](img/windows-search-2.png)
<center>**Fig2.16.** Search (Folder mode)</center>

If you want to search only one file, you can also input the feature file name, click run to search Hyperparameters (**Document mode**). Then you will get the C_number and Gamma from CMD.

* document name : `t1s10_rpct.fs`
* folder name : `-`

![windows-search-1](img/windows-search-1.png)
<center>**Fig2.17.** Search (Document mode)</center>

#### Evaluate

Choose eval function, input the folder name, cross-validation fold, out folder name and cg file name, click run to evaluate feature models (**Folder mode**). Then you will get a evaluate-result folder.

* file : `-`
* folder : `Feature`
* c : `-`
* g : `-`
* cv : `5`
* out : `Eval_fs`
* cg : `Hyperparameters.txt`

![windows-eval-2](img/windows-eval-2.png)
<center>**Fig2.18.** Evaluate (Folder mode)</center>

![windows-eval-3](img/windows-eval-3.png)
<center>**Fig2.19.** /Eval_fs</center>

![eval-plot](img/eval-plot.png)
<center>**Fig2.20.** Evaluate result (Folder mode)</center>

And you can also input the single feature file name, C_number, Gamma, cross-validation fold and out file name, click run to extract (**Document mode**). Then you will get a evaluate result file with your target feature model.

* file : `t1s10_rpct.fs`
* folder : `-`
* c : `2048`
* g : `0.0001220703125`
* cv : `5`
* out : `t1s10`
* cg : `-`

![windows-eval-1](img/windows-eval-1.png)
<center>**Fig2.21.** Evaluate (Document mode)</center>

![eval-plot-2](img/eval-plot-2.png)
<center>**Fig2.22.** Evaluate result (Document mode)</center>

#### Filter

Choose filter function, input the feature file name, C_number, Gamma, cross-validation fold, random sampling times, out file name, RAAC book name and reduce type, click run to filter features. Then you will get a filter-result folder.

* file name : `Feature\t19s8_rpct.fs`
* c : `32`
* g : `0.001953125`
* cv : `5`
* r : `10`
* out : `t19s8`
* reduce : `raaCODE`
* type : `t19s8`

![windows-filter-1](img/windows-filter-1.png)
<center>**Fig2.23.** Filter</center>

![windows-filter-2](img/windows-filter-2.png)
<center>**Fig2.24.** /t19s8</center>

![filter-plot](img/filter-plot.png)
<center>**Fig2.25.** Filter result</center>

#### Filter-feature-file set

Choose fffs function, input the feature file name, IFS file name, end feature number and out file name, click run to set filter-feature-file. Then you will get a filter-feature-file.

* file name : `Feature\t19s8_rpct.fs`
* IFS file name : `t19s8\Fsort-ifs.txt`
* end : `436`
* out : `t19s8_436`

![windows-fffs-1](img/windows-fffs-1.png)
<center>**Fig2.26.** FFFs</center>

#### ROC

Choose roc function, input the feature file name, out file name, C_number and Gamma, click run to draw ROC curve. Then you will get a roc curve graph.

* file name : `Feature\t19s8_rpct.fs`
* out : `t19s8`
* c : `32`
* g : `0.001953125`

![windows-roc-1](img/windows-roc-1.png)
<center>**Fig2.27.** ROC</center>

![roc-plot](img/roc-plot.png)
<center>**Fig2.28.** ROC result</center>

#### Train

Choose train function, input the folder name, out folder name and cg file name, click run to train feature models (**Folder mode**). Then you will get a model folder.

* file : `-`
* folder : `Feature`
* c : `-`
* g : `-`
* out : `Model`
* cg : `Hyperparameters.txt`

![windows-train-2](img/windows-train-2.png)
<center>**Fig2.29.** Train (Folder mode)</center>

![windows-train-3](img/windows-train-3.png)
<center>**Fig2.30.** /Model</center>

You can also input the feature file name, C_number, Gamma and out file name, click run to train single feature model (**Document mode**). Then you will get a feature model.

* file : `Feature\t19s8_rpct.fs`
* folder : `-`
* c : `32`
* g : `0.001953125`
* out : `t19s8.model`
* cg : `-`

![windows-train-1](img/windows-train-1.png)
<center>**Fig2.31.** Train (Document mode)</center>

#### Predict

Choose predict function, input the feature file name, model name and out file name, click run to predict. Then you will get a predict-result file and Accuracy of prediction on CMD.

* file name : `Feature\t19s8_rpct.fs`
* model name : `t19s8.model`
* out : `t19s8`

![windows-predict-1](img/windows-predict-1.png)
<center>**Fig2.32.** Predict</center>

### 3. Additional functions

#### Check Blast Database

For a new RAAC toolkit, we don't provide any blast database (Considering the inconvenience of downloading). Therefore you should download public database from [BLAST](https://ftp.ncbi.nlm.nih.gov/blast/db/) or create your own blast database. We provide two functions to help you, Check Blast Database and Make Blast Database. If you download the formatted database, you can pass these two steps. If you download the initial database (FASTA format), you should check your database before you make it.

![windows-blast-2](img/windows-blast-2.png)
<center>**Fig2.33.** Download blast database</center>

![windows-makedb-4](img/windows-makedb-4.png)
<center>**Fig2.34.** File-Check blast database</center>

#### Edit RAAC Database

You can view and edit the RAAC books by this function. And click OK to save your edition.

<center>![menu-edit-1](img/menu-edit-1.png)</center>
<center>**Fig2.35.** File-Edit RAAC Database</center>

<center>![menu-edit-2](img/menu-edit-2.png)</center>

<center>![menu-edit-3](img/menu-edit-3.png)</center>
<center>**Fig2.36.** Edit RAAC Database</center>

#### Make Blast Database

You can make your own blast database by this function. And you should check it before you make.

![windows-makedb-1](img/windows-makedb-1.png)
<center>**Fig2.37.** File-Make Blast Database</center>

![windows-makedb-3](img/windows-makedb-3.png)
<center>**Fig2.38.** Make Blast Database (without check)</center>

![windows-makedb-5](img/windows-makedb-5.png)
<center>**Fig2.39.** Check blast database (check)</center>

#### Save Operation Process

You can save your operation process to RPCT History document.

![menu-save-2](img/menu-save-2.png)
<center>**Fig2.40.** File-Save Operation Process</center>

#### Set Hyperparameters File

You can set the Hyperparameters file according to your experience (Considering that not all grid hyperparameters can get the best prediction model, and it takes too long time to grid the hyperparameters in folder mode).

<center>![menu-set-1](img/menu-set-1.png)</center>
<center>**Fig2.41.** File-Set Hyperparameters File</center>

![menu-set-2](img/menu-set-2.png)
<center>**Fig2.42.** Set Hyperparameters File</center>

#### Integrated Learning

You can use the integrated learning function to make simple protein ensemble predictions. You need input the train features folder, predict features folder, evaluate file, hyperparameters file and model members. And the program will choose the best n feature models to join in the majority vote. If more than half of the members predict that it is a binding protein, it is finally predicted to be a binding protein. In this sample, we use the same features folder to show this function, and you should set the features folder of predict-datasets as same as train-datasets.

* train features : `Feature`
* predict features : `Feature`
* evaluate file : `Eval_fs\Features_eval.csv`
* cg file : `Hyperparameters.txt`
* member : `5`

![menu-intl-1](img/menu-intl-1.png)
<center>**Fig2.43.** Tools-Integrated Learning</center>

![menu-intl-1](img/menu-intl-2.png)
<center>**Fig2.44.** /Intlen_predict</center>

#### Principal Component Analysis

You can obtain feature filder results through the principal component analysis function. We use it as a backup option for IFS-RF. It can usually get a lower feature dimension, but the heighest prediction accuracy is slightly lower than IFS-RF.

* features file : `Feature\t19s8_rpct.fs`
* out : `t19s8`
* c_number : `32`
* g : `0.001953125`
* cv : `5`

![menu-pca-1](img/menu-pca-1.png)
<center>**Fig2.45.** Tools-Principal Component Analysis</center>

![menu-pca-2](img/menu-pca-2.png)
<center>**Fig2.46.** /t19s8</center>

#### Self Reduce Amino Acids Code

You can generate custom reduced amino acid codes through the self reduce amino acids code function. You need input an AAindex ID (If you don't know how to get ID, please view the Additional functions-AAindex) and click run. The program will search AAindex database, which was saved in aaindexDB folder in rpct folder, to calculate the Euclidean distance of amino acid residues and out put the RAACs of your rules.

* property : `ARGP820103`

![menu-res-2](img/menu-res-2.png)
<center>**Fig2.47.** Tools-Self Reduce Amino Acids Code</center>

#### View RAAC Map Of Different Type

You can view the reduced maps of raac in different types through the view RAAC map of different type function.

* RAAC book : `raaCODE`
* type : `19`

![menu-view-2](img/menu-view-2.png)
<center>**Fig2.48.** Tools-View RAAC Map Of Different Type</center>

#### View Sequence Reduce Weblogo

You can view the reduced weblogo of PSSM scores through the view sequence reduce weblogo function.

* PSSM file : `PSSMs\pssm-tp\1`
* RAAC Book : `raaCODE`
* Reduce Type : `t19s8`
* out : `t19s8`

![menu-web-2](img/menu-web-2.png)
<center>**Fig2.49.** Tools-View Sequence Reduce Weblogo</center>

#### AAindex

You can browse the brief information of the AAindex database through the AAindex function. If you want to view the AAindex database in detail, you can log in to [AAindex](https://www.genome.jp/aaindex/) or view AAindex_detail.txt in the aaindexDB subfolder under the rpct folder. And if you want to edit or add aa property, you can edit AAindex.txt in the aaindexDB subfolder under the rpct folder (Please make sure the format).

<center>![menu-aaindex-1](img/menu-aaindex-1.png)</center>
<center>**Fig2.50.** Help-AAindex</center>

<center>![menu-aaindex-2](img/menu-aaindex-2.png)</center>
<center>**Fig2.51.** AAindex list</center>

#### Author Information

<center>![menu-help-1](img/menu-help-1.png)</center>
<center>**Fig2.52.** Help-Author Information</center>

#### Precautions

<center>![menu-help-2](img/menu-help-2.png)</center>
<center>**Fig2.53.** Help-Precautions</center>