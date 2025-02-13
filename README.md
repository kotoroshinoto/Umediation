## Umediation
The Umediation R package enables the user to simulate unmeasured confounding in mediation analysis in order to see how the results of the mediation analysis would change in the presence of unmeasured confounding.

## Installation
```
install.packages("devtools") #The devtools package must be installed first
install.packages("mediation") #The mediation package must be installed first
install.packages("car") #The car package must be installed first

devtools::install_github("SharonLutz/Umediation")
```
## Example
Below, we simulate 4 unmeasured confounders U (2 normally distributed and 2 Bernouilli distributed random variables) on the binary exposure, A, normally distributed mediator, M, and normally distributed outcome Y adjusted for one normally distributed covariate and 2 binary distributed covariates.
```
library(Umediation)
?Umediation # For details on this function and how to choose input variables

testM<- Umediation(n=1000,Atype="D",Mtype="C",Ytype="C",Ctype=c("C","D","D"),Utype=c("C","D","D","C"),interact=TRUE,muC=c(0.1,0.3,0.2),
varC=c(1,1,1),muU=c(.1,0.3,0.2,.1),varU=c(1,1,1,1),gamma0=0,gammaC=c(1,0.3,0.2),gammaU=c(1,0.3,0.2,0.4),varA=1,alpha0=0,alphaA=1,
alphaC=c(0.3,0.2,0.2),alphaU=c(0.3,0.2,0.3,0.2),varM=1,beta0=0,betaA=-1,betaM=1,betaI=1,betaC=c(0.3,0.2,0.1),betaU=c(0.3,0.2,-1.3,0.2),
varY=1,alpha=0.05,nSim=100,nBoot=400)

testM

```

## Output
For this analysis, we can see that there is not a significant difference in the proportion of simulations for the mediated effect if the unmeasured confounders are included, but there is a large diffence in the inference for the direct effect if these unmeasured confounders are not included in the analysis.

```
$Results
                                                                               [,1]
Prop. of simulations w/ significant ACME excluding U                    1.000000000
Prop. of simulations w/ significant ACME including U                    1.000000000
Prop. of simulations where conclusions based on ACME match              1.000000000
Average ACME excluding U                                                2.061403587
Average ACME including U                                                1.494588641
Average absolute difference of ACME including U minus ACME excluding U  0.566814946
Prop. of simulations w/ significant ADE excluding U                     0.030000000
Prop. of simulations w/ significant ADE including U                     0.450000000
Prop. of simulations where conclusions based on ADE match               0.520000000
Average ADE excluding U                                                 0.006946501
Average ADE including U                                                -0.185434050
Average absolute difference of ADE including U minus ADE excluding U    0.192380551

$Correlations_Between_Variables
      A    M     Y    C1    C2    C3    U1    U2    U3    U4
A  1.00 0.56  0.49  0.31  0.04  0.07  0.37  0.05  0.03  0.16
M  0.56 1.00  0.87  0.33  0.08  0.09  0.35  0.11  0.15  0.26
Y  0.49 0.87  1.00  0.37  0.09  0.08  0.38  0.11 -0.08  0.27
C1 0.31 0.33  0.37  1.00  0.04  0.01 -0.03 -0.01  0.00 -0.01
C2 0.04 0.08  0.09  0.04  1.00  0.03 -0.03  0.02  0.00  0.02
C3 0.07 0.09  0.08  0.01  0.03  1.00  0.01  0.01  0.02 -0.01
U1 0.37 0.35  0.38 -0.03 -0.03  0.01  1.00 -0.01  0.00 -0.04
U2 0.05 0.11  0.11 -0.01  0.02  0.01 -0.01  1.00  0.01 -0.02
U3 0.03 0.15 -0.08  0.00  0.00  0.02  0.00  0.01  1.00  0.04
U4 0.16 0.26  0.27 -0.01  0.02 -0.01 -0.04 -0.02  0.04  1.00

$Warning
[1] "Warning: correlations are only valid if at least one of the variables is normally distributed."

```

## Speed Improvements using C++ Bindings and Threading
There is another version of this package which implements support for threading and a version of the mediation algorithm implemented using RcppEigen which can provide a significant boost in speed akin to multiprocessing but with less of a memory footprint.

Find it here [UmediationThread](https://github.com/SharonLutz/UmediationThread)

## Reference
**Lutz SM**, Thwing A, Schmiege S, Kroehl M, Baker CD, Starling AP, Hokanson JE, Ghosh D. (2017) Examining the Role of Unmeasured Confounding in Mediation Analysis with Genetic and Genomic Applications. *BMC Bioinformatics.* 18(1):344.

