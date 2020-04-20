# AugBin

## [![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/dwyl/esta/issues)

## Description
AugBin is a Shiny app which can be used to implement efficient analysis of mixed outcome composite responder endpoints. The analysis is based on using latent variable methods to model the underlying structure of the composite endpoint, so that information in continuous components can be retained. The app also implements the commonly used 'standard binary' approach for comparison, which treats the composite as a single binary endpoint. 

The tutorial below uses an example dataset to provide step-by-step guidance for using the AugBin Shiny app to analyse mixed outcome continuous and discrete composite responder endpoints. In the case that further queries arise about the functionality of the app for specific applications, contact Martina McMenamin at <martina.mcmenamin@mrc-bsu.cam.ac.uk>.

## Getting started

To access the AugBin GUI, go to https://martinamcm.shinyapps.io/augbin/. 

An example dataset for a composite with one continuous and one binary component is available in the repository.

## Tutorial

### Uploading data
A simulated dataset where the composite endpoint is comprised of one continuous and one binary outcome is used for illustration. An important first step before using the app is to have the dataset in the required format. The columns should be organised as follows:
* patient ID 
* treatment allocation
* Observed continuous outcome measure(s) (may be one or two depending on the composite endpoint)
* Observed binary measure (if it exists)
* Observed continuous baseline measure(s) 

After uploading the data, the raw data table will displayed as shown.

<p align="center">
<img src="/Images/SS1.png" title="SS1" width="90%" />
</p>

### Visualising data
The raw data can then be visualised by group using a boxplot, histogram, density, bar graph or violin plot, depending on the outcome type. 

<p align="center">
<img src="/Images/SS2.png" title="SS2" width="75%" align="center" />
</p>
 
The example below shows a boxplot of the continuous outcome variable by treatment arm. The raw data points may be included or excluded from the plots as shown.

<p align="center">
<img src="/Images/SS3.png" title="SS3" width="48%" align="center"/> <img src="/Images/SS4.png" title="SS4" width="48%" align="center"/>
</p>

### Composite structure
The 'Composite Structure' panel allows the user to select the number of continuous and binary outcomes that make up the composite endpoint. This may be one or two continuous measures and zero or one binary measures. Note that the composite may include any number of discrete variables however these must be combined to a single binary discrete response indicator. Additional continuous measures may also be incorporated in this way. 

'Generate Model' will show the corresponding model fitted, which is shown below for this example. For further information on the model and how the probability of response and standard errors are obtained see <https://arxiv.org/abs/1902.07037>. 

<p align="center">
<img src="/Images/SS5.png" title="SS5" width="85%" />
</p>

When the composite combines any mixture of continuous and discrete components the latent variable model is used. For two continuous and one binary it takes the form shown below.

<p align="center">
<img src="/Images/SS8.png" title="SS8" width="85%" />
</p>

If the composite is comprised only of continuous measures a normal distribution of the appropriate dimension is assumed. 

<p align="center">
<img src="/Images/SS6.png" title="SS6" width="85%" /> <img src="/Images/SS7.png" title="SS7" width="85%" />
</p>

### Analysis
The Analysis tab allows the user to enter the dichotomisation threshold for the continuous components. The input depends on the structure selected as shown.

<p align="center">
<img src="/Images/SS13.png" title="SS13" width="17%" /> <img src="/Images/SS13a.png" title="SS13a" width="81%" /> 
<img src="/Images/SS14.png" title="SS14" width="22%" /> <img src="/Images/SS14a.png" title="SS14a" width="75%" />
</p>

Clicking the 'Run Analysis' button starts the analysis. This stage involves the following steps:
1. The model is fit and maximum likelihood parameter estimates are obtained
2. The covariance matrix of the parameter estimates is obtained by inverting the Hessian
3. Numerical integration is implemented to obtain the probability of response in each arm
4. Treatment effects in the form of the log-odds ratio, log-risk ratio and risk difference are calculated
5. Partial derivatives of the treatment effect estimates with respect to the parameter estimates are computed
6. Standard error estimates for the treatment effect estimates are obtained using the delta method 
7. These are used to give 95% confidence intervals
8. A logistic regression model on the overall response endpoint is implemented for comparison (Standard Binary method)

This process may take anywhere between a few seconds and a few minutes depending on the complexity of the endpoint. A loading bar will show to indicate when the analysis is still running and if any errors occur then they will be displayed in the Analysis panel. The output is as shown below where the probability of response in each arm for both methods is shown in a table. The plots indicate the treatment effect point estimates and 95% confidence intervals. Note that the improvement in efficiency from the augmented approach depends on the response in each arm and the dichotisation threshold, as illustrated.

<p align="center">
<img src="/Images/SS9.png" title="SS9" width="80%" /> <img src="/Images/SS10.png" title="SS10" width="80%" />
</p>

#### Transforming continuous components
The continuous outcome data may be transformed using a Box-Cox transformation, more details of which are available at <https://doi.org/10.1111/j.2517-6161.1964.tb00553.x>. This is achieved by using the selection shown below and executed using the 'Run Analysis' button. 

<p align="center">
<img src="/Images/SS11.png" title="SS11" width="80%" />
</p>

#### Goodness-of-fit
The final panel shows the goodness-of-fit for the model fitted in the augmented approach. This is evaluated using modified Pearson residuals, which are shown in the left hand plot. The right hand plot shows the density of the residuals which should follow a chi-squared distribution if the model is a good fit.  

<p align="center">
<img src="/Images/SS12.png" title="SS12" width="80%" />
</p>

