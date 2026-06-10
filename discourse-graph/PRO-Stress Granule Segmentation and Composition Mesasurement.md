---
nodeTypeId: node__1xAFVzzF5E8z9R3MXFPN
nodeInstanceId: 019ea6d8-1361-781d-8f37-2c7ca3503ec2
---
# Segmentation and Measurement Protocol
1. Crop individual cells into their own 3D stack `tif` images using Fiji from the processed sim data. 
2. For each `tif` of a cell, do the following: run the `segment_stress_granules.ijm` ImageJ macro to generate a label image which segments the stress granules in the fluorescent image. This is done using the 3D Suite plugin to identify local signal maxima and then fit a 3D gaussian to use a FWHM threshold for each object (stress granule). Because there are two signal channels, this was done on the per-pixel SUM of the channels to prevent bias toward segmenting objects which have more or less of a single component. 
3. Then in python (see python script in data). The fluorescent `tif` is loaded and the label image `tif` is loaded, and the `regionprops` function is used to compute the statistics for each color channel on each spot. 

# Analysis and Statistics Protocol
Each cell was binned into time bins based on how long it has been since the stress was applied. 