# magPI_anglesweep_analysis

This is documentation for using the magPI vector sensing analysis pipeline on github at: https://github.com/Isaac-MW-Shelby/magPI_anglesweep_analysis

It is designed to work for data taken of a magnetic particle over a sweep of a rotating external applied field with a static splitting field.  

It assumes as inputs: 

A single .mat file with all the resonance information for the entire angle sweep (output from (insert code name here)). 

the anglesweep .mat file should cointain: a 1D list of the magnetic tweezer (MT) angles (in degrees), a 4D array with the (sorted) locations 
of all 8 resonances (2 for each of the 4 NV orientations) at each pixel, at each angle. 

The array is in: MT angle by x index by y index by resonance location. 

Formating should be: ‘anglesweep_WFfullresonancemaps_****.mat’ where **** is the identifying information, i.e. 020822_e1421_r1 
(region 1 on the 1421 sample from date 020822)

The .mat “matrices” files for each step in the scan (needed for the precisions, can remove if not available).

Formatting should be: ‘fullODMRspectrum_matrices_newfitter_magangle***.mat’ where *** is the angle of the tweezers, i.e. 261 or 176point4. 

You can modify the code to not require these additional matrices by changing convert_sensitivities_to_precisions in convert_resonane_maps_to_B_fields
from true to false. (will instead assume a flat precision of 0.6 uT, experimentally determined from empty FOVs). 


Start by going to the folder that contains the data files and run ‘convert_resonance_maps_to_B_fields.m’

This will generate a parent folder that it will store the data files and the fit files.
The parent folder will be named just the identifying information from the anglesweep format, and will contain the following: 

The “files_for_analysis_****” folder (which contains the files)
1) A file titled ‘B_field_maps.mat’ this file contains the generated b field maps from the resonance data (and other relevant outputs)
2) A folder called “figures_from_convert_resonance_maps_to_B_fields” that contains subfolders labeled by filetype. Each folder contains a file 
   of every figure generated by the convert_resonance_maps_to_B_fields script in the respective format. 


Next, from the parent folder generated by ‘convert_resonance_maps_to_B_fields.m’ , 
run the desired section of ‘fitting_bead_parameters_from_anglesweep_wrapper.m’ 
(it can do a single angle, a full angle sweep, or a selected subset of angles). 
This will take ~5 minutes per b field map that is being fit. (hopefully will be optimized for faster eventually, already have some thoughts)

This will generate the following under the parent folder: 
1) A folder titled ‘bead_parameter_fit_figures’. It will contain subfolders labeled by filetype. Each of those contains saves of every 
   figure generated by the ‘fitting_bead_parameters_from_b_fields_wrapper’ function, in the respective filetype.
2) A folder titled ‘bead_parameter_fit_outputs’ which contains the .mat file outputs of the ‘fitting_bead_parameters_from_b_fields_wrapper’ function. 

Finally, still in the parent folder, run ‘analysis_and_figures_for_full_anglesweep’ script

This will generate

1) a .mat file in the top level folder labeled ‘full_angle_sweep_bead_fit_data’
2) a folder labeled ‘figures_from_full_anglesweep’ with subfolders labeled by filetype. 
   Each of those contains saves of every figure generated by ‘analysis_and_figures_for_full_anglesweep’, in the respective filetype.


Lists of figures generated

They are named to not necessarily need this but here are the files generated and the explanation of what they are:

IDENTIFIER: the **** string from ‘anglesweep_WFfullresonancemaps_****.mat’ that contains identifying information from the data set

under figures_from_convert_resonance_maps_to_B_fields: 

IDENTIFIER_bead_field_components_MT_angle_#: the Bx, By, Bz, and magnitude of the FOV (mean subtracted) at MT angle #

IDENTIFIER_mean_resonance_shifts_vs_tweezer_magnetic_field_angle: plot of mean resonance shifts at each MT angle. generates by subtracting pairs of the 
resonance location: 1 2 3 4 5 6 7 8: (8-1), (7-2), (6-3), (5-4). Note that these are absolute value of zeeman shifts because we don't have which of the 
two resonaces is the + or the -. 

IDENTIFIER_fit_mean_applied_field_shifts_and_raw_data: splittings generated by the fit applied magnetic field. assumes a static splitting field that is
roughly along the [1 1 1] axis of the diamond, a rotating magnetic tweezer in the plane of the diamond of fixed magnitude, and a small oscillation in the
component perpendicular to the diamond due to offset of the magnetic tweezer rotation axis from the center of the FOV. Note that the only important readout
from this fit is the order of the 4 resonances, not 

IDENTIFIER_mean_field_components_vs_twezzer_angle: plots the mean field components (x,y,z) vs tweezer angle and magnitude

under bead_parameter_fit_figures

IDENTIFIER_measured_fit_residual_bead_fields_for_#: measured, fit, and residual of fit for bead fields at MT angle #

under figures_from_full_anglesweep




