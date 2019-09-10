---
title: "Working with Numpy Arrays"
teaching: 45
exercises: 20
questions:
- "What are the differences between numpy arrays and lists?"
- "How can I use NumPy to do calculations?"
objectives:
- "Be able to name the differences between Python lists and numpy arrays."
- "Understand the idea of broadcasting."
keypoints:
- "NumPy arrays which are the same size use element-wise operations when added or subtracted"
- "NumPy uses something called *broadcasting* for arrays which are not the same size to allow arrays to be added or multiplied."
- "NumPy has extensive documentation online - you should check this out if you need to do a computation."
---

[Numpy](https://numpy.org/) is a widely used Python library for scientific computing. It has a number of useful features, including the a data structure called an array. Compared to the built-in data typles `lists` which we discussed in the [Python Data and Scripting Workshop](https://molssi-education.github.io/python_scripting_cms/), `numpy` has many features which can help you in your data analysis.

## NumPy Arrays vs. Python Lists

Previously, you have worked with the built-in types of `lists`. NumPy arrays seem similar, but offer some distinct advantages. 

Numpy arrays take up less space, are faster, and have more mathematical operations associated with them. However, unlike lists, they elements all have to be the same type.

There are also differences in how lists and numpy arrays behave. Let's look at some of these.

First open a Jupyter notebook to record your work. 

To use the numpy library, we have to import it. When `numpy` is imported, it is often shortened to `np`. 

~~~
import numpy as np

# Create some lists
r1 = [0.0, 0.1, 0.0]
r2 = [0.0, 0.0, 0.0]

# Create some numpy arrays
r1_array = np.array(r1)
r2_array = np.array(r2)
~~~
{: .language-python}

In this code block, we've created two lists (`r1` and `r2`). We then created numpy array versions of these lists by calling `np.array(list_name)`.

When we print `r1` and `r1_array`, the results look very similar, but we see that they are different types.

~~~
print(r1)
print(r1_array)
~~~
{: .language-python}

~~~
[0.0, 0.1, 0.0]
[0.  0.1 0. ]
~~~
{: .output}

~~~
print(type(r1))
print(type(r1_array))
~~~
{: .language-python}

~~~
<class 'list'>
<class 'numpy.ndarray'>
~~~
{: .output}

One way numpy arrays and lists are different is that you can easily perform element-wise operations on numpy arrays without loops. **This is advantageous because loops in Python are slow. However, `numpy` functions are optimized to run quickly.**  You can make your code much faster if you use numpy element-by-element operations instead of loops. 

You can add two arrays together, multiply arrays by scalars, or do element-wise multiplcation of arrays.

For example, you can multiply two numpy arrays to get their element-wise product. This means that given two vectors `a = np.array([a0, a1, a2])` and `b = np.array([b0, b1, b2])`, `a * b = [a0*b0, a1*a1, a2*b2]`.

In contrast, if `a` and `b` were lists, you would get an error. 

> ## Check your understanding
> Consider the variable definitions for `a1` and `a2`. What does each print statment result in?
>
> ~~~
> a1 = np.array([2, 1, 0])
> a2 = np.array([1, 3, 5])
> 
> print(a1 * a2)
> print(a1 + a2)
> ~~~
> {: .language-python}
> 
>
>> ## Answer
>> 
>> The code block results in 
>> ~~~
>> [2 3 0]
>> [3 4 5]
>> ~~~
>> {: .output}
>> For the first line, the first element is `a1[0]*a2[0]`, the second element is `a1[1]*a2[1]`, and the third element is `a1[2]*a2[2]`.
>> For the second line, the first element is `a1[0]+a2[0]`, second is `a1[1]+a2[1]`, third is `a1[2]+a2[2]`.
>>
> {: .solution}
{: .challenge}

> ## Check your understanding
> What happens if `a1` and `a2` are lists? Consider each `print` statement separately.
>
> ~~~
> a1 = [2, 1, 0]
> a2 = [1, 3, 5]
> 
> print(a1 + a2)
> print(a1 * a2)
> ~~~
> {: .language-python}
> 
>> ## Answer
>> 
>> The first print statement results in concatenation of the lists.
>> ~~~
>> [2, 1, 0, 1, 3, 5]
>> ~~~
>> {: .output}
>> 
>> Second print statement results in a TypeError. Two lists cannot be multiplied. 
>> ~~~
>> TypeError: can't multiply sequence by non-int of type 'list
>> ~~~
>> {: .error}
>> This is because two lists cannot be multiplied. If you wanted to do element-wise multiplication, you would have to use `numpy` arrays (like in the previous exercise.)
>>
> {: .solution}
{: .challenge}

## Making copies 

One important aspect of mutable objects in Python to be aware of is behavior when you try to make copies. Imagine a situation where you are trying to make a copy of a list.

~~~
my_list = [1, 2, 3]
my_list_copy = my_list

my_list_copy[0] = 100
print(my_list_copy)
~~~
{: .language-python}

~~~
[100, 2, 3]
~~~
{: .output}

This is probably what you expected. But, what if we print our original list?

~~~
print(my_list)
~~~
{: .language-python}

~~~
print(my_list)
~~~
{: .language-python}

~~~
[100, 2, 3]
~~~
{: .output}

This is probably **not** what you expected. The statements `my_list_copy = my_list` did not create a new copy of our list. Instead, it pointed back at the original list! The vocabulary for this is that Python is a *pass by reference* language. When you set `my_list_copy = my_list`, `my_list_copy` is literally set equal to `my_list`, *not* the values in the list. 

If you truly wanted to create a copy, you would use the `.copy()` method on the list.

~~~
my_list = [1, 2, 3]
my_list_copy = my_list.copy()

my_list_copy[0] = 100
print(my_list_copy)
print(my_list)
~~~
{: .language-python}

~~~
my_list = [1, 2, 3]
my_list_copy = my_list.copy()

my_list_copy[0] = 100
print(my_list_copy)
print(my_list)
~~~
{: .language-python}

~~~
[100, 2, 3]
[1, 2, 3]
~~~
{: .output}

This behavior also occurs with numpy arrays.

~~~
r1_copy = r1
r1_copy[0] = 100

print(r1)
print(r1_copy)
~~~
{: .language-python}

A difference between numpy arrays and lists can be seen when you take slices.

~~~
r1_slice = r1[1:]
r1_array_slice = r1_array[1:]
~~~
{: .language-python}

What do you notice about the original array if you try to overwrite the first element of each slice?

When you slice a numpy array, the slice will point back to the original array. If you would truly like to make a copy, use the `.copy()` function.


## Analyzing Tabular Data

Let's consider tabular data from the Python Data and Scripting Lesson. If you have not completed that lesson, you can download the files for it [here](https://molssi-education.github.io/python_scripting_cms/data/data.zip).

Put the `data` folder from this link into the directory where you are working with your jupyter notebook. We will be working with a file called `distance_data_headers.csv`.

> ## Exercise
> Use the function [np.genfromtxt](https://docs.scipy.org/doc/numpy/reference/generated/numpy.genfromtxt.html) to read the data from the file `distance_data_headers.csv`. Save everything from row 2 to the end of the file as a numpy array named `data` with type `float`. Save the first row as a `numpy array` with type `str`. 
> 
> **Hint** - If you need help, see the [Tabular Data Lesson] on the [Python Data and Scripting Workshop](https://molssi-education.github.io/python_scripting_cms).
>> ## Solution
>> Here is one code block which achieves the goal. You may do it differently. It is just important to have the variables as described in the exercise.
>>
>> ~~~
>> import os
>> import numpy as np
>> 
>> distance_file = os.path.join('data','distance_data_headers.csv')
>> distances = np.genfromtxt(fname=distance_file, delimiter=',', dtype='unicode')
>> headers = distances[0]
>> data = distances[1:]
>> data = data.astype(numpy.float)
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

In a previous lesson, we calculated the mean of each column of the array using the `range` function and a `for` loop. This was good because it reminded us of the `range` function and how `for` loops worked. However, the `numpy.mean` function will let us do that without a `for` loop.

> ## Exercise
> Check out the [numpy documentation](https://docs.scipy.org/doc/numpy/reference/generated/numpy.mean.html) for the mean function. Can you figure out what argument we might change to get the mean of each column?
>> ## Solution
>> We will use the `axis` argument. If we do not specify `axis`, the mean of the entire array is computed. For the `axis` argument, rows correspond to axis 0, and columns correspond to axis 1. This is similar to slices where you give the indices as `row, column`.
> {: .solution}
{: .challenge}

~~~
column_averages = np.mean(data, axis=0)
print(column_averages)
~~~
{: .language-python}

~~~
[5000.5          10.87695093    7.34234496   11.20979133   10.9934435 ]
~~~
{: .output}

Now, the mean function has returned a `numpy` array to us with the same number of elements as we have columns. The 0th element of the variable `column_averages` corresponds the mean of column index 0, the second number (index 1) corresponds to the mean of column index 1. 

This will work with slices as well. If we wanted to only calculate the mean of columns 1 and 2, we could use slices inside the `mean` function call.

~~~
avg_col_1_2 = np.mean(data[:, 1:3], axis=0)
print(avg_col_1_2)
~~~
{: .language-python}

~~~
[10.87695093  7.34234496]
~~~
{: .output}

> ## Don't forget the `axis` argument!
> If you forget to specify the `axis` argument, you will only get one value.
> ~~~
> avg_col_1_2 = np.mean(data[:, 1:3])
> print(avg_col_1_2)
> ~~~
> {: .language-python}
> 
> ~~~
> 9.109647945
> ~~~
> {: .output}
> Here, `9.109647945` is the mean of all the values in row 1 and in row2. Be sure to specify the `axis` argument unless you really just want the average of all the values.
{: .callout}

> ## Check your understanding
> Within in your mean function, take a slice of the data such that you only get the mean of the columns of data, i.e. exclude the frame numbers column.
> 
> Save your answer as a variable called `data_averages`.
>> ## Answer
>> We would add slicing to `data` in our mean calculation. Recall from the lesson on Tabular Data in the Python Data and Scripting workshop, that slicing is `[row, column]`.
>> ~~~
>> data_averages = np.mean(data[:,1:], axis=0)
>> print(data_averages)
>> ~~~
>> {: .language-python}
>> ~~~
>> [10.87695093  7.34234496 11.20979133 10.9934435 ]
>> ~~~
>> {: .output}
>> {: .language-python}
> {: .solution}
{: .challenge}

## Logical comparisons

We can also do logical comparisons on whole arrays. For example, to find out if values in the array are greater than 10, we can write

~~~
print(data>10)
~~~
{: .language-python}

~~~
array([[False, False, False,  True, False],
       [False, False, False,  True,  True],
       [False, False, False,  True,  True],
       ...,
       [ True, False, False, False,  True],
       [ True, False, False, False,  True],
       [ True, False, False, False,  True]])
~~~
{: .output}

This will print either `True` or `False` for each array element depending on whether the value of that element is greater than 10 or not.

To get every value in the array that is greater than 10, we can use this as a list of indices we want, or a slice.

~~~
greater_than_10_values = data[data>10]
print(greater_than_10_values)
~~~
{: .language-python}

~~~
[   11.5478    13.9594    11.6945 ...    10.7564 10000.        10.7848]
~~~
{: .output}


## Broadcasting
Another special thing about numpy is something called **broadcasting**. Broadcasting occurs when you attempt mathematical operations on arrays that have different shapes. If possible, the smaller array is "broadcast" across the larger array.

For example, using numpy arrays, you can multiply every element in a numpy array by 10

~~~
print(10*r1_array)
~~~
{: .language-python}

~~~
[0. 1. 0.]
~~~
{: .output}

But, if you multiply our variable `r1` (which is a list), by 10, the list will just be repeated 10 times.

~~~
print(r1*10)
~~~
{: .language-python}

~~~
[0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0]
~~~
{: .output}

The multiplication of `r1_array*10` is an example of what is called *broadcasting* in NumPy. You can think of the scalar `10` being stretched during the operation to be the same size and shape as `r1_array`.

Broadcasting looks at the dimensions of each array, and sees if it can match them. For example, if you attempt array addition with an array that has size `(1000, 4)`, and the other has `(1, 4)`, `numpy` will notice that each has four columns, and *broadcast* the smaller array over the larger one.

Imagine you have molecular coordinates stored in a numpy array, and you'd like to translate the molecule by a particular distance vector. Let's work with our `water.xyz` file in the `data` folder.

~~~
file_location = os.path.join('data', 'water.xyz')
xyz_file = np.genfromtxt(fname=file_location, skip_header=2,  dtype='unicode')
symbols = xyz_file[:,0]
coordinates = (xyz_file[:,1:])
coordinates = coordinates.astype(np.float)

print(coordinates)
~~~
{: .language-python}

~~~
[[ 0.       -0.007156  0.965491]
 [-0.        0.001486 -0.003471]
 [ 0.        0.931026  1.207929]]
~~~
{: .output}

We would like to move our molecule 2 units in the x direction, 1 unit in the 0 direction, and 0 units in the z direction. We can define a translation vector as a numpy array.

~~~
translation_vector = np.array([2,1,0])
~~~
{: .language-python}

If you were working with Python lists, or you didn't know about the features of numpy arrays, you might try to do this with a `for` loop. 

~~~
new_coordinates = []

for atom in coordinates:
    new_x = atom[0] + translation_vector[0]
    new_y = atom[1] + translation_vector[1]
    new_z = atom[2] + translation_vector[2]
    
    new_coordinates.append([new_x, new_y, new_z])
    
print(new_coordinates)
~~~
{: .language-python}

~~~
[[ 2.        0.992844  0.965491]
 [ 2.        1.001486 -0.003471]
 [ 2.        1.931026  1.207929]]
~~~
{: .output}

Broadcasting in `numpy` allows us to achieve that with one command, rather than a `for` loop.

~~~
new_coordinates = coordinates + translation_vector

print(new_coordinates)
~~~
{: .language-python}

~~~
[[ 2.        0.992844  0.965491]
 [ 2.        1.001486 -0.003471]
 [ 2.        1.931026  1.207929]]
~~~
{: .output}

> ## Exercise
> Use the features of numpy arrays to subtract the mean of each column from the column data. Leave the `frame` column unchanged. Save your new array as a variable with the name `deviation`. Recall that we already have a variable with the average of each column (`column_averages`).
>> ## Solution
>>  Using broadcasting, we could subtract this vector from our data array. However, since we want to leave frame unchanged, there is one additional step. Note that if we just slice `column_averages`, broadcasting will not work since the arrays do not have matching dimensions. 
>> 
>> ~~~
>> deviation = data - column_averages[1:]
>> ~~~
>> {: .language-python}
>> ~~~
>> ---------------------------------------------------------------------------
>> ValueError                                Traceback (most recent call last)
>> <ipython-input-78-f0fc06fc540c> in <module>
>> ----> 1 data - column_averages[1:]
>> 
>> ValueError: operands could not be broadcast together with shapes (10000,5) (4,) 
>> ~~~
>> {: .error}
>>
>> Because we don't care about the frame average, we will overwrite it with zero so that we can still use broadcasting.
>> ~~~
>> column_averages[0] = 0
>> deviation = data - column_averages
>> print(deviation)
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

## When can you use broadcasting?
Broadcasting is extremely useful, but there will be times when you cannot use it and will have to use a `for` loop. For example, if the value of something in the loop depends on a previous value that is calculated in the loop.

~~~
my_array = np.array([1, 0, 1])

for i in range(9):
    my_array = my_array + 2
    print(my_array)
~~~
{: .language-python}

~~~
[3 2 3]
[5 4 5]
[7 6 7]
[9 8 9]
[11 10 11]
[13 12 13]
[15 14 15]
[17 16 17]
[19 18 19]
~~~
{: .language-output}

In this example, we might want to save all of the results in an array called `results` which has size `(10, 3)`. If you were working with `lists`, you would have defined an empty list and used the `append` function within the loop. However, we would like to save our results in a numpy array. Often, when you encounter a situation like this you will make an empty or placeholder array of the appropriate size. The function `np.zeros([m,n])` will create an array filled with zeros of size `m rows` x `n columns`.

~~~
results = np.zeros([10, 3])
print('This is my array/n', results)

results[0] = np.array([1, 0, 1])

for i in range(1,10):
    results[i] = results[i-1] + 2

print('I have filled in the array :/n', results)
~~~
{: .language-python}

~~~
This is my array
 [[0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]
 [0. 0. 0.]]
I have filled in the array :
 [[ 1.  0.  1.]
 [ 3.  2.  3.]
 [ 5.  4.  5.]
 [ 7.  6.  7.]
 [ 9.  8.  9.]
 [11. 10. 11.]
 [13. 12. 13.]
 [15. 14. 15.]
 [17. 16. 17.]
 [19. 18. 19.]]
 ~~~
 {: .output}

Here, we have created an array which only has zeros, then filled in values to that array. Notice a few more things about this modification. Our `range` function now goes over `1-10` instead of `9`. This is because we are calculating `results[i]` using `results[i-1]`.

> ## Exercise
> Imagine you are simulating a [random walk](https://en.wikipedia.org/wiki/Random_walk). We will simulate a *two dimensional* random walk. In our random walk, we start with a particle at the origin and it moves either +1, 0, or -1 in each direction randomly for each step. Write a code which achieve this a variable `n_steps` number of steps. Initially set the `n_steps` variable to `100`.
> 
> Record the position at each step in a numpy array called `coordinates` of size `(n_steps, 2)` where columns 0-1 correspond to the `x` position, the `y` position, and the `z` position respectively.
>
> **Hint** - You will need to generate a numpy array with random number for this. Try googling 'numpy random integer' to see if you can find a numpy function to use.
>> ## Hint Solution
>> If you google 'numpy random integer', you will see the function `np.random.randit` - see documentation [here](https://docs.scipy.org/doc/numpy-1.15.0/reference/generated/numpy.random.randint.html?highlight=randint#numpy.random.randint). You can use this function to create arrays which have values from `low` up to, but *not including* the number you set for the `high` argument. The argument `size` should be set to the size we want, which is `[1, 2]`.
> {: .solution}
>
>> ## Solution
>> ~~~
>> n_steps = 100
>> 
>> coordinates = np.zeros([n_steps, 2])
>> 
>> for i in range(1, n_steps):
>>     random_displacement = np.random.randint(-1, 2, size=[1,2])
>>     coordinates[i] = coordinates[i-1] + random_displacement
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

> ## Exercise
> Use `matplotlib` to plot your particle's path.
>> ## Solution
>> ~~~
>> import matplotlib.pyplot as plt
>> 
>> plt.plot(coordinates[:,0], coordinates[:,1])
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}


## Optional - Returning to the geometry analysis project

In your geometry analysis project, you had to analyze an xyz file, find the bonds, and print bond lengths.

We can rewrite that project using the features of numpy arrays.  

Recall that a solution given for a function for calculating distances between two points was the following

~~~
def calculate_distance(rA, rB):
    """Calculate distance between points A and B"""
    x_dist = (rA[0] - rB[0]) ** 2
    y_dist = (rA[1] - rB[1]) ** 2
    z_dist = (rA[2] - rB[2]) ** 2
    
    distance = np.sqrt(x_dist + y_dist + z_dist)
    
    return distance
~~~
{: .language-python}

This function would work for both lists and numpy arrays, because it does not assume that rA and rB can do something like element-wise subtraction. 

> ## Exercise
> Using what you've learned about numpy arrays, rewrite the `calculate_distance` function to use the features of numpy arrays.
>> ## Answer
>> If rA and rB are both numpy arrays, we can use element-wise subtraction. It is also a good idea to update your function docstring to reflect the restriction on the data type of `rA` and `rB`. 
>> ~~~
>>  def calculate_distance(rA, rB):
>>     """Calculate the distance between points A and B. rA and rB must be numpy arrays."""
>>     AB = (rB - rA)**2
>>     distance = np.sqrt(np.sum(AB))
>>     return distance
>> ~~~
>> {: .language-python}
>> You might also have used the `numpy` function `np.linalg.norm` which calculates the magnitude of a given vector.
>> ~~~
>> def calculate_distance(rA, rB):
>>    dist_vec = (rA-rB)
>>    distance = np.linalg.norm(dist_vec)
>>    return distance
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

Redefine your original distance function as `calculate_distance_list`. Using both, we see that both functions give the same answer. 

~~~
print(calculate_distance_list(r1, r2))
print(calculate_distance(r1_array, r2_array))
~~~
{: .language-python}
