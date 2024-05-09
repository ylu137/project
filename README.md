# Objective 
The focus will be on using public data to explore the significance of “self-reported health” as a health indicator.
```markdown
                              1. Data
                                     \
                          2. Code -> 4. Inferences -> 5. Updates -> 6. Transparency 
                                     /
                                      3. Parameters
```

# Data source
There are two data need to be downloaded.
- The first is the survey data from the 1999-2000 National Health and Nutrition Examination Survey (NHANES).
  - This is the link to download [NHANES 1999-2000 Survey Data](https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/DEMO.XPT) 
- The second is mortality follow-up data from the National Center for Health Statistics (NCHS).
  - This is the link to download [NHANES Mortality Data](https://ftp.cdc.gov/pub/HEALTH_STATISTICS/NCHS/datalinkage/linked_mortality/NHANES_1999_2000_MORT_2019_PUBLIC.dat)

# Detailed Steps and Resources
## Step 1: Data Acquisition and Preparation:
- Survey Data:
  Import the survey data from the 1999-2000 National Health and Nutrition Examination Survey (NHANES):
  ```markdown
  import sasxport5 "https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/DEMO.XPT", clear
  ```
- Mortality Follow-up Data:
  Obtain follow-up mortality data to analyze over a 20-year period from the National Center for Health Statistics (NCHS).
  ```markdown
   //data
   global mort_1999_2000 https://ftp.cdc.gov/pub/HEALTH_STATISTICS/NCHS/datalinkage/linked_mortality/NHANES_1999_2000_MORT_2019_PUBLIC.dat
   //code
   cat https://ftp.cdc.gov/pub/HEALTH_STATISTICS/NCHS/datalinkage/linked_mortality/Stata_ReadInProgramAllSurveys.do`
  ```
The two data above will be merged and analyzed. 
## Step 2: Code Development (Preperation):
- Download, modify, and upload `Stata_ReadInProgramAllSurveys.do`.
   - Download, modify, and upload the provided Stata `.do` file for linking the `DEMO.XPT` data to mortality follow-up data.
      `https://ftp.cdc.gov/pub/HEALTH_STATISTICS/NCHS/datalinkage/linked_mortality/Stata_ReadInProgramAllSurveys.do`
   - Rename this file to followup.do and commit it with the description: “Updated DEMO.XPT linkage .do file”.
   - You may watch the week 6 video for the key items to edit. For instance, you may edit it so that it reads in the data directly from the website.
## Step 3: Code Development (Merge):
- Execute the following Stata code to merge the survey data with the mortality data, ensuring alignment on the unique sequence numbers:
  ```markdown
    global repo "https://github.com/ylu137/project/raw/main/"
    cls
    do ${repo}followup.do
    save followup, replace 
    import sasxport5 "https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/DEMO.XPT", clear
    merge 1:1 seqn using followup
    lookfor follow
  ```
## Step 4: Key Parameters for Week 7s Analysis (data import)
- Import `Self-Reported Health Assessment`:
  - Import the specific health questionnaire data and prepare it for analysis in Week 7:
  ```markdown
    import sasxport5 "https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/HUQ.XPT", clear
  ```
## Step 5: Code Summary
- Here’s a first iteration of a script that answers they project main goal.
  - Save it as `project.do` and upload it to you `repo`.
  - Keep updating it over the next two weeks, with a meaningful commit statement each time for version control.

  ```markdown
    global repo "https://github.com/ylu137/project/raw/main/"
    cls
    do ${repo}followup.do
    save followup, replace 
    import sasxport5 "https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/DEMO.XPT", clear
    merge 1:1 seqn using followup
    lookfor follow
    lookfor mortstat permth_int eligstat 
    keep if eligstat==1
    capture g years=permth_int/12
    codebook mortstat
    stset years, fail(mortstat)
    sts graph, fail
    save demo_mortality, replace 
    import sasxport5 "https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/HUQ.XPT", clear 
    merge 1:1 seqn using demo_mortality, nogen
    sts graph, by(huq010) fail
    stcox i.huq010
  ```
## Parameters
- Self-reported health
- Age
- Sex
- Race
- Ethnicity
  
## Inferences
- 95%CI
- p-values

## Updates
- Expand from 1999 to 2005

## Transparency
- Show all your work in one `.do` file script
- Have donors as a parallel?
