---
nodeTypeId: node__1xAFVzzF5E8z9R3MXFPN
nodeInstanceId: 019ea6d7-a227-7e6a-a18f-547b025e9879
---
# Protocol
1. Cultured cells were adhered to a coverslip and placed in a well containing 1µl of the media they were grown in. This cell line was tagged with endogenous `PROTEIN 1`-GFP and was transfected with a plasmid expressing `PROTEIN 2`-mKate. 
2. As quickly as possible: 
	1. The media was removed from the well with a pipette. 
	2. New media containing the stress (Sodium Arsenite or osmotic stress) was added using a pipette. Do not pipette directly onto the coverslip. Deposit the new media on the side of the well to avoid disturbing the cells or applying any mechanical stress. 
	3. Start a stopwatch to calculate the amount of time from the since the stress was applied. 
3. Place the cells on the Elyra 7 stage and shut the enclosure in preparation for imaging. The time from the exchange of the media to the first image acquisition should be as short as possible to observe the stress response in its early stages. 
4. Scan over the cells using the bright-field to identify cells which are healthy and which are close enough together that you can get a good amount (purely to increase the data acquisition efficiency). You can position the center of the z-stack in the bright-field to avoid using the laser and initiating any photobleaching so that we can quantitatively compare the intensities in the analysis step. 
5. Define the "center" of the z-stack in the ZEN software controlling the microscope. 
6. Switch to the Lattice SIM acquisition track and acquire a z-stack using the parameters outlined below. Note the time since the stress was applied using your stopwatch and include this time in the filename for the raw image.
7. Move to a NEW field of view so we have fresh cells which have never been exposed to the laser and repeat the imaging steps above (4-6). Repeat this until you have imaged for 30 minutes after the stress application. 
8. Repeat steps 1-7 with a new coverslip in a new well of cultured cells to generate replicates. 
9. Process the RAW image files using batch mode in ZEN to perform the SIM deconvolution and generate the super-resolution images. Use the parameters in the table below. Use the default parameters otherwise.

## Imaging Parameters

| Parameter            | Value  |
| -------------------- | ------ |
| Number of Z slices.  | 25     |
| Z slice spacing      | 300 nm |
| 488 nm laser power   | 0.8    |
| 561 nm laser power   | 0.8    |
| Exposure Time        | 30 ms  |
| Number of SIM Phases | 13     |
| Use Z-piazo          | Yes    |
## Processing Parameters

| Parameter           | Value    |
| ------------------- | -------- |
| SIM Squared         | No       |
| Leap Mode           | 3D Leap  |
| Processing Strength | Weak     |
| Scale To Raw        | Yes      |
| Gaussian Fit Method | Fast Fit |
