# W0 Day 1 Data Cleaning Challenge for Emergency Triage
In a high-pressure Emergency Department (ED) setting, data entry is often very messy. Assignment 1 focused on cleaning the gender demographics of patients at 'Mercer General Hospital' to ensure that our triage AI system interprets patient data accurately. 

## Methodology
The raw dataset contained highly inconsistent entries for Gender, including varying case, numeric strings and mixed formats. My cleaning process involved: 
- Converting inconsistent strings (e.g., "m", "MALE", "1") into a unified integer system to remove ambiguity for the AI. Males were assigned to `1` and Females were assigned to `0`. 
- Adding a check to note empty (null) cells. Identifying missing data is critical in a triage system because missing a vital or demographic can lead to major errors.
- Inclusivity. The code was written to recognise and categorise non-binary gender markers, ensuring the system remains relevant for all patient populations in the region. Other genders were assigned to `2`.


# W0 Day 2 Advanced Data Cleaning Challenge
Contributors: Tyler Baksh, Kaylah Leigertwood-Ollivierre, Mya Symister, Sariana Ramoutar (myself), Zhanna McDonald, Sekou Ruddock

In an ED, patient vitals must be clean and reliable before they are processed by a machine learning model. Assignment 2 focused on advanced data cleaning and statistical imputation of the **Respiratory Rate (RR)** column. 

## Methodology
Our group ran a comprehensive check on the raw RR column of the dataset. While the data type was already correctly formatted as numeric, we discovered several missing values (`NaN`).
- Generated side by side histogram and box plot visualisations to analyse the data's behaviour and point out extreme abnormalities before cleaning.
- Discovered that the RR distribution was heavily right-skewed due to critical patients experiencing rapid breathing. Because these outliers pull the clinical `mean` upward, we chose to impute missing values using the `median` (~18 breaths per min) to maintain a robust, unbiased baseline.
- Implemented logical safety filter to check for physiologically impossible data entry errors. A strict clinical range threshold (5 to 60 breaths per min) was established. Upon execution, it was confirmed that 100% of the active RR data fell safely within these boundaries.
- By reviewing distribution changes across histograms and box plots both *before* and *after* the cleaning cycle, the integrity of the adjusted data was verified. 


# W0 Day 3 Basic Data Visualisation with matplotlib
Charts are useful in seeing which patients require care first based on plotted vitals and distributions. This is especially helpful in a an ED triage system where nurses have to make quick decisions, just by looking at data at a glance. 

## Methodology
For this assignment, I chose two vital plots: 
- Histogram: Respiratory Rate (RR) Distribution. This asks how many patients in the ED are breathing abnormally. Added threshold lines that indicate patient severity. Less than `12` breaths/min indicates bradypnea and over `24` breaths/min indicates distress (tachypnea).
- Scatter Plot: Pulse Rate vs. Respitatory Rate. This spots patients whose bodies are crashing. Heart rate and breathing rate climb together. Threshold boundaries were plotted (pulse at 100, RR at 20), so the upper right quadrant indicates patients in the danger zone, making it easy for triage nurses to spot. 
