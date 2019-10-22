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

We will start with reading in some data from an xyz file. The following block will read a file called `water.xyz` (from the Python Data and Scripting lesson) and saving two numpy arrays - one called `coordinates` with the molecular coordinates, and another called `symbols` with the element symbols.

~~~
file_location = os.path.join('data', 'water.xyz')
xyz_file = np.genfromtxt(fname=file_location, skip_header=2, dtype='unicode')
symbols = xyz_file[:,0]
coordinates = (xyz_file[:,1:])
coordinates = coordinates.astype(np.float)

print(symbols)
print(coordinates)
~~~
{: .language-python}

~~~
['O' 'H1' 'H2']
[[ 0.       -0.007156  0.965491]
 [-0.        0.001486 -0.003471]
 [ 0.        0.931026  1.207929]]
~~~
{: .output}

> ## Exercise
> Slice the `coordinates` array to create a new array called
> `oxygen_coord` which has the x, y, and z coordinate for the 
> oxygen atom.
>> ## Solution
>> For this, we want all of the columns of the first row.
>> ~~~
>> oxygen_coord = coordinates[0]
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

Now that we have the oxygen coordinate, let's imagine we wanted to do something to it. Let's imagine that we wanted to translate the position of the oxygen atom. We want to translate it 0.1 units in the x direction and -0.1 units in the y direction.

If we were writing for loops like we did before, we might do this by defining a translation vector and using a for loop.

~~~
translation_vector = [0.1, -0.1, 0]

oxygen_coord_new = []

for dim in range(3):
    new_position = oxygen_coord[dim] + translation_vector[dim]
    oxygen_coord_new.append(new_position)

print(oxygen_coord_new)
~~~
{: .language-python}

~~~
[0.1, -0.107156, 0.965491]
~~~
{: .output}

However, since `oxygen_coord` is a numpy array, we could have done this differently. One way numpy arrays and lists are different is that you can easily perform element-wise operations on numpy arrays without loops. You can make your code much faster if you use numpy element-by-element operations instead of loops. 

Numpy is smart. If two arrays (or a list and an array), it will guess that you want to do element-wise addition. In the `for` loop we just wrote, we actually wanted an answer that looked like

`[x1 + x2, y1+ y2, z1+z2]`

where [x1, x2, x3] was `oxygen_coord` and [y1, y2, y3] was `translation_vector`. Using the power of numpy arrays, we could have instead written

~~~
oxygen_coord_new = oxygen_coord + translation_vector
print(oxygen_coord_new)
~~~
{: .language-python}

~~~
[ 0.1      -0.107156  0.965491]
~~~
{: .output}

Numpy was smart - it looked at the shape of both of these variables, saw they were the same shape, and assumed we wanted to do element-wise operation. You could have also subtracted, multiplied, or divided these, and it would have performed element-wise operations.

Note, that this only worked because `oxygen_coord` was a `numpy` array.

~~~
type(oxygen_coord)
~~~
{: .language-python}

~~~
numpy.ndarray
~~~

What happens if we try this with a list?

~~~
oxygen_list = list(oxygen_coord)
type(oxygen_list)
~~~
{: .language-python}

~~~
list
~~~
{: .output}

~~~
oxygen_list + translation_vector
~~~
{: .language-python}

~~~
[0.0, -0.007156, 0.965491, 0.1, -0.1, 0.0]
~~~
{: .output}

As a note, if you wanted to concatenate the two where `oxygen_coordinate` was a numpy array, you could have done so with the `np.concatenate` function.

~~~
# To concatenate numpy arrays...

np.concatenate((oxygen_coord, translation_vector))
~~~
{: .language-python}

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

## Broadcasting
Another special thing about numpy is something called **broadcasting**. Broadcasting occurs when you attempt mathematical operations on arrays that have different shapes. If possible, the smaller array is "broadcast" across the larger array.

Let's think about what would happen if we wanted to move every atom in our water molecule by our translation vector.

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
[[0.1, -0.107156, 0.965491], [0.1, -0.098514, -0.003471], [0.1, 0.831026, 1.207929]]
~~~
{: .output}

Broadcasting in `numpy` allows us to achieve that with one command, rather than a `for` loop.

~~~
new_coordinates = coordinates + translation_vector

print(new_coordinates)
~~~
{: .language-python}

~~~
[[ 0.1      -0.107156  0.965491]
 [ 0.1      -0.098514 -0.003471]
 [ 0.1       0.831026  1.207929]]
~~~
{: .output}

For this to work, we have to have two arrays that have a matching dimension. You can see the shape of an array using the function `np.shape`.

~~~
np.shape(coordinates)
~~~
{: .language-python}

~~~
(3, 3)
~~~
{: .output}

~~~
np.shape(translation_vector)
~~~
{: .language-python}

~~~
(3,)
~~~
{: .output}

When you typed, `coordinates + translation_vector`, numpy looked at the shapes of both arrays to figure out if they were compatible. 

It starts with the dimensions to the right, so when it saw to matching 3's it assumed you wanted to do element-wise operation this way, and stretched or 'broadcast' the smaller array to match the larger one.

~~~
row_translate = [[0.1], [0.2], [0.3]]
print(np.shape(x_translate))
~~~
{: .language-python}

~~~
(3, 1)
~~~
{: .output}

Think about what will happen if you try this code

~~~
coordinates + row_translate
~~~

## Logical comparisons

We can also do logical comparisons on whole arrays. For example, to find out if values in the array are greater than 0, we can write

~~~
print(coordinates > 0)
~~~
{: .language-python}

This will print either `True` or `False` for each array element depending on whether the value of that element is greater than 10 or not.

~~~
array([[False, False,  True],
       [False,  True, False],
       [False,  True,  True]])
~~~
{: .output}

To get every value in the array that is greater than 10, we can use this as a list of indices we want, or a slice.

~~~
greater_than_0_values = coordinates[coordinates>0]
print(greater_than_0_values)
~~~
{: .language-python}

~~~
[0.965491 0.001486 0.931026 1.207929]
~~~
{: .output}

## Array Axes

Imagine we wanted to calculate the geometric center of our molecule. To do this, we would need to get the average x coordinate, the average y coordinate, and the average z coordinate.

In a previous lesson, we calculated the mean of each column of an array using the `range` function and a `for` loop. This was good because it reminded us of the `range` function and how `for` loops worked. However, the `numpy.mean` function will let us do that without a `for` loop.

Our code for that would look something like this:

~~~
center = []

for dim in range(len(symbols)):
    dim_mean = np.mean(coordinates[:, dim])
    center.append(dim_mean)
~~~

A `numpy` array can be thought of like a coordinates system. Axis 0 runs along the ROWS, while axis 1 runs along the COLUMNS.

> ## Exercise
> Check out the [numpy documentation](https://docs.scipy.org/doc/numpy/reference/generated/numpy.mean.html) for the mean function. Can you figure out what argument we might change to get the mean of each column?
>> ## Solution
>> We will use the `axis` argument. If we do not specify `axis`, the mean of the entire array is computed. 
>> 
>> A 2-dimensional array has two corresponding axes: the first running vertically downwards across rows (axis 0), and the second running horizontally across columns (axis 1).
>>
>> For the `axis` argument, rows correspond to axis 1, and columns correspond to axis 0.
> {: .solution}
{: .challenge}

~~~
center = np.mean(data, axis=0)
print(center)
~~~
{: .language-python}

~~~
[0.         0.308452   0.72331633]
~~~
{: .output}

Now, the mean function has returned a `numpy` array to us with the same number of elements as we have columns. The 0th element of the variable `column_averages` corresponds the mean of column index 0, the second number (index 1) corresponds to the mean of column index 1. 


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
