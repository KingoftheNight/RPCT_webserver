# Function

On this page, we introduce all the functions and parameters in detail.

## 1. Check Blast Database

    database_name    # input the target fasta database, and make sure it located in the current folder.

## 2. Make Database

    database_name    # input the target fasta database, and make sure it has been checked in Check Blast Database function (1).
    -o               # input the out database name, and it will be saved in rpct/blastDB.

## 3. Read

    file_name    # input your Fasta datasets name, and you should make sure your file in the current folder.
    -o           # input the out folder name, and it will be saved by default in /Reads.

## 4. Blast

    folder_name    # input the Fasta folder name which has been created in Read function (3).
    -db            # choose the blast database, or you can make your database through Make Database function (2).
    -n             # input the iteration number.
    -ev            # input the expected value.
    -o             # input the out folder name, and it will be saved by default in PSSMs folder.

## 5. Extract

    folder_name     # input the two PSSM folders name which has been created in Blast function (4).
    -raa            # raac book saved in raacDB folder in rpct, and you can not use this parameter and -r together.
    -o              # if you choose the parameter -raa, you should input a folder name, and if you choose the parameter -r, you should input a file name.
    -l              # input the window size for the RAAC-PSSM extraction method.
    -r              # self_raa_code format should contain all amino acid types, and be separated by '-', for example: LVIMC-AGST-PHC-FYW-EDNQ-KR .

## 6. Search

    -d    # input the target feature file and output a single result of it, and you can not use this parameter and -f together.
    -f    # input the target feature folder and output the Hyperparameters file which contains all results of the feature folder, and you can not use this parameter and -d together.

## 7. Train

    -d     # input the target feature file, and you can not use this parameter with -f and -cg together.
    -f     # input the feature folder, and you can not use this parameter with -d, -c and -g together.
    -c     # the penalty coefficient of SVM, and you can not use this parameter with -f and -cg together.
    -g     # the gamma of RBF-SVM, and you can not use this parameter with -f and -cg together.
    -o     # if you choose the parameter -f, you should input a folder name, and if you choose the parameter -d, you should input a file name.
    -cg    # the Hyperparameters file which has been created in Search function (6) or defined in Set Hyperparameters File function (16), and you can not use this parameter with -d, -c and -g together.

## 8. Evaluate

    -d     # input the target feature file, and you can not use this parameter with -f and -cg together.
    -f     # input the feature folder, and you can not use this parameter with -d, -c and -g together.
    -c     # the penalty coefficient of SVM, and you can not use this parameter with -f and -cg together.
    -g     # the gamma of RBF-SVM, and you can not use this parameter with -f and -cg together.
    -o     # if you choose the parameter -f, you should input a folder name, and if you choose the parameter -d, you should input a file name.
    -cg    # the Hyperparameters file which has been created in Search function (6) or defined in Set Hyperparameters File function (16), and you can not use this parameter with -d, -c and -g together.
    -cv    # the cross validation fold of SVM, you can choose 5, 10 or -1 or define it by your experience.

## 9. ROC

    document_name    # input the target feature file.
    -c               # the penalty coefficient of SVM, you can get it through Search function (6) or define it by your experience, and you can not use this parameter with -f and -cg together.
    -g               # the gamma of RBF-SVM, you can get it through Search function (6) or define it by your experience, and you can not use this parameter with -f and -cg together.
    -o               # input the out file name, and the ROC-Cruve will be saved by defualt in current folder.

## 10. Filter

    document_name    # input the target feature file name.
    -c               # the penalty coefficient of SVM, you can get it through Search function (6) or define it by your experience.
    -g               # the gamma of RBF-SVM, you can get it through Search function (6) or define it by your experience.
    -cv              # the cross validation fold of SVM, you can choose 5, 10 or -1 or define it by your experience.
    -o               # input the out folder name.
    -r               # the random sampling number of Relief method.
    -raac            # input the raac book name.
    -t               # input the raac type and size.

## 11. Predict

    document_name    # input the target feature file, and make sure it has the same reduce type with the target model.
    -m               # input the target model file, and make sure it has the same reduce type with the target feature file.
    -o               # input the out file name, and the predict result will be saved by defualt in current folder.

## 12. Filter-feature-file set

    document_name    # input the target feature file which has been chosen in Filter function (10).
    -f               # input the Feature_Sort_File which has been created in Filter function (10).
    -n               # the stop feature number of target feature file, and you can find it in the ACC_Chart which has been created in Filter function (10).
    -o               # input the out file name.

## 13. Self Reduce Amino Acids Code

    aaindex_id    # the ID of physical and chemical characteristics in AAindex Database, and you can check it in /rpct/aaindexDB or view it online.

## 14. Integrated Learning

    -tf    # input the train feature folder name.
    -pf    # input the predict feature folder name which has been created by an independent datasets.
    -ef    # input the eval result file name which has been created by Evaluate funtion (8) and saved in eval_result folder.
    -cg    # the Hyperparameters file which has been created in Search function (6) or defined in Set Hyperparameters File function (16).
    -m     # the number of integrated-learning members.

## 15. Principal Component Analysis

    document_name    # input the feature file name.
    -c               # the penalty coefficient of SVM, you can get it through Search function (6) or define it by your experience, and you can not use this parameter with -f and -cg together.
    -g               # the gamma of RBF-SVM, you can get it through Search function (6) or define it by your experience, and you can not use this parameter with -f and -cg together.
    -o               # input the out folder name.
    -cv              # the cross validation fold of SVM, you can choose 5, 10 or -1 or define it by your experience.

## 16. Set Hyperparameters File

    folder_name    # input the train feature folder name.
    -c             # the penalty coefficient of SVM.
    -g             # the gamma of RBF-SVM.
    -o             # input the out file name, and the Hyperparameters file will be saved by defualt in current folder.

## 17. Ray Blast

    folder_name    # input the target fasta folder which has been created in Read function (3).
    -o             # input the out folder name, and the folder saved by default in /PSSMs.

## 18. Ray Supplement

    folder_name    # input the target fasta folder which has been created in Read function (3).
    -o             # input the out folder name, and the folder saved by default in /PSSMs.

## 19. View

    raac_book_name    # input the target RAAC book name.
    -t                # input the target type which you want to view.

## 20. Weblogo

    pssm_file_name    # input the target pssm file path.
    -raa              # input the target RAAC book name.
    -r                # input the target reduce type
    -o                # input the out file name, and the file saved by default in current folder.