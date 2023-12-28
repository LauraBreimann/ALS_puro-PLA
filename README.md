<div align="center">
  
# Documentation for Puro-PLA analysis in neurons

</div>

### Pipeline

* _**1.	Tiff file extraction from lif files**_
* _**2.	Mask creation using Ilastik**_
* _**3.	Mask refinement and area detection using Fiji**_
* _**4.	Puro-PLA analysis using RS-FISH**_
* _**5.	Mask filtering using RS-FISH**_
* _**6.	Generating overlay images of the detections**_
* _**7.	Colletion of all datasets and plotting**_

<br />

<div style="text-align: justify">
  
 ### 1.	Tiff file extraction from .lif files
  
Use the [Fiji](https://fiji.sc/) macro ```open_lif_split_gfp_max.ijm``` script to open a set of .lif files, split the channels and resave them as single tiff files. The script also renames the files, for this, the script exprects these channels in this order: 1. DAPI, 2. Puro-PLA, 3. Map2-GFP. The script also saves a max projection of the GFP channel, which may be used to create a binary mask.
  
  
 ### 2.	Mask creation using Ilastik
  
[Ilastik](https://www.ilastik.org/) can be used to create a binary mask to filter the Puro-PLA detections. Since neurons are pretty flat, mask are creeated from max projections of the GFP  channel. The ```Autocontex``` workflow uses pixel classification to segement the cell boundaries. For this cells are segemented in two rounds. After innitial training, similar images can be processed together in the batch mode. To export the masks, select the ```Simple segmentation``` output format and tiff as file format. 
    
  <img src="https://github.com/LauraBreimann/neuron_Puro-PLA_quantification/blob/main/screenshots/C1-Composite_22.jpg" alt="Max projection of the Map2-GFP channel" width="400">
  
   <img src="https://github.com/LauraBreimann/neuron_Puro-PLA_quantification/blob/main/screenshots/Act_Puro647_Map2_488_scr_ctrl_scr22_GFP_max_ilastik_raw_mask.jpg" alt="Binary mask after ilastik segmentation" width="400">
  
   
  
 ### 3.	Mask refinement and area detection using Fiji
  
Use the ```creating_mask.ijm``` to create binary masks from the Ilastik segmentation. Using the paintbrush tool, the segmentation can be corrected to remove small background signals or neighbouring cells by coloring them in black (with a value of 0). Small corrections on the masks can be perfromed using white (with a value of 1).  
  
  <img src="https://github.com/LauraBreimann/neuron_Puro-PLA_quantification/blob/main/screenshots/Act_Puro647_Map2_488_scr_ctrl_scr22.jpg" alt="Binary mask of 70 µm of the main neurites" width="400">
  

  
 ### 4.	Puro-PLA analysis using RS-FISH
  
One optional step is to prepare the Puro-PLA images using a difference of Gaussian filer using the macro script ```DoG_filter.ijm```. The sigma for the Gaussian blur needs to be adapted for each new set of images. 
  
To detect Puro-PLA spots in the image, the Fiji plugin [RS-FISH](https://github.com/PreibischLab/RS-FISH) can be used (info on how to use RS-FISH and how to download the plugin can be found on the [RS-FISH](https://github.com/PreibischLab/RS-FISH) GitHub page). The macro ```RS-FISH_macro.ijm``` can be used to run RS-FISH in batch mode. Determine the parameters before using one representative image and then run the rest of the images in batch mode. 
 
 
  <img src="https://github.com/LauraBreimann/neuron_Puro-PLA_quantification/blob/main/screenshots/Puro-PLA.png" alt="Image of Puro-PLA signal" width="450"> 
 
 <img src="https://github.com/LauraBreimann/neuron_Puro-PLA_quantification/blob/main/screenshots/RS-FISH_detection.png" alt="Image of RS-FISH detection of the Puro-PLA signal" width="450"> 
 
 <img src="https://github.com/LauraBreimann/neuron_Puro-PLA_quantification/blob/main/screenshots/Puro-PLA_detected.png" alt="Image of Puro-PLA signal detected by RS-FISH" width="450"> 
 
  
  
 ### 5.	Mask filtering using RS-FISH
  
  With the created mask, the resulting csv files can be filtered using the [RS-FISH](https://github.com/PreibischLab/RS-FISH) plugin "Mask filtering". Go to ```Plugins > RS-FISH > Tools > Mask filtering```. 
  
 
  <img src="https://github.com/LauraBreimann/smFISH_neuron_analysis/blob/main/screenshots/mask_filtering_plugin.png" alt="Screenshot of the mask filtering plugin" width="700">
  


  
 ### 6.	Generating overlay images of the detections
  
 To show the RS-FISH detections on the images one can use the visualization tools within [RS-FISH](https://github.com/PreibischLab/RS-FISH) using Fiji or the BigDataViewer. For 2D images the macro ```Overlay_generator.ijm``` can be used to show detections easily. It will also add the detections to the ROI manager so they can be further manipulated in shape, size and color. 
  
 
  <img src="https://github.com/LauraBreimann/neuron_Puro-PLA_quantification/blob/main/screenshots/no_detections.png" alt="Screenshot of the composite image showing no detections" width="450">
  
 <img src="https://github.com/LauraBreimann/neuron_Puro-PLA_quantification/blob/main/screenshots/full_overlay.png" alt="Screenshot of the composite image showing RS-FISH detections filtered by the neurite mask" width="450">
    
 <img src="https://github.com/LauraBreimann/neuron_Puro-PLA_quantification/blob/main/screenshots/just_detections.png" alt="Screenshot of just the detections filtered by the neurite mask" width="450">
  

  
 ### 7.	Colletion of all datasets and plotting
  
To detect the size of the created masks run the ```record_size_of_mask.ijm``` to create csv files for each binary mask containing an area measurement. Make sure that files are binary files with values of 0 and 1 and that all have set the same scale. 
  
  To create a dataset for plotting, this python script searches all the files and combines them, but this might be dependen on the file structure. 
  
  ```Collect_counts_and_mask_sizes.ipynb```
  
  To plot from the created database: ```Plot_Puro-PLA_values.ipynb```
