### Evaluating Ethnicolr

We evaluate some of the [ethnicolr models](https://github.com/appeler/ethnicolr) on the [NC Voter Registration Data](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/NEFUBN) (access limited to researchers with university affiliation). There are some challenges in evaluation given how race and ethnicity are coded varies across the two states.

### Measuring Race and Ethnicity

North Carolina distinguishes between race and ethnicity and has two columns. Here's the codebook:

```
/ ***************************************************************************
Race codes
race               description
*******************************************************************************
A                  ASIAN
B                  BLACK or AFRICAN AMERICAN
I                  AMERICAN INDIAN or ALASKA NATIVE
M                  TWO or MORE RACES
O                  OTHER
P                  NATIVE HAWAIIAN or PACIFIC ISLANDER
U                  UNDESIGNATED
W                  WHITE
*************************************************************************** /

/ ***************************************************************************
Ethnic codes
ethnicity          description
*******************************************************************************
HL                 HISPANIC or LATINO
NL                 NOT HISPANIC or NOT LATINO
UN                 UNDESIGNATED
*************************************************************************** /
```

FL codebook is as follows:

![](img/fl_race_code.png)

### Analyses

We start by presenting a full cross-tabulation of prediction from the FL full name model and NC concatenation of race and ethnicity, e.g., Asian--HL, Asian--NL, etc.

Next we present crosstabs that merge some categories. We propose two coding schemes --- Low FP and High FP. Both schemes code the first list item the same way. For the

1. Between FL and NC, the clearest 1 to 1 mapping is for the following:
  * (FL) White, Not Hispanic --> (NC) W--NL
  * (FL) Black, Not Hispanic --> (NC) B--NL

  We leave those categories as is. For other categories, we need to make assumptions.

2. We try two mappings for (FL) Hispanic:
  * Low FP/High FN: (NC) HL (W--HL, B--HL) (as W--NL and B--NL are defined in FL)
  * High FP/Low FN: (NC) HL (all categories)

3. For (FL) Asian or Pacific Islander, American Indian or Alaskan Native, Other, Multi-racial, and Unknown, we try the same two codings:
  * Low FP/High FN: (NC) A-NL, I-NL, etc. For Unknown, we use U-UN as the lowest FP coding scheme.
  * High FP/Low FN: (NC) A (all ethnic codes), (NC) I (all ethnic codes), etc

### NC Ethnicolr Model(s)

We build new LSTM models based on NC data. We start by assuming y = concatenation of ethnic code and race code. We remove U and also UN --- assuming they are 'missing at random.' This gives us 12 categories.
