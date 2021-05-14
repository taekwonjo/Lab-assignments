# Lab-assignments
SPSS syntax codes to lab assignments in SIMM32


DATASET ACTIVATE DataSet1.
FREQUENCIES VARIABLES=pain ID sex age STAI_trait pain_cat cortisol_serum cortisol_saliva 
    mindfulness weight IQ household_income
  /STATISTICS=VARIANCE MINIMUM MAXIMUM MEAN SKEWNESS SESKEW
  /HISTOGRAM NORMAL
  /ORDER=ANALYSIS.

DESCRIPTIVES VARIABLES=pain_cat pain age STAI_trait cortisol_serum cortisol_saliva mindfulness 
    weight IQ household_income
  /STATISTICS=MEAN STDDEV MIN MAX SKEWNESS.

SPSSINC CREATE DUMMIES VARIABLE=sex
ROOTNAME1=female_dummy
/OPTIONS ORDER=A USEVALUELABELS=YES USEML=YES OMITFIRST=NO.

COMPUTE cortisol_sum=cortisol_serum + cortisol_saliva.
EXECUTE.



REGRESSION
  /MISSING LISTWISE
  /STATISTICS COEFF OUTS R ANOVA SELECTION
  /CRITERIA=PIN(.05) POUT(.10)
  /NOORIGIN 
  /DEPENDENT pain
  /METHOD=ENTER age female_dummy_1
  /METHOD=ENTER mindfulness STAI_trait pain_cat cortisol_sum
  /RESIDUALS DURBIN.

DATASET ACTIVATE DataSet1.
REGRESSION
  /MISSING LISTWISE
  /STATISTICS COEFF OUTS CI(95) BCOV R ANOVA COLLIN TOL
  /CRITERIA=PIN(.05) POUT(.10)
  /NOORIGIN 
  /DEPENDENT pain
  /METHOD=ENTER age female_dummy_1
  /METHOD=ENTER mindfulness STAI_trait pain_cat cortisol_sum
  /SCATTERPLOT=(*ZRESID ,*ZPRED)
  /RESIDUALS NORMPROB(ZRESID)
  /SAVE PRED COOK RESID.
