# harmony  

Harmony package helps to build the tables and plots from alignment analysis designed for the multi-factor categorical case by extracting the Mplus output's information.   

## Installing  

To install, type in the RStudio console:    
`devtools::install_github("hai-mn/harmony", dependencies = TRUE)`  

Note that:  
- code in R is case-sensitive so capitalization is important.  
- harmony package requires some other dependent packages (tidyverse, reshape2, data.table, readxl, openxlsx, latex2exp, scales, ggpubr) in order to correctly execute. That's the reason we suggest users should install package with the `dependencies = TRUE` option, so that the users do not need to manually install the required packages themselves.   

Then call the package:   
`library(harmony)`  

## Structure of The Package  
The `harmony` package has six main functions:  
- `alignmentout()`: to generate table(s) with estimates, alignment values and R-square of Thresholds and Loadings  
- `alignmentthresholdplot()`: to generate alignment threshold plots having True/False invariant with estimates of group items and invariant average  
- `alignmentloadingplot()`: to generating alignment loading plot having True/False invariant with estimates of group items and invariant average  
- `convert2irt()`: to convert IFA estimates (threshold and loading) to IRT estimates (difficulty and discrimination)  
- `irc()`: to generate Cumulative and Category Item Probability (Characteristic/Response) Curve Plots  
- `cellsizedetect()`: to check the 2 x 2 crosstabs of items having the cell size and below specified by the user    

There are some supported functions `paraextract(inputfile, begphrase, endphrase, outputfile)`, `mplussplit(outpath = outpath, inputfile = inputfile)`,`latentsplit(filepath = paste0("Output","_",Sys.Date()), inputfile = "ext4_model results.txt")`, and `invariancesplit(inputfile="Invariant_Noninvariant.txt")` enable the main functions to properly work.  

## Strongly suggestion for a Mplus output running with `harmony` package
In order to the package to function, we recommend:  

- A Mplus output must have these parts:  
    1. INPUT INSTRUCTIONS
    2. SUMMARY OF ANALYSIS
    3. MODEL FIT INFORMATION
    4. MODEL RESULTS (including a part of <span style="color:red">APPROXIMATE MEASUREMENT INVARIANCE (NONINVARIANCE) FOR GROUPS</span>)  
    5. ALIGNMENT OUTPUT  
    6. SAVEDATA INFORMATION

    To have APPROXIMATE MEASUREMENT INVARIANCE (NONINVARIANCE) FOR GROUPS part, an user must add a line in Mplus syntax (.inp):   
    <span style="color:blue">OUTPUT:</span> 	ALIGN;

- Avoid comments in the Mplus syntax (phrases after "!" sign)   

- the labeling for alignment plots generated by `alignmentplot()` has to save in Excel file with 2 column names: __GroupNumber__ and __GroupLabel__

## Case Study

We demonstrated a case study of invariance analysis

__Step 1:__ Install and load the `harmony` package
- In RStudio console (or in the Source panel), type then Enter (or Ctrl+Enter if in the Source panel):
`devtools::install_github("hai-mn/harmony", dependencies = TRUE)`
`library(harmony)`
- We can safely ignore the warnings when call `harmony` package


__Step 2:__ Generating alignment tables
- Set the working directory in which we want to store the output fo tables and plots
`setwd(your-working-directory-here)`
- To generate the alignment tables, we call `alignmentout()`
- The pop-up results in the console would be:
~~~
The function provides information from Mplus alignment output, including:
 - Groups (latent classes),
 - Factors (continuous latent variables),
 - Items (dependent variables) and
 - Categories of each Item (equal to Thresholds + 1).

In addition, you may find from the folder 'Output_current date' in the working directory:
 - the multiple text files which split from the origin Mplus output
 - the thresholds, loadings tables (CSV format) and
 - especially, a combined Excel file with all separated spreadsheets
Enter path and Mplus output file (separated by /): your-path-file-here/Tutorial-Alignment-Free.out
- The Number of Groups (Latent Classes): 6
- The Name of Groups (Latent Classes): subgroup with categories of 1 2 3 4 5 6
- The Number of Factors: 2 ,including  HEADSTRO HYPERACT
- The Number of Items:  6 ,including:  BPI8 BPI10 BPI11 BPI12 BPI14 BPI16
- The Number of Categories and Threshold in each Item:
 Item.name Category Threshold
      BPI8        3         2
     BPI10        3         2
     BPI11        3         2
     BPI12        3         2
     BPI14        3         2
     BPI16        3         2
~~~
- Enter the path file and file name when the function requests:
"_your-path-file-here/Tutorial-Alignment-Free.out_"
Note: path file should be separated by / or \\\\
- The user can check the output at folder "Output_day-is-today" in the working directory: "threshold1.csv", "threshold2.csv", "loadings.csv", and ""
- Mplus code would look like (__should not have any comments__):
~~~
TITLE: 		Alignment (free)
DATA: 		FILE = NLSYPSIDbpi.dat ;
VARIABLE:	NAMES = subgroup
                        bpi8 bpi10 bpi11
                        bpi12 bpi14 bpi16
                        childid ;
                CATEGORICAL = bpi8 bpi10 bpi11
                              bpi12 bpi14 bpi16 ;
                IDVARIABLE = childid ;
                CLASSES = c(6) ;
                KNOWNCLASS = c(subgroup = 1 2 3 4 5 6 ) ;
ANALYSIS:       TYPE = MIXTURE ;
                ESTIMATOR = MLR ;
                LINK = LOGIT ;
                ALIGNMENT = FREE(Configural) ;
                ALGORITHM = INTEGRATION ;
OUTPUT: 	ALIGN ;
SAVEDATA: 	file is alignment-free.txt ;
MODEL: 	        %OVERALL%
                Headstrong BY bpi8 ;
                Headstrong BY bpi10 ;
                Headstrong BY bpi11 ;
                Hyperactive BY bpi12 ;
                Hyperactive BY bpi14 ;
                Hyperactive BY bpi16 ;
                Headstrong with Hyperactive ;
~~~
- The loadings table looks like:
![](loadings_table_example.JPG)
__Step 3:__ Generating alignment plots

__Step 4:__ Convert (Item Factor Analysis) IFA estimates to (Item Response Theory) IRT estimates

__Step 5:__ Generating category and cumulative probability curves


### Example Mplus files
Here is the list of files used in case study:
- [](): dataset
- [](): Mplus syntax file
- [](): Mplus output file



## Acknowledgment

Research reported in this publication was supported by the Eunice Kennedy Shriver National Institute Of Child Health & Human Development of the National Institutes of Health under Award Number R03HD098310. The content is solely the responsibility of the authors and does not necessarily represent the official views of the National Institutes of Health.

## References
