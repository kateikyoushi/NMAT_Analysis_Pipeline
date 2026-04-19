# NMAT_Ultima Data Dictionary

## Source
- File: dataset/NMAT_Ultima.csv
- Rows: 178,927
- Columns: 115
- Profiling basis: dataset/output/NMAT_Ultima_profile.csv (full-file missingness scan + sampled type inference)

## Scoring Framework Notes
- Part I (Aptitude/Cognitive Skills): Verbal, Inductive Reasoning, Quantitative, Perceptual Acuity.
- Part II (Science/Subject Knowledge): Biology, Physics, Social Science, Chemistry.
- TRUE Raw formula used in analysis: TotalRawScoreTRUE = PartIRawScoreTRUE + PartIIRawScoreTRUE.
- PercentileDecile is the decile categorization of percentile rank (D1 lowest to D10 highest).

## Column Dictionary

| Column | Group | Inferred Type | Missing % | Description | Sample Values |
|---|---|---|---:|---|---|
| Year | Time | numeric | 0.000 | NMAT examination year. | 2006 ; 2007 ; 2008 |
| NMA_TestDate | Time | datetime-like | 0.000 | Original NMAT test date string from source. | 12/10/2006 ; 4/9/2006 ; 10/21/2006 |
| NMA_AppNo | Identity & Demographics | numeric | 0.000 | Original applicant number from NMAT source. | 1070637 ; 1060350 ; 1060229 |
| NMA_AppNo_clean | Identity & Demographics | numeric | 0.000 | Cleaned applicant number used in matching. | 1070637 ; 1060350 ; 1060229 |
| NMA_Name | Identity & Demographics | text-like | 0.000 | Original full examinee name. | Abad, Giselle Katigbak ; Abad, Timothy Joseph Galimba ; Abadam, Edelyn Joy Macadaan |
| NMA_Sex | Identity & Demographics | numeric | 0.000 | Original NMAT sex code (typically 1/2). | 2 ; 1 |
| NMA_BirthDate | Time | text-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 00/00/0000 ; 8/27/1985 ; 3/18/1992 |
| BDATE | Time | datetime-like | 14.086 | Parsed birthdate used for PERSON_KEY construction when available. | 08/27/1985 ; 03/18/1992 ; 06/16/1991 |
| AGE | Identity & Demographics | numeric | 0.025 | Examinee age at test date (derived/standardized). | 21.0 ; 20.0 ; 19.0 |
| SEX | Identity & Demographics | categorical-like | 0.025 | Standardized sex label (Male/Female when available). | Female ; Male |
| CIVIL_STATUS | Identity & Demographics | categorical-like | 0.031 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | Single ; Separated ; Married |
| NAC_NATIONALITY | Identity & Demographics | text-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | Filipino ; Fil-Am ; Korean |
| NMA_Graduating | Other | boolean-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 0 ; 1 |
| NMA_YearGrad | Time | numeric | 0.000 | Reported year of college graduation. | 2006 ; 2007 ; 2001 |
| NMA_Course | Education & Institution | text-like | 0.002 | Original reported pre-med course/program. | Education & Teacher Training Education ; General Arts ; Education & Teacher Training Teacher Training For Teaching Pr... |
| Course Classification | Education & Institution | text-like | 0.000 | Higher-level classification of NMA_Course. | Education & Teacher Training ; General ; Social & Behavioral Sciences |
| Course_recode | Education & Institution | text-like | 0.000 | Standardized course recode label. | Education ; Arts ; Teacher Training for Teaching Pre-school or Kindergarten |
| CourseGroup | Education & Institution | categorical-like | 0.000 | Final grouped pre-med background used in analysis. | Education ; Other ; Social & Behavioral Sciences |
| MED_SCHOOL_CHOICE1 | Other | text-like | 2.123 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | Pamantasan ng Lungsod ng Maynila ; UP College of Medicine (Health Sciences Center) ; UERM Memorial Medical Center |
| MED_SCHOOL_CHOICE2 | Other | text-like | 6.154 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | University of Santo Tomas ; UERM Memorial Medical Center ; St. Luke's Medical Center |
| MED_SCHOOL_CHOICE3 | Other | text-like | 6.204 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | UERM Memorial Medical Center ; St. Luke's Medical Center ; University of Santo Tomas |
| NMA_College_RAW | Education & Institution | text-like | 0.000 | Original college text before normalization. | University Of Santo Tomas ; Ateneo De Manila University ; University Of The Philippines - Diliman |
| UNIVERSITY | Education & Institution | text-like | 0.000 | Standardized university name. | UNIVERSITY OF SANTO TOMAS ; ATENEO DE MANILA UNIVERSITY ; UNIVERSITY OF THE PHILIPPINES |
| UNI_TYPE | Education & Institution | categorical-like | 0.000 | Final university type category (Public/Private/Foreign/Not Specified). | Private ; Public ; Foreign |
| UNI_LOCATION | Education & Institution | categorical-like | 0.000 | University location class (Local/International/Unknown). | Local ; International ; Unknown |
| UNIVERSITY_VERIFIED | Education & Institution | text-like | 1.010 | University value after verification against reference table. | UNIVERSITY OF SANTO TOMAS ; ATENEO DE MANILA UNIVERSITY ; UNIVERSITY OF THE PHILIPPINES |
| UNI_TYPE_VERIFIED | Education & Institution | categorical-like | 1.010 | Verified university type from reference process. | Private ; Public ; Foreign |
| UNI_LOCATION_VERIFIED | Education & Institution | categorical-like | 1.010 | Verified university location from reference process. | Local ; International ; Unknown |
| final_value_source | Pipeline Metadata | categorical-like | 0.000 | Source pathway used for final university fields. | UNIVS_MATCHED ; FALLBACK_UNSPECIFIED |
| draft_university | Education & Institution | text-like | 1.010 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | UNIVERSITY OF SANTO TOMAS ; ATENEO DE MANILA UNIVERSITY ; UNIVERSITY OF THE PHILIPPINES |
| draft_uni_type | Education & Institution | categorical-like | 1.010 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | Private ; Public ; Foreign |
| draft_uni_location | Education & Institution | categorical-like | 1.010 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | Local ; International ; Unknown |
| draft_hint_method | Pipeline Metadata | categorical-like | 1.010 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | UNIVS_EXACT_PRIMARY ; UNIVS_EXACT_SECONDARY ; UNIVS_FUZZY |
| draft_hint_score | Assessment Scores | numeric | 1.010 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 100.0 ; 88.0 ; 94.0 |
| verification_method | Pipeline Metadata | categorical-like | 0.000 | Matching method used to verify institution fields. | UNIVS_EXACT_PRIMARY ; UNIVS_EXACT_SECONDARY ; NO_UNIVS_MATCH |
| verification_status | Pipeline Metadata | categorical-like | 0.000 | Verification status of institution mapping. | VERIFIED ; UNMATCHED |
| confidence | Pipeline Metadata | numeric | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 100 ; 0 ; 88 |
| evidence_summary | Pipeline Metadata | categorical-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | Matched from UNIVS.csv via UNIVS_EXACT_PRIMARY (score=100). ; Matched from UNIVS.csv via UNIVS_EXACT_SECONDARY (score... |
| IS_PLE_PASSER | PLE Linkage | boolean-like | 0.000 | Whether examinee matched a PLE passer record. | False ; True |
| IS_PLE_ANALYSIS_SAFE | PLE Linkage | boolean-like | 0.000 | Strict PLE linkage flag used for analysis-safe matched subset. | False ; True |
| IS_BEST_NMAT_RECORD | Other | boolean-like | 0.000 | Flag marking selected best NMAT attempt per person. | True ; False |
| PLE_MATCH_STATUS | PLE Linkage | categorical-like | 0.000 | PLE linkage state (e.g., FINAL_MATCH, AMBIGUOUS, NOT_IN_PLE). | NOT_IN_PLE ; FINAL_MATCH ; AMBIGUOUS |
| PLE_MATCH_METHOD | PLE Linkage | categorical-like | 68.184 | Method used in PLE matching process (EXACT/FUZZY/MANUAL). | EXACT ; MANUAL_APPNO_MATCH ; FUZZY |
| PLE_YEAR_PASSED | Time | numeric | 68.717 | Year of PLE passage for matched records. | 2012.0 ; 2013.0 ; 2011.0 |
| PLE_YEAR_GAP | Time | numeric | 71.902 | PLE_YEAR_PASSED - NMAT year gap. | 6.0 ; 7.0 ; 5.0 |
| PLE_MATCH_CONFIDENCE | PLE Linkage | numeric | 68.184 | Numeric confidence score of PLE match. | 100.0 ; 50.0 ; 85.0 |
| PLE_MATCH_REASON | PLE Linkage | text-like | 68.184 | Text reason/rationale for chosen PLE match. | Single exact name match, year gap OK ; 2 candidates remain after all filters — manual review needed ; 1 candidate aft... |
| School Type_rec2_FINAL | Education & Institution | categorical-like | 0.000 | Original finalized school type label used for integrity checks. | Private ; Public ; Foreign |
| NMC_Center | Other | categorical-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | Manila ; Baguio ; Cebu |
| NMAT Province local address | Other | text-like | 0.003 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | Rizal ; Ncr ; Bulacan |
| NMAT Region permanent address | Other | text-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | Region IV-A ; NCR ; Region III |
| NMS_VCss | Assessment Scores | numeric | 0.000 | NMAT standard score: Verbal subtest. | 512 ; 726 ; 529 |
| NMS_IRss | Assessment Scores | numeric | 0.000 | NMAT standard score: Inductive Reasoning subtest. | 461 ; 584 ; 617 |
| NMS_Qss | Assessment Scores | numeric | 0.000 | NMAT standard score: Quantitative subtest. | 599 ; 570 ; 652 |
| NMS_PAss | Assessment Scores | numeric | 0.000 | NMAT standard score: Perceptual Acuity subtest. | 526 ; 473 ; 423 |
| NMS_BIOss | Assessment Scores | numeric | 0.000 | NMAT standard score: Biology subtest. | 461 ; 652 ; 570 |
| NMS_PHYss | Assessment Scores | numeric | 0.000 | NMAT standard score: Physics subtest. | 481 ; 601 ; 617 |
| NMS_SSCss | Assessment Scores | numeric | 0.000 | NMAT standard score: Social Science subtest. | 497 ; 652 ; 542 |
| NMS_CHEMss | Assessment Scores | numeric | 0.000 | NMAT standard score: Chemistry subtest. | 374 ; 634 ; 556 |
| NMS_APT | Assessment Scores | numeric | 0.000 | NMAT Part I (Aptitude) standard score composite. | 539 ; 624 ; 588 |
| NMS_SA | Assessment Scores | numeric | 0.000 | NMAT Part II (Science) standard score composite. | 454 ; 672 ; 594 |
| NMS_GPS | Assessment Scores | numeric | 0.000 | General Performance Score (standardized overall score). | 489 ; 650 ; 589 |
| NMS_PER | Assessment Scores | numeric | 0.000 | Percentile rank (string/integer source variant). | 46 ; 93 ; 81 |
| NMS_PER_num | Assessment Scores | numeric | 0.713 | Numeric percentile rank used in analysis. | 46.0 ; 93.0 ; 81.0 |
| PercentileDecile | Assessment Scores | categorical-like | 2.314 | Decile bin derived from percentile rank (D1-D10). | D5 ; D10 ; D9 |
| HasCEMMatch | PLE Linkage | boolean-like | 0.000 | Whether NMAT record has a CEM linked row. | True ; False |
| HasTRUErawScores | Assessment Scores | boolean-like | 0.000 | Whether all needed raw-score fields were available to derive TRUE raw. | True ; False |
| STU_RSCORE | Assessment Scores | numeric | 44.494 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 82.0 ; 147.0 ; 141.0 |
| STU_RSCORE_CALC | Assessment Scores | numeric | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 125.0 ; 175.0 ; 156.0 |
| STU_RSCORE_VALID | Assessment Scores | categorical-like | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | VALID ; INVALID |
| StoredRawTotal | Assessment Scores | numeric | 44.494 | Stored total raw score from source when present. | 82.0 ; 147.0 ; 141.0 |
| CalculatedRawTotal_Source | Assessment Scores | numeric | 0.025 | Calculated raw total from source-linked components. | 125.0 ; 175.0 ; 156.0 |
| TotalRawScoreTRUE | Assessment Scores | numeric | 0.025 | TRUE total raw score used in analyses. | 125.0 ; 175.0 ; 156.0 |
| PartIRawScoreTRUE | Assessment Scores | numeric | 0.025 | TRUE Part I raw score (Verbal + IR + Quantitative + Perceptual Acuity). | 71.0 ; 86.0 ; 80.0 |
| PartIIRawScoreTRUE | Assessment Scores | numeric | 0.025 | TRUE Part II raw score (Biology + Physics + Social Science + Chemistry). | 54.0 ; 89.0 ; 76.0 |
| Raw_Verbal | Assessment Scores | numeric | 0.025 | Raw subscore: Verbal. | 16.0 ; 28.0 ; 19.0 |
| Raw_InductiveReasoning | Assessment Scores | numeric | 0.025 | Raw subscore: Inductive Reasoning. | 17.0 ; 22.0 ; 24.0 |
| Raw_Quantitative | Assessment Scores | numeric | 0.025 | Raw subscore: Quantitative. | 21.0 ; 20.0 ; 25.0 |
| Raw_PerceptualAcuity | Assessment Scores | numeric | 0.025 | Raw subscore: Perceptual Acuity. | 17.0 ; 16.0 ; 12.0 |
| Raw_Biology | Assessment Scores | numeric | 0.025 | Raw subscore: Biology. | 16.0 ; 24.0 ; 20.0 |
| Raw_Physics | Assessment Scores | numeric | 0.025 | Raw subscore: Physics. | 14.0 ; 19.0 ; 20.0 |
| Raw_SocialScience | Assessment Scores | numeric | 0.025 | Raw subscore: Social Science. | 15.0 ; 24.0 ; 18.0 |
| Raw_Chemistry | Assessment Scores | numeric | 0.025 | Raw subscore: Chemistry. | 9.0 ; 22.0 ; 18.0 |
| Std_Verbal_CEM | Assessment Scores | numeric | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 512.0 ; 726.0 ; 529.0 |
| Std_InductiveReasoning_CEM | Assessment Scores | numeric | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 461.0 ; 584.0 ; 617.0 |
| Std_Quantitative_CEM | Assessment Scores | numeric | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 599.0 ; 570.0 ; 652.0 |
| Std_PerceptualAcuity_CEM | Assessment Scores | numeric | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 526.0 ; 473.0 ; 423.0 |
| Std_Biology_CEM | Assessment Scores | numeric | 0.029 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 461.0 ; 652.0 ; 570.0 |
| Std_Physics_CEM | Assessment Scores | numeric | 0.029 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 481.0 ; 601.0 ; 617.0 |
| Std_SocialScience_CEM | Assessment Scores | numeric | 0.029 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 497.0 ; 652.0 ; 542.0 |
| Std_Chemistry_CEM | Assessment Scores | numeric | 0.029 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 374.0 ; 634.0 ; 556.0 |
| APT_CEM | Assessment Scores | numeric | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 539.0 ; 624.0 ; 588.0 |
| SA_CEM | Assessment Scores | numeric | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 454.0 ; 672.0 ; 594.0 |
| GPS_CEM | Assessment Scores | numeric | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 489.0 ; 650.0 ; 589.0 |
| Percentile_CEM | Assessment Scores | numeric | 2.337 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 46.0 ; 93.0 ; 81.0 |
| AllRawComponentsPresent | Assessment Scores | boolean-like | 0.025 | Whether all 8 raw components are present. | True |
| StoredVsDerivedMismatch | PLE Linkage | numeric | 44.494 | Indicator that stored raw total differs from derived total. | 1.0 ; 0.0 |
| CalcVsDerivedMismatch | PLE Linkage | numeric | 0.025 | Indicator that calculated source total differs from derived total. | 0.0 |
| raw_component_count | Assessment Scores | numeric | 0.025 | Count of available raw component scores. | 8.0 |
| COLLEGE_NAME | Identity & Demographics | text-like | 1.133 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | University of Santo Tomas ; Ateneo de Manila University ; University of the Philippines - Diliman |
| COURSE_DESC | Education & Institution | text-like | 0.145 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | [EDUCATION & TEACHER TRAINING] Education ; [GENERAL] Arts ; [EDUCATION & TEACHER TRAINING] Teacher Training for Teach... |
| STU_TESTDATE | Time | datetime-like | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 'Dec06 ; 'Apr06 ; 'Oct06la |
| NMAT_YEAR | Time | numeric | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 2006.0 ; 2007.0 ; 2008.0 |
| KEY | Identity & Demographics | text-like | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | ANON_15903_261811 ; ANON_26269_260134 ; ANON_23735_260094 |
| merge_verified_university | Education & Institution | categorical-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | both |
| merge_cem | Pipeline Metadata | categorical-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | both ; left_only |
| SOURCE_NMAT | Pipeline Metadata | categorical-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | NMAT_CLEANED_DATA |
| NMA_College | Education & Institution | text-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | University Of Santo Tomas ; Ateneo De Manila University ; University Of The Philippines - Diliman |
| NMA_College_norm | Education & Institution | text-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | UNIVERSITY OF SANTO TOMAS ; ATENEO DE MANILA UNIVERSITY ; UNIVERSITY OF THE PHILIPPINES DILIMAN |
| STU_NO_clean | Other | numeric | 0.025 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 1070637.0 ; 1060350.0 ; 1060229.0 |
| NAME_NORM | Identity & Demographics | text-like | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | ABAD, GISELLE KATIGBAK ; ABAD, TIMOTHY JOSEPH GALIMBA ; ABADAM, EDELYN JOY MACADAAN |
| APPNO_CLEAN | Identity & Demographics | numeric | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 1070637 ; 1060350 ; 1060229 |
| YEAR_INT | Time | numeric | 0.000 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 2006 ; 2007 ; 2008 |
| BDATE_CLEAN | Time | datetime-like | 14.086 | Pipeline/source field retained from NMAT+CEM+PLE integration outputs. | 08/27/1985 ; 03/18/1992 ; 06/16/1991 |
| PERSON_KEY | Identity & Demographics | text-like | 0.000 | Person-level deduplication key (normalized name + birthdate context). | ABAD, GISELLE KATIGBAK;; ; ABAD, TIMOTHY JOSEPH GALIMBA;; ; ABADAM, EDELYN JOY MACADAAN;; |

## Interpretation Tips
- Use IS_BEST_NMAT_RECORD = True for person-level trend analyses to avoid repeated-attempt inflation.
- Use IS_PLE_ANALYSIS_SAFE = True for conservative PLE-linked analyses.
- School Type_rec2_FINAL is the original school-type reference field; compare with UNI_TYPE for harmonization checks.
- For institution-level reporting, filter low-volume groups (e.g., n < 10) to reduce unstable percentages.