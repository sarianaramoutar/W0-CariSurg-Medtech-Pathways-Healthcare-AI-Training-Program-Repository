# CariSurg Healthcare AI Summer Program - Week 0 Repository
This repository documents my progress through Week 0 of the CariSurg Healthcare AI Summer Program at 'Mercer General Hospital's Clinical AI & Innovation Unit'. The work done starts with raw clinical data engineering to the foundational algorithmic design of an AI learning model in an Emergency Department Triage System. 

## Day 1: Data Cleaning Challenge for Emergency Triage
In a high-pressure Emergency Department (ED) setting, data entry is often very messy. Assignment 1 focused on cleaning the gender demographics of patients at 'Mercer General Hospital' to ensure that our triage AI system interprets patient data accurately. 

### Methodology
The raw dataset contained highly inconsistent entries for Gender, including varying case, numeric strings and mixed formats. My cleaning process involved: 
- **Binary Mapping:** converting inconsistent strings (e.g., "m", "MALE", "1") into a unified integer system to remove ambiguity for the AI. Males were assigned to `1` and Females were assigned to `0`. 
- **Null Value Monitoring:** adding a validation check to flag empty (null) cells. Identifying missing data is critical in a triage system because missing a vital or demographic can lead to major downstream routing errors.
- **Demographic Inclusivity:** ensuring the code recognises and categorises non-binary gender markers so the system remains relevant for all local patient populations. Other genders were assigned to `2`.

## Day 2: Advanced Data Cleaning (Group Challenge)
Contributors: Tyler Baksh, Kaylah Leigertwood-Ollivierre, Mya Symister, Sariana Ramoutar (myself), Zhanna McDonald, Sekou Ruddock

Patient vitals must be mathematically clean and reliable before they are processed by predictive machine learning models. Assignment 2 focused on advanced auditing and statistical imputation of the **Respiratory Rate (RR)** vector. 

### Methodology
- **Pre-Cleaning Visualisation:** generated side-by-side histogram and box plot visualisations to analyse the baseline data distribution and isolate extreme abnormalities before altering any values.
- **Skew-Aware Imputation:** discovered that the RR distribution was heavily right-skewed due to critical patients experiencing rapid breathing. Because these outliers pull the clinical `mean` upward, we chose to impute missing values using the `median` (~18 breaths per min) to maintain a strong, unbiased baseline.
- **Physiological Safety Boundaries:** implemented a logical threshold filter (5 to 60 breaths per min) to eliminate impossible data entry errors. Our diagnostic confirmed that 100% of the active, non-corrupted RR records fell safely within these boundaries.
- **Verification:** re-evaluated distribution shifts using post-cleaning visualisations to confirm the data integrity without introducing unexpected distortions.

## Day 3: Basic Data Visualisation with matplotlib
Charts are useful for seeing which patients require care first based on plotted vitals and distributions. This is especially helpful in an ED triage system where nurses must make quick decisions, just by looking at data at a glance. For Assignment 3, I structured 3 distinct plots around specific clinical research questions: 

### 1. Histogram: Respiratory Rate (RR) Distribution
*"What proportion of Emergency Department patients present with abnormal respiratory rates (bradypnea or tachypnea) compared to the normal clinical baseline of 12–20 breaths per minute?*
This plot isolates patients falling outside the healthy resting baseline, where threshold lines indicate patient severity. Less than `12` breaths/min indicates bradypnea and over `24` breaths/min indicates distress (tachypnea).

### 2. Scatter Plot: Cardiorespiratory Panic Map (Pulse Rate vs. RR)
*"How prevalent is concurrent cardiorespiratory distress among incoming emergency patients?"*
This scatter plot flags systemic failure where the heart and lungs speed up together to maintain homeostasis. By plotting threshold boundaries (Pulse at 100 bpm, RR at 20 breaths/min), the upper-right quadrant serves as a clear visual "danger zone" to spot patients in physiological shock.

### 3. Scatter Plot: Occult Shock Identification (Pulse Rate vs. Systolic Blood Pressure)
*"How many patients have a dangerous Shock Index, and does this score help us catch hidden shock in patients who otherwise look normal?"* 
The **Shock Index ({Pulse} / {SBP})** is a critical tool for detecting "occult shock" — a deceptive state where a patient is dangerously ill but their individual vital signs still appear normal. In hemorrhages, sepsis, or massive trauma, the heart beats rapidly to compensate for dropping blood volume, keeping the SBP temporarily stable. The normal baseline is from `0.5` to `0.7`. Elevated Risk from `0.8` to `0.9`, and Critical Circulatory Collapse >= `1.0` (Occurs when heart rate equals or exceeds SBP, signaling immediate crashing status).

## Day 4: Vital Sign Deep Dive - Respiratory Rate (RR)
To design clinical AI algorithms, we have to first understand the physiology behind each metric. Assignment 4 focused on an exploration of RR as a primary clinical marker for acute patient distress. 

## Day 5: Missing Vital Metrics for Comprehensive Triage
An AI model can only see what is inside its dataset. For Assignment 5, I analysed three other important clinical features that were missing from the baseline dataset. 

### Proposed Additions
- **Oxygen Saturation (SpO2):** measured via pulse oximetry. It tracks the percentage of haemoglobin bound to oxygen.
- **Capillary Blood Glucose (CBG):** a rapid finger-prick test to track blood sugar.
- **Capillary Refill Time (CRT):** a quick physical test measuring peripheral blood flow.

## Day 6: AI Triage Logic Framework
The final capstone of Week 0 was integrating data cleaning rules, multi-variable interactions and missing feature checks into a structured, step-by-step instruction framework for an AI-assisted digital triage system. 

### System Architecture Highlights
- **[STAGE 1] Immediate Trauma Override:** a high-priority visual bypass condition (`Is_Visual_Trauma_Emergency`) instantly routes major injuries (e.g., active hemorrhages, penetrating trauma) straight to a `Level 1` emergency, bypassing calculation times completely.
- **[STAGE 2] Range Verification & Imputation:** filters out data entry typos and uses a two-tier lookup (Individual EHR history falling back or using a safe population median) to patch missing data, accompanied by a warning for the nurse.
- **[STAGE 3 & 4] Clinical Multi-Variable Adjustment:** adjusts thresholds for chronic baseline conditions and scores multi-variable interactions simultaneously. 
- **[STAGE 5 & OUTPUT] Point Scoring & Fail-Safe Mechanisms:** calculates scores into 4 clear levels (Resuscitation down to Non-Urgent). The system builds in explicit safeguards for regional Caribbean hospitals, utilizing a **local-first SQLite database** to protect data during sudden power grids failures, a **digital aging score** to prioritize patients waiting in crowded rooms, and a **computer vision alert** to spot patients collapsing silently in the waiting hall. 
