# PACTA_analysis

This set of files is the methodology for running PACTA on EQ and CB portfolios. 

To use this repository of code please follow the following instructions:


## System Requirements ##

|   | RECOMMENDED | MINIMUM |
| ------------- | ------------- |------------- |
| RAM  | 16GB or more  | 8 GB |
| if you use Windows  | 64-bit Version  |32-bit Version |
| R version  | 3.6.1  |no earlier than 3.4.3 |

You can download R here: 
https://cran.r-project.org/bin/windows/base/


## Software Requirements / R Environment

**Github software**

For example GitHub Desktop
https://desktop.github.com/

**Integrated Development Environment (IDE):f**

RStudio 
preferably version 1.1.456
https://rstudio.com/products/rstudio/

**R Environment:**

Ensure you have the required libraries installed on your computer.

Further, the evaluation will require a bunch of package. It is recommanded install the package during the first run of the code. (assess approx. 30 minutes for the installation process during the first usage of the code).

Necessary packages are: 
config, devtools, dplyr, fs, fst, glue, here, janitor, knitr, purrr,
readr, renv, rmarkdown, snakecase, testthat, tibble, tidyr, withr, yaml
    
**For report generation:**

MiKTeX - a distribution of the LaTeX typesetting system ; includes TeXworks
preferably version 2.9

Further R packages needed:
grid, ggplot2, ggthemes, reshape2, gridExtra, scales, stringr, extrafont, knitr, RColorBrewer, matrixStats, rworldmap, ggmap, cowplot, ggrepel, readxl, ggforce, sitools

## How to "download" the code

In order to keep track on the latest updates of the codes, we suggest not to download the code just once, but to synchronize a local folder with the PACTA_analysis git repository (https://help.github.com/en/github/getting-started-with-github/fork-a-repo). 
 - Create an user account on github
 - Fork this repo by clicking the Fork button at the top right  
 - Clone the repo to your local machine using a git-software (such as github desktop) 
 - Do frequent updates of the code by your github desktop

## Data Requirements
The evaluation requires input data that is provided by Asset Resoluation (https://asset-resolution.com/). If you are working in a collaboration with the 2DII, the 2DII possibly will allocate the necessary data files as a zip file. Unzip the data and save it a folder, that will later be refered to as data_location 

## Workflow

There are four files that must be run in sequence to create results. 

1_portfolio_check_initialisation.R
2_project_input_analysis.R
3_run_analysis.R
4_report_maker.R

If you stop the process at a certain point (let's say after 2_project_input_analysis), you can go on later at any time with the subsequent steP (with 3_...) but, you after a break/restart of R, you will need to run 1_portfolio_check_initialisation again.

### Initialisation

**1. Set Variables**

Within 1_portfolio_check_initialisation.R there are some lines that should be edited to make reference to local directories on your computer. 

```project_name <- "TEST"```

   - Please define ```project_name``` with a string ("...") by your choice. The project name is relavant for input, output and naming of data files.

```twodii_internal <- FALSE ```

   - Ensure that the variable ```twodii_internal``` is set ```False```. 

```project_location_ext <- "/Desktop/ExternalTest"```

   - Please define ```project_location_ext``` by copying the full path to the folder, in which you want to store data (including the results) related to this particular evaluation. During the run of the code, the a project folder with several sub-folders will be created here.
   - Please consider: 
     - copying in a path from windows requires to change all "\" to "/". 
     - path needs to be a string ("...")

```data_location_ext <- "/Desktop/ExternalTest/Inputs"```

   - Please define ```data_location_ext``` by copying the full path to the folder, in which you stored the data files (set of rda files) provided Asset Resoluation or the 2DII.

   - Please consider: 
     - copying in a path from windows requires to change all "\" to "/". 
     - path needs to be a string ("...")

**2. Run 1_portfolio_check_intialisation.R**

**3. Check results**

After running 1_portfolio_check_intialisation, go to the project_location_ext folder. Now, you should see that a new folder was generated. The name of the folder equals the project name you defined before running the code. The folder contains several subfolders.

### Input Analysis
**1. Setting the portfolio input data**

Go to the project folder and open the subfolder 20_Raw_Inputs. There is a csv-file that is named according to the project name and has the annex "-Input". Open the csv-file. 
If you use Excel for editting the csv file, make sure that "," and not ";" is set to be the column seperater. If Excel uses wrong settings, the different columnes are not detected, but as a string of words, seperated by commas is shown. If so, select the first row in Excel (A):
    - Go to Data > Text to Columns.
    - Choose Delimited. Click Next.
    - Choose Comma. Click Next.
    - Choose General or Text, whichever you prefer.
    - Leave Destination as is, or choose another column. Click Finish.
    
As soon as the headings are displayed in seperate coumns, add your own portfolio data to this file and save. It is important that the column headings are not changed. The data needs to have the following format: 

| Investor.Name	| Portfolio.Name	| ISIN	| NumberofShares	| MarketValue	| Currency | 
 | -------------  | -------------  | -------------  | -------------  | -------------  | ------------- |
| Berlin_Pension_Fund| Green_Assets | DE2939203293 | ------------- | 8493050 | USD |
| Berlin_Pension_Fund  | Green_Assets  | FR3439285941 |------------- | 324234 | CHF |
| Berlin_Pension_Fund  | EU_Fund  | FR3439285941 |------------- | 2384929 | EUR |
| Paris_Fund  | EmergingMarkets  | FR3439285941 | 172 | 1239322 | GBP |

   - There might be data from several investors  and several portfolios for each investor. The code will generate results for each single portfolio. 
   - NumberofShares is the only row that is optional. It might stay empty. All others need to be filled
   - Currency needs to have the 3-letter format
  
**2. Setting the parameter files**

Go to the project folder and open the subfolder 10_Parameter_File. There are two yml-files that can be opened in usual text editors. At this stage only open the AnalysisParameter file. Assert that the mentioned Timestamps match the holdings date of your portfolio input. The Years.Startyear should equal the year after the data time stamp. If you see any inconsistencies please reach out to transitionmonitor@2degrees-investing.org 

By ```CreateMetaPortfolio: TRUE``` it is determined that a Meta Portfolio is created. That means, that one portfolio with aggregated results of all investors / portfolios is generated and evaluated. By re-writing it to ```CreateMetaPortfolio: FALSE``` and saving the file, the generation of the MetaPortfolio is deactivated.

**3. Run 2_project_input_analysis.R**

**4. Check outputs**

Go to the project folder and open the subfolder 30_Processed_Inputs. You should find 11 files, that are generated from your input: One image and five pairs of data files, each one as csv and rda file.

- AuditChart: The Audit Chart is a scale bar chart that shows on a relative scale the share of ISINs that are included in analysis (blue) and the ISINs that are neglected as no Bloomberg Data is available (gray). The blue bar is usually far above 50%, mostly in the range of 90-95%.

- ...total_portfolio: This is the basic audit files that lists the derived data for each ISIN from the initial input.


### Run Analysis
There are no settings at this point. 

**1. Run 3_run_analysis.R**

**2. Check results**

Go to the project folder and open the subfolder 40_Results. You should find 4 files and several folders. For each investor you one folder is created. Further, if CreatMetaPortofolio was set True, a folder for MetaInvestor was created. 


### Report Maker

## Contact

Sample data files will be made available shortly. Complete data files are available under specific circumstances. Be in contact at transitionmonitor@2degrees-investing.org if you would like to find out more about this.  
