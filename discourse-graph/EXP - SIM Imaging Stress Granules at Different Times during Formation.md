---
nodeTypeId: node_zF94G4VvOsvL2sQYcaf3N
nodeInstanceId: 019ea6d6-d619-7557-a5d3-c83bbe410b4b
---
## Progress & Notes
- `2025_08_23`: Kate's student visited Montreal and they sent their cells head of time. Khalid is maintaining the cell culture using their protocol. (See linked protocol node.) This cell line has `PROTEIN 1` tagged with GFP and we will transfect it with a plasmid expressing `PROTEIN 2`-mKate. Applied the stress to the cells and imaged them on the microscope to measure the rate/amount/coordination of the two proteins forming stress granules. 
- `2025_10_14`: I have developed the segmentation and subsequent image analysis pipeline. This has yielded initial results (attached). There were several design choices which had to be made including: 
	- What channel to use for the segmentation, or whether to use the SUM or the MAX (pixel-wise) for the segmentation. 
	- What intensity threshold to use to exclude local maxima which are due to background noise? (This is a parameter of the `ijm` macro and was set by choosing a value that is just above the background max intensity.)
# Hypothesis
We have multiple hypotheses that are addressed by this experiment: 
1. Stress granule components begin to form droplets independently of each other, and these components later mix into the same droplet to give the SGs we typically observe. 
2. This process will be perturbed when we change the type of stress because different types of stress have different sensors which trigger the SG formation mechanism. The different nature of the formation (with respect to the two tagged proteins) will give us insight into how these two proteins are related to the downstream effects of the stress sensing mechanism which lead to the droplet formation. 
