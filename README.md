# "Ah-it-makes-sense" tricks for coding beginners like me

This is a notepad to record my (perhaps) strange coding needs and tricks that meet the needs with "ah yes, it makes sense".

## 1. How to generate a batch of regular variables in python?
__Q.__ I need to define a series of variables like a_1=[], a_2=[], a_3=[], ... , a_n=[] and they have various length and index.

__A.__ Use `exec()` which executes strings as executable code

````python   
# Let's define 5 empty lists and name them a_1, a_2, a_3, a_4, a_5
for i in range(1,6):
    exec('a_'+str(i)+'=[]')
````

## 2. How to create an array to record a huge amount of boolean values (True/1 and False/0)?
__Q.__ I need to create an array of size 25000\*700\*700 to record boolean values; and `np.zeros((25000, 700, 700))` is warned with the memory error of `Unable to allocate 91.3 GiB for an array with shape (25000, 700, 700) and data type float64`.

__A.__ Change the data type to `bool` will save the memory significantly

````python
numpy.zeros((25000, 700, 700), dtype=bool)
# the memory size now is around 12.25 GB -- still large but at least accessible
````
__Update:__ Try to record only the indices if the matrix will be sparse.

## 3. How to save a large boolean array (where the True values are sparse) with numpy?
__Q.__ I have a large boolean array (the one created in question 2 and processed further), for example, of size 25000\*700\*700, which is expensive to be saved as a csv file because the required memory (after reshaping into 2D) is soooo large and the the required time is sooooo long.

__A.__ Save the indices of the True values (the csv file of indices occupies only 4.8 MBs in my case!)

````python
indices = np.argwhere(true_or_false_array)
# When I need to recover the original true_or_false_array
recovered_array = np.zeros((25000, 700, 700), dtype=bool)
recovered_array[indices[:,0], indices[:,1], indices[:,2]] = True
````

## 4. How to check if a sequence (1 times n) is in an array (m times n)?
__Q.__ I have a sequence S of size 1×n and an array S_set of size m×n, and I need to check if S is in S_set.

__A.__ Rather than comparing S with rows in S_set one by one, we can utilize logical operators offered by numpy. Step 1, we create an array consisting of m rows of S; step 2, with np.equal() and np.all(), we compare the new array with S_set to find out if corresponding rows are the same; step 3, if step 2 find any equal rows, S is in S_set.

````python
is_S_in_S_set = np.any(np.all(np.equal(np.array([S,]*S_set.shape[0]), S_set), axis=1))
````

__Update:__ Another concise way for Python lists can be

````python
is_S_in_S_set = np.any([S == x for x in S_set])
````
--to be continued--
