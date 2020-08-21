---
title: "Using scipy for data fitting"
teaching: 20
exercises: 20
questions:
- "How do I fit data to a specified function?"
- "How do I assess the quality of my fit?"
- "How do I determine the standard error for my fit parameters?"
objectives:
- "Use curve_fit from scipy to fit data to a specified functional form."
keypoints:
- "Recasting your data to numpy arrays lets you utilize features like broadcasting, which can be helpful in evaluating functions."
- "If the initial fit model does not match the data well, use the bounds argument of fit_curve to guess a range of values for the fit parameters."
---
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML"> </script> <script src="https://unpkg.com/ngl@0.10.4/dist/ngl.js"></script>


# Data fitting
Python is a power tool for fitting data to any functional form. You are no longer limited to the simple linear or polynominal functions you could fit in a spreadsheet program. You can also calculate the standard error for any parameter in a functional fit.

The basic steps to fitting data are:

1. Import the curve_fit function from scipy.
2. Create a list or numpy array of your independent variable (your x values). You might read this data in from another source, like a CSV file.
3. Create a list of numpy array of your depedent variables (your y values). You might read this data in from another source, like a CSV file.
4. Create a function for the equation you want to fit. The function should accept as inputs the dependent variable and all the parameters to be fit.
5. Use the function `curve_fit` to fit your data.
6. Extract the fit parameters from the output of `curve_fit`.
7. Use your function to calculate y values using your fit model to see how well your model fits the data.
8. Graph your original data and the fit equation.

# Fitting x, y Data
First, import the relevant python modules that will be used.  
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import curve_fit
```
{: .language-python}
Now we will consider a set of x,y-data.  This data has one independent variable (our x values) and one dependent variable (our y values).  We will recast the data as numpy arrays, so we can use numpy features when we are evaluating our data.  It is often very helpful to look at a plot of the data when deciding what functional form to fit.
```
xdata = [ -10.0, -9.0, -8.0, -7.0, -6.0, -5.0, -4.0, -3.0, -2.0, -1.0, 0.0, 1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0, 8.0, 9.0, 10.0]
ydata = [1.2, 4.2, 6.7, 8.3, 10.6, 11.7, 13.5, 14.5, 15.7, 16.1, 16.6, 16.0, 15.4, 14.4, 14.2, 12.7, 10.3, 8.6, 6.1, 3.9, 2.1]

#Recast xdata and ydata into numpy arrays so we can use their handy features
xdata = np.asarray(xdata)
ydata = np.asarray(ydata)
plt.plot(xdata, ydata, 'o')
```
{: .language-python}

<img src = '../fig/03-data.png'>

This data could probably be fit to many functional forms. We will try two different functional forms. (Looking at data and knowing what function it might fit is non-trivial and beyond the scope of this lesson. For purposes of this lesson, we will simply fit the data to given functional forms.)
1.  Gaussian Function: $ y = A e^{-Bx^2} $
2.  Cosine Function: $ D cos (E x) $

## Example 1 - the Gaussian function
First, let's fit the data to the Gaussian function.  Our goal is to find the values of A and B that best fit our data.  First, we need to write a python function for the Gaussian function equation.  The function should accept as inputs the independent varible (the x-values) and all the parameters that will be fit.

```
# Define the Gaussian function
def Gauss(x, A, B):
    y = A*np.exp(-1*B*x**2)
    return y
```
{: .language-python}

We will use the function `curve_fit` from the python module `scipy.optimize` to fit our data.  It uses non-linear least squares to fit data to a functional form.   You can learn more about `curve_fit` by using the `help` function within the Jupyter notebook or from the [scipy online documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.curve_fit.html).

The `curve_fit` function has three required inputs: the function you want to fit, the x-data, and the y-data you are fitting.  There are two outputs.  The first is an array of the optimal values of the parameters. The second a matrix of the estimated covariance of the parameters from which you can calculate the standard error for the parameters.

```
parameters, covariance = curve_fit(Gauss, xdata, ydata)
```
{: .language-python}

The optimized values of A and B are now stored in the list `parameters`.  From this, we can extract our best fit values of A and B and print them.
```
fit_A = parameters[0]
fit_B = parameters[1]
print(fit_A)
print(fit_B)
```
{: .language-python}
```
16.934286340519687
0.015739600927241554
```
{: .output}

Now we want to see how well our fit equation matched our data. To do this, we will calculate values of y, using our function and the fit values of A and B, and then we will make a plot to compare those calculated values to our data.
```
fit_y = Gauss(xdata, fit_A, fit_B)
plt.plot(xdata, ydata, 'o', label='data')
plt.plot(xdata, fit_y, '-', label='fit')
plt.legend()
```
{: .language-python}

<img src = '../fig/03-gauss_fit.png'>

Looks like a good fit!  

To calculate the standard error of the parameters from the covariance, you take the square root of the diagonal elements of the matrix.  You can do this in one line using functions from `numpy`.
```
SE = np.sqrt(np.diag(covariance))
SE_A = SE[0]
SE_B = SE[1]

print(F'The value of A is {fit_A:.5f} with standard error of {SE_A:.5f}.')
print(F'The value of B is {fit_B:.5f} with standard error of {SE_B:.5f}.')
```
```
The value of A is 16.93429 with standard error of 0.35658.
The value of B is 0.01574 with standard error of 0.00087.
```
{: .language-python}

## Example 2 - the cosine function

The cosine function proves to be a bit trickier. If we approach the problem as we do above, we see from the graph that we don't get a good fit.

```
def cos_func(x, D, E):
    y = D*np.cos(E*x)
    return y

    parameters, covariance = curve_fit(cos_func, xdata, ydata)
    fit_D = parameters[0]
    fit_E = parameters[1]

    fit_cosine = cos_func(xdata, fit_D, fit_E)

    plt.plot(xdata, ydata, 'o', label='data')
    plt.plot(xdata, fit_cosine, '-', label='fit')
```
{: .language-python}

<img src = '../fig/03-cosine_fit1.png'>

To fix this, we need to give a guess for what we think our parameters are.  Thinking about the form of the cosine function, the height of the function is controlled by the D parameter.  Looking at our graph, it seems the value of D is somewhere between 15 and 17.  Similarly, the E parameter tells us how many cycles occur over the 0 to 2$\pi$ interval.  This is a very wide graph; there is clearly much less than one cycle between 0 and 2$\pi$, so we will guess that E is between 0 and 1.  To incorporate these guesses into our code, we create two new arrays.  One array specifies the lower bound for each parameter; the other specifies the upper bound for each parameter.  We then specify these bounds when we call `curve_fit`.

```
lower = [15, 0]
upper = [17, 1]
parameters, covariance = curve_fit(cos_func, xdata, ydata, bounds=(lower, upper) )
fit_D = parameters[0]
fit_E = parameters[1]

fit_cosine = cos_func(xdata, fit_D, fit_E)

plt.plot(xdata, ydata, 'o', label='data')
plt.plot(xdata, fit_cosine, '-', label='fit')
```
{: .language-python}

<img src = '../fig/03-cosine_fit2.png'>

Now that looks like a good fit!  

Frequently, you will have to adjust your guesses to get a good fit for your data.  If you understand the physical significance of your data and the equation you are trying to fit, you will have an easier time fitting your data.

> ## Exercise
> Calculate the standard error for the D and E parameters. Print each parameter and its standard error.
>> ## Solution
>> ~~~
>> SE = np.sqrt(np.diag(covariance))
>> SE_D = SE[0]
>> SE_E = SE[1]
>>
>> print(F'The values of D is {fit_D:.5f} and the standard error is {SE_D:.5f}.')
>> print(F'The value of E is {fit_E:.5f} and the standard error is {SE_E:.5f}.')
>> ~~~
>> {: .language-python}
>> ~~~
>> The values of D is 16.31473 and the standard error is 0.11091.
>> The value of E is 0.14649 and the standard error is 0.00090.
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}

> ## Exercise - Fitting a Lennard-Jones potential
> Consider the following data computed for a helium dimer.  The interaction energy at several different internuclear separations is given.  Fit this data to a Lennard-Jones potential.
> $$ V = 4 \varepsilon \left( \left( \sigma/r \right)^{12} - \left( \sigma/r \right)^6 \right) $$
>
> ~~~
> # Internuclear separation in angstroms
> distances = [2.875, 3.0, 3.125, 3.25, 3.375, 3.5, 3.75, 4.0, 4.5, 5.0, 6.0]
> # Energy in Wavenumbers
> energies = [0.35334378061169025, -2.7260131253801405, -4.102738968283382, -4.557042640311599, -4.537519193684069, -4.296388508321034, -3.6304745046204117, -3.0205368595885536, -2.1929538006724814, -1.7245616790238782, -1.2500789753171557]
> ~~~
> {: .language-python}
>
>> ## Solution
>> ~~~
>> # Internuclear separation in angstroms
>> distances = [2.875, 3.0, 3.125, 3.25, 3.375, 3.5, 3.75, 4.0, 4.5, 5.0, 6.0]
>> # Energy in Wavenumbers
>> energies = [0.35334378061169025, -2.7260131253801405, -4.102738968283382, -4.557042640311599, -4.537519193684069, -4.296388508321034, -3.6304745046204117, -3.0205368595885536, -2.1929538006724814, -1.7245616790238782, -1.2500789753171557]
>>
>> distances = np.asarray(distances)
>> energies = np.asarray(energies)
>>
>> def LJ_func(r, epsilon, sigma):
>>     V = 4*epsilon*((sigma/r)**12-(sigma/r)**6)
>>     return V
>>
>> parameters, covariance = curve_fit(LJ_func, distances, energies)
>>
>> fit_epsilon = parameters[0]
>> fit_sigma = parameters[1]
>>
>> print(F'The value of epsilon is {fit_epsilon:.3f} wavenumbers.')
>> print(F'The value of sigma is {fit_sigma:.3f} angstroms.')
>>
>> fit_energies = LJ_func(distances, fit_epsilon, fit_sigma)
>>
>> plt.plot(distances, energies, 'o', label='data')
>> plt.plot(distances, fit_energies, '-', label='LJ fit')
>> ~~~
>> {: .language-python}
>> ~~~
>> The value of epsilon is 4.857 wavenumbers.
>> The value of sigma is 2.893 angstroms.
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}





{% include links.md %}
