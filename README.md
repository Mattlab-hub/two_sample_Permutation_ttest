# 2sample_Permutation_ttest
This is a python function that runs a 2 sample permutation t-test. It takes 2 sets of data of data up to 4 dimensional arrays. The first dimension in both sets needs to be the observations. This function is capable of handling nan values, excluding them and adjusting the n size appropriately per each t-test ran. 

To use, enter: from permutation import perm      

Args:   
   
   X: first data set where the first dimension is the observations.
   
   Y: second data set where the first dimension is the observations.
   
   
   perms:  the number of times to permute the data, typically 1000 or greater.
   
   alpha: significance value for the probability of rejecting the null hypothesis when it is true. Typically set at 0.05.

Returns: 
   
   Global: array of T values where the observed T values were larger than 1-alpha% of all the null permuted data. Best for data that is 1D or 2D unless you want separate comparisons for each index. 
   
   Part: array of T values where the observed T value was larger than the permuted T distributions in the same index point of the array. Very conservative, observed values must be greater than All permuted values in the same index point.
   
   P_array: array showing p values where the observed t values exceeded 1-alpha% of the permuted values at each index point of a multidimensional array. Typical permutation test output, best for multidimensional data where index matters. 
   
   null_Tmax: The single T value from the permuted null distribution that is above 1-alpha 
   
   t_score: array of T values from the observed data.
   
   nullT: list of arrays of T values from the permuted data.
   
   max_null: identify outliers in null permuted data after ttest
   
   max_obs: identify outliers in observed data after ttest

	The P_array output gives back an array with the same dimensions of your original data in P values, minus the observations dimension which is what the T test is performed across. For each index in all dimensions, it divides the number of permuted null T values that were greater-than-or-equal-to your observed T value by the total number of permutations ran (typically 1000+). This fraction is the P value of your observed T value at that time/space/dimension point. Doing this for all time/space/dimension points gives you a multidimensional distribution of significance values, so you can see if you have clusters of significance in time/space or other dimensions. Any P value less than alpha is reported back as a nan value to make viewing significance clusters easier. 
	The Part output also gives back an array with the same dimensions of your original data (minus the observation dimension), but still as T values. It finds a maximum null T value from the permuted data for each index in all dimensions. Then, only observed T values greater than their indexed maximum null T value get reported. Otherwise, a nan value is put into that index as a place holder. This output is thus slightly more conservative than the P_array output depending on your alpha value.
	The Global output also gives back an array with the same dimensions of the original data (minus the observations dimension) and also gives back T values. The Global output first finds the 1-alpha value for the whole permuted null distribution of T values. Thus, if alpha = 0.05, then this output finds the null permuted T value that is in the 95th percentile of the entire dataset regardless of index. Then, only observed T values that are greater than it get reported. Otherwise, a nan value is put into that index as a place holder. This output can be interpreted to be more or less conservative of a measurement depending on the dimensions of the data. It is more in line with traditional permutation tests but may be inappropriate for data with several non-collapsible dimensions that one would rather bin instead. 
	Further, the null_Tmax, t_score, nullT, max_obs, and max_null outputs are provided as well. The null_Tmax is the T value at 1-alpha used in the Global output. The t_score is the array of T values from performing T-tests at each dimensional index of the observed data prior to permutations. The nullT is a list of arrays where each list entry is the array of T values from each permutation. The t_score and nullT outputs are provided if the end user wants to perform more analyses or check the T values. The max_obs is the largest T value in the entire observed t_score array. The max_null is the largest T value in the entire nullT list of arrays. These last two values are provided as checks. If the user finds they receive strangely high T values in either of these last two outputs then it could be a sign that something is wrong with the input data.
