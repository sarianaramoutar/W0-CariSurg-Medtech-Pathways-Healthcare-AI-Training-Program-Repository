# W0-Day-1-Data-Cleaning-Challenge-for-Emergency-Triage
In a high-pressure Emergency Department (ED) setting, data entry is often very messy. Assignment 1 focused on cleaning the gender demographics of patients at 'Mercer General Hospital' to ensure that our triage AI system interprets patient data accurately. 

## Methodology
The raw dataset contained highly inconsistent entries for Gender, including varying case, numeric strings and mixed formats. My cleaning process involved: 
- Converting inconsistent strings (e.g., "m", "MALE", "1") into a unified integer system to remove ambiguity for the AI. Males were assigned to `1` and Females were assigned to `0`. 
- Adding a check to note empty (null) cells. Identifying missing data is critical in a triage system because missing a vital or demographic can lead to major errors.
- Inclusivity. The code was written to recognise and categorise non-binary gender markers, ensuring the system remains relevant for all patient populations in the region. Other genders were assigned to `2`.
