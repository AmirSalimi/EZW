# EZW
Implementation of EZW image compression algorithm
EZW Encoding
Although the discrete wavelet transform is interesting, it is not the focus of this report.  The actual algorithm for EZW encoding is conceptually simple.  It employs an initialization step, a dominant pass, and a refinement pass.  In the initialization step, we must first find our initial threshold to determine which coefficients are the most important.  The threshold is found to be , where cmax is simply the absolute value of the largest coefficient.  At this point we begin our first iteration with the dominant pass.  Here, we apply one of four labels to each coefficient as seen in Table 1.

Symbol
Label
Description
SP
Significant Positive
Assigned to coefficients whose values (and the values of their subordinates) fall above the threshold.
SN
Significant Negative
Assigned to negative coefficients whose absolute values (and the absolute values of their subordinates) fall above the threshold.
ZR
Zerotree Root
Assigned to coefficients whose values (and the values of their subordinates) fall below the threshold.
IZ
Isolated Zero
Assigned to coefficients whose values are below the threshold and if any of its subordinates are above the threshold.
Table 1
The coefficients which are compared to the threshold are scanned in the following manner.  The highest level coefficients are all scanned in a raster scan order.  In the case for the 512x512 image, the number of decomposition levels was nine so that there were only 4 coefficients.  Depending on how the coefficients are labeled, we scan the subsequent lower level coefficients.  If a coefficient is labeled as significant or as an isolated zero, we then scan the four coefficients corresponding to it and label them accordingly.  This completes the dominant pass.  Afterwards, we use a refinement pass to get a better estimate of our reconstructed coefficients.

A detailed breakdown of the algorithm can be seen below:

1.	Initialization:
a.	Compute wavelet transform
b.	Find the threshold as specified above
2.	Dominant Pass:
a.	Coefficients are scanned (parents first, then offspring), compared to the threshold and assigned one of the four tags mentioned in Table 1. 
b.	Tags are converted to binary and sent.
c.	Coefficients determined to be significant are reconstructed to ±1.5To and placed in a refinement list.
3.	Refinement Pass:
a.	The actual and reconstructed values are compared and adjusted by ±T/4.  Then ones and zeros are sent to specify the sign of the adjustment value.
b.	The threshold is adjusted by dividing it by two.
4.	Repeat:
a.	Go back to step two if desired.
