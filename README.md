java cHomework Assignment 1
Linear Regression
NEKN96
Guidelines
1. Upload the HWA in .zip format to Canvas before the 2nd of October, 23:59, and only
upload one HWA for each group. The .zip ffle should contain two parts:
- A report in .pdf format, which will be corrected.
- The code you used to create the output/estimates for the report. The code itself will
not be graded/corrected and is only required to conffrm your work. The easiest is to add
the whole project folder you used to the zip ffle.
1 However, if you have used online tools,
sharing a link to your work is also ffne.
2
2. The assignment should be done in groups of 3-4 people, pick groups at
Canvas → People → Groups.
3
3. Double-check that each group member’s name and ID number are included in the .pdf ffle.
4. To receive your ffnal grade on the course, a PASS is required on this HWA.
- If a revision is required, the comments must be addressed,  However, you are only guaranteed an additional
evaluation of the assignment in connection to an examination period.
4
You will have a lot of ffexibility in how you want to solve each part of the assignment, and all things
that are required to get a PASS are denoted in bullet points:
 . . .
Beware, some things require a lot of work, but you should still only include the ffnal table or ffgure
and not all intermediary steps. If uncertain, add a sentence or two about how you reached your
conclusions, but do not add supplementary material. Only include the tables/ffgures explicitly asked
for in the bullet points.
Good Luck!
1Before uploading the code, copy-paste the project folder to a new directory and try to re-run it. Does it still work?
2Make sure the repository/link is public/working before sharing it.
3Rare exceptions can be made if required. In that case,4Next is the retake on December 12th, 2024.
1NEKN96
Assignment
Our goal is to put into practice the separation of population vs. sample using a linear regression
model. This hands-on approach will allow us to generate a sample from a known Population Regression
Function (PRF) and observe how breakages of the Gauss-Markov assumptions can affect our sample
estimates.
We will assume that the PRF is:
Y = α + β1X1 + β2X2 + β3X3 + ε (1)
However, to break the assumptions, we need to add:
A0: Non-linearities
A2: Heteroscedasticity
A4: Endogeneity
A7: Non-normality in a small sample
A3 autocorrelation will be covered in HWA2, time-series modelling.
Q1 - All Assumptions Fulfflled
Let’s generate a ”correct” linear regression model. Generate a PRF with the parameters:
α = 0.7, β1 = −1, β2 = 2, β3 = 0.5, ε ∼ N(0, 4), Xi
 iid∼ N(0, 1). (2)
The example code is also available in Canvas
Setup Parameters
n = 30
p = 3
beta = [-1, 2, 0.5]
alpha = 0.7
Simulate X and Y, using normally distributed errors
5
np. random . seed ( seed =96)
X = np. random . normal (loc=0, scale =1, size =(n, p))
eps = np. random . normal (loc =0, scale =2, size =n)
y = alpha + X @ beta + eps
Run the correctly speciffed linear regression model
result_OLS = OLS( endog =y, exog = add_constant (X)). fit ()
result_OLS . summary ()
 Add a well-formatted summary table
 Interpret the estimate of βˆ
2 and the R2
.
5
Important: The np.random.seed() will ensure that we all get the same result. In other words, ensure that we are
using the ”correct” seed and that we don’t generate anything else ”random” before this simulation.
2NEKN96
 In a paragraph, discuss if the estimates are consistent with the population regression function.
Why, why not?
 Re-run the代 写NEKN96、Python/Java
代做程序编程语言 model, increasing the sample size to n = 10000. In a paragraph, explain what happens
to the parameter estimates, and why doesn’t R2 get closer and closer to 1 as n increases?
Q2 - Endogeneity
What if we (wrongly) assume that the PRF is:
Y = α + β1X1 + β2X2 + ε (3)
Use the same seed and setup as in Q1, and now estimate both the ”correct” and the ”wrong” model:
result_OLS = OLS( endog =y, exog = add_constant (X)). fit ()
result_OLS . summary ()
result_OLS_endog = OLS ( endog =y, exog = add_constant (X[:,0:2 ])). fit ()
result_OLS_endog . summary ()
 Shouldn’t this imply an omitted variable bias? Show mathematically why it won’t be a problem
in this speciffc setup (see lecture notes ”Part 2 - Linear Regression”).
Q3 - Non-Normality and Non-Linearity
Let’s simulate a sample of n = 3000, keeping the same parameters, but adding kurtosis and skewness
to the error terms:
6
n = 3000
X = np. random . normal (loc=0, scale =1, size =(n, p))
eps = np. random . normal (loc =0, scale =2, size =n)
eps_KU = np. sign ( eps) * eps **2
eps_SKandKU_tmp = np. where ( eps_KU > 0, eps_KU , eps_KU *2)
eps_SKandKU = eps_SKandKU_tmp - np. mean ( eps_SKandKU_tmp )
Now make the dependent variable into a non-linear relationship
y_exp = np.exp( alpha + X @ beta + eps_SKandKU )
 Create three ffgures:
1. Scatterplot of y exp against x 1
2. Scatterplot of ln(y exp) against x 1
3. plt.plot(eps SKandKU)
The ffgure(s) should have a descriptive caption, and all labels and titles should be clear to the
reader.
Estimate two linear regression models:
6The manual addition of kurtosis and skewness will make E [ε] ̸= 0, so we need to remove the average from the errors
to ensure that the exogeneity assumption is still fulfflled.
3NEKN96
res_OLS_nonLinear = OLS( endog =y_exp , exog = add_constant (X)). fit ()
res_OLS_transformed = OLS ( endog =np.log ( y_exp ), exog = add_constant (X)). fit ()
 Add the regression tables of the non-transformed and transformed regressions
 In a paragraph, does the transformed model fft the population regression function?
Finally, re-run the simulations and transformed estimation with a small sample, n = 30
 Add the regression table of the transformed small-sample estimate
 Now, re-do this estimate several times
7 and observe how the parameter estimates behave. Do
the non-normal errors seem to be a problem in this spot?
Hint: Do the parameters seem centered around the population values? Do we reject H0 : βi = 0?
 In a paragraph, discuss why assuming a non-normal distribution makes it hard to ffnd the
distributional form under a TRUE null hypothesis, H0 ⇒ Distribution?
Hint: Why is the central limit theorem key for most inferences?
Q4 - Heteroscedasticity
Suggest a way to create heteroscedasticity in the population regression function.
8
 Write down the updated population regression function in mathematical notation
 Estimate the regression function assuming homoscedasticity (as usual)
 Adjust the standard errors using a Heteroscedastic Autocorrelated Consistent (HAC) estimator
(clearly state which HAC estimator you use)
 Add the tables of both the unadjusted and adjusted estimates
 In a paragraph, discuss if the HAC adjustment to the standard errors makes sense given the
way you created the heteroscedasticity. Did the HAC adjustment seem to ffx the problem?
Hint: Bias? Efffcient?
7Using a random seed for each estimate.
8Tip: Double-check by simulating the model and plotting the residuals against one of the regressors. Does it look
heteroscedastic?
4

         
加QQ：99515681  WX：codinghelp
